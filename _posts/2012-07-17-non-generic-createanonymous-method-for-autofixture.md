---
ID: 625
post_title: >
  Non-generic CreateAnonymous method for
  AutoFixture
author: Thomas Ardal
post_date: 2012-07-17 06:50:54
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/non-generic-createanonymous-method-for-autofixture/
published: true
---
I sometimes (not often) find myself needing a non-generic version of the CreateAnonymous extension-method for <a href="http://autofixture.codeplex.com/" target="_blank">AutoFixture</a>. Usually when needing a test value, you do something like this:

[csharp]
 int someInt = new Fixture().CreateAnonymous&lt;int&gt;();
[/csharp]

The code works for 99% of all tests. Now and then I write reflection based tests, testing conventions like:
<ul>
	<li>All methods start with a capital letter.</li>
	<li>All actions on ASP.NET MVC controllers specify either an HttpPost or HttpGet attribute.</li>
	<li>No public methods specify out parameters.</li>
</ul>
The above examples are all fictional and don't represent tests I've actually written. The point is, tests assert the existing code base as well as the code added in the future.

These "self-extending" tests sometimes need test data for input arguments. I've simply loved AutoFixture from the second I started using it, which is why I always use this framework to build test data. The CreateAnonymous method takes a generic argument, which is hard to specify when not known at compile-time. In fact, the simplest possible example of doing this with AutoFixture would look something like this:

[csharp]
 var fixture = new Fixture();
 var buildMethod = typeof(Fixture).GetMethod(&quot;Build&quot;);
 var buildGenericMethod = buildMethod.MakeGenericMethod(typeof(int));
 var composer = buildGenericMethod.Invoke(fixture, new object[0]) as ISpecimenBuilderComposer;
 var create = typeof(SpecimenFactory).GetMethods().Where(m =&gt; m.Name.Equals(&quot;CreateAnonymous&quot;)).ToList()[1];
 var gen = create.MakeGenericMethod(type);
 var instance = gen.Invoke(typeof(SpecimenFactory), new object[] { composer });
 [/csharp]

Ugly right? In exactly this case, a non-generic overload of CreateAnonymous would be fantastic for avoiding those nasty reflection-based code lines in your test-project. Unfortunately, Mark Seemann and Co. didn't provide us with such an overload, but Mark was luckily a great help in finding a <a href="http://autofixture.codeplex.com/workitem/4229" target="_blank">solution to the problem</a>. A non-generic overload of CreateAnonymous can be written as a new extension method for Fixture as simply as this:

[csharp]
 public static class AutoFixtureExtensions
 {
   public static object CreateAnonymous(this Fixture fixture, Type type)
   {
     var context = new SpecimenContext(fixture.Compose());
     return context.Resolve(type);
   }
 }
[/csharp]

This makes it possible to write a test containing code like this:

[csharp]
 int someInt = new Fixture().CreateAnonymous(typeof(int));
[/csharp]