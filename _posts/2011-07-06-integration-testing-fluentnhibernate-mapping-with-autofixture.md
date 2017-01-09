---
ID: 337
post_title: >
  Integration testing FluentNHibernate
  mapping with AutoFixture
author: Thomas Ardal
post_date: 2011-07-06 08:02:19
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/integration-testing-fluentnhibernate-mapping-with-autofixture/
published: true
---
I'm usually writing about unit testing but hey, you want to test your lower layers as well. If you design your application with low coupling and test in mind, all of your layers but the data layer, can be tested with unit testing. You need to write some integration tests of your <a href="http://en.wikipedia.org/wiki/Object-relational_mapping" target="_blank">ORM</a> layer though, in order to test your object to database mappings.

At my workplace we use <a href="http://fluentnhibernate.org/">FluentNHibernate</a> and are pretty happy with it. I won't go into detail on how the tool works, but in short it is an alternative to hbm.xml and ActiveRecord mapping entities in NHibernate. All the mapping stuff goes into a separate class, which makes the entity itself clean and simple. When doing mapping code, you always want to test it against a real database (possible in-memory), doing all of the <a href="http://en.wikipedia.org/wiki/Create,_read,_update_and_delete" target="_blank">CRUD</a> operations. If you have a lot of entities, writing all these tests can be a time consuming and tedious task. Luckily FluentNHibernate provides us with the <a href="http://wiki.fluentnhibernate.org/Persistence_specification_testing" target="_blank">Persistence specification testing</a> framework, which makes testing the mapping code easy (source code from the FluentNHibernate site):

[csharp]
[Test]
public void CanCorrectlyMapEmployee()
{
    new PersistenceSpecification&lt;Employee&gt;(session)
        .CheckProperty(c =&gt; c.Id, 1)
        .CheckProperty(c =&gt; c.FirstName, &quot;John&quot;)
        .CheckProperty(c =&gt; c.LastName, &quot;Doe&quot;)
        .VerifyTheMappings();
}
[/csharp]

Pretty sleek, right? The code creates a new PersistenceSpecification with a generic type of the entity I want to test. For each property, call the CheckProperty method on the fluent API, assigning a test value and finally call VerifyTheMappings, which executes the basic CRUD operations. I used this approach during some time and found it usable but required some maintenance, each time the entities grew with more properties. <a href="http://twitter.com/#!/chripede">@chripede</a> and yours truly set out to find a better solution.

We ended up using <a href="http://autofixture.codeplex.com/" target="_blank">AutoFixture</a> by <a href="http://twitter.com/#!/ploeh" target="_blank">@ploeh</a> to create maintainable tests. We already used AutoFixture to generate test values for all of our unit tests, but found the tool to work extremely well in conjunction with the Persistence specification testing methods. The above test were refactored to look something like this:

[csharp]
[Test]
public void CanCorrectlyMapEmployee()
{
    var fixture = new Fixture();
    new PersistenceSpecification&lt;Employee&gt;(session)
        .VerifyTheMappings(fixture.CreateAnonymous&lt;Employee&gt;());
}
[/csharp]

AutoFixture generates a new Employee object, filling out all the properties with test values. In the test we don't care if FirstName is set to "John" or some random string. The important part is, that the property can be written to and fetched from the database. So why is the second test method cooler than the first? When adding new properties to the Employee entity, our CanCorrectlyMapEmployee test automatically tests the new property, because AutoFixture uses reflection to fill the entity with data.