---
ID: 394
post_title: >
  Using AutoMoqCustomization with NUnit,
  Moq and AutoFixture
author: Thomas Ardal
post_date: 2011-10-04 18:56:57
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/using-automoqcustomization-with-nunit-moq-and-autofixture/
published: true
---
I've used mock as long as I remember (pretty much). I’m a big fan of the whole dependency injection movement and make heavily use of <a href="http://en.wikipedia.org/wiki/Inversion_of_control" target="_blank">IoC</a> containers. Especially constructor injection turns out to be the correct approach for almost every system I've worked on. One thing that always bugged me is when creating a new object to run some tests on, is the amount of mocking code I will have to write to create the instance:

[csharp]
var mock = new Mock&lt;IService3&gt;();
mock.Setup(x =&gt; x.DoStuff()).Returns(true);
var sut = new ServiceToTest(
    new Mock&lt;IService1&gt;().Object,
    new Mock&lt;IService2&gt;().Object,
    mock.Object);
[/csharp]

The above code is a simplified example. Most of my test-hungry classes typically have lots of dependencies injected through their constructors. In the example I only need to set expectations on the mocked IService3, why I simply inject “empty” mocks for the rest of the parameters. Alternative I could inject nulls for the not important parameters, but in my experience, injecting something not null, makes the test more robust to changes in the tested code.

So why is this a problem? It all leads down to the fact that I am lazy. Creating multiple “empty” mocks is boring and every time I add a new parameter to the constructor of the ServiceToTest class, I need to fix one to multiple compile errors in my test project.

I recently discovered that <a href="http://autofixture.codeplex.com/" target="_blank">AutoFixture</a>, one of my favorite unit test frameworks, became a lot smarter from version 2.0 and beyond. AutoFixture now accepts plugins, which have opened up for a range of interesting additions to this great framework. A great plugin for all of your Moq users is <a href="http://nuget.org/List/Packages/AutoFixture.AutoMoq" target="_blank">AutoFixture with Auto Mocking using Moq</a>. Even though the name sounds a bit uncool, the added value is really great! With Auto Mocking my code can be refactored to the following lines:

[csharp]
var fixture = new Fixture().Customize(new AutoMoqCustomization());
var mock = fixture.Freeze&lt;Mock&lt;IService3&gt;&gt;();
mock.Setup(x =&gt; x.DoStuff()).Returns(true);
var sut = fixture.CreateAnonymous&lt;ServiceToTest&gt;();
[/csharp]

I start by creating a new instance of the AutoFixture Fixture type (line 1). I typically have this line in my test base class anyway. In the same line I tell AutoFixture to use the Auto Moq plugin by creating a new AutoMoqCustomization instance and passing it to the new Customize method on my fixture instance. Fluent APIs are great!

Let’s jump to line 4, where I ask AutoFixture to create a new instance of the ServiceToTest class. AutoFixture does its magic and creates a new instance using reflection. Remember that the ServiceToTest class have a constructor taking three arguments. AutoFixture itself can’t figure out how to create instances of IService1, IService2 and IService3, but because we provided it with the AutoMoqCustomization plugin, which uses <a href="http://code.google.com/p/moq/" target="_blank">Moq</a> for creating those instances. Brilliant!

In my case I actually want to setup some expectations on the IService3 mock, why I use the Freeze feature of AutoFixture (line 3), to tell the framework to use a freezed variable in the fixture. This will bypass the default behavior for creating new instances in AutoFixture.

So what did we accomplish here? Adding parameters to the constructor of ServiceToTest no longer causes compile errors in the tests and furthermore we don't need to manually add additional "empty" mock parameters. Sweet right?