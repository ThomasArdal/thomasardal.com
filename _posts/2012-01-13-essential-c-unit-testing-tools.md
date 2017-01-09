---
ID: 440
post_title: 'Essential c# unit testing tools'
author: Thomas Ardal
post_date: 2012-01-13 06:38:50
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/essential-c-unit-testing-tools/
published: true
---
During the last 6 years, I've preached unit testing to my colleagues and people attending my <a href="http://en.wikipedia.org/wiki/Test-driven_development">test-driven development</a> courses. The set of tools I've used for this purpose have been stable, for the most part. This blog post is an attempt to create a snapshot of the tools and frameworks I'm using right now.

<strong>Unit-Test Framework and Runner</strong>

<a href="http://nunit.org/">NUnit</a> has been my preferred tool to execute tests. I did some fooling around with <a href="http://en.wikipedia.org/wiki/MSTest" target="_blank">MSTest</a> a couple of years ago, but unlike NUnit, you need to install Visual Studio on the build server to run the tests. The syntax is strange and the test runner for Visual studio sucks. I also looked at <a href="http://xunit.codeplex.com/" target="_blank">xUnit.net</a> which is cool, but the limited support for <a href="http://www.jetbrains.com/resharper/" target="_blank">ReSharper</a> at the time kept me from switching. Let's admit it: NUnit is the defacto standard these days. The support and community around NUnit is so great, that I will probably not switch anytime soon. I really hope that NUnit looks into the cool things happening in xUnit.net. I also hope that build servers, Visual Studio extensions and other tools integrating unit test frameworks will do a better job of integrating xUnit.net.

<strong>Mocking Framework</strong>

<a href="http://hibernatingrhinos.com/open-source/rhino-mocks" target="_blank">Rhino Mocks</a> has been my favorite mocking framework for years. I tried <a href="http://code.google.com/p/moq/" target="_blank">Moq</a> two years ago, but quickly switched back to Rhino Mocks due to the strange errors I was receiving from Moq. Last year, I tried Moq again and liked it so much that this is now my favorite mocking framework. I also tried <a href="http://code.google.com/p/fakeiteasy/" target="_blank">FakeItEasy</a>, but I didn't really think that it brought anything new to the table.

<strong>Buildserver</strong>

You might be thinking something like, “Whuuut? Why does the unit-test freak suddenly mention anything about build servers, in the middle of a blog post about unit-testing tools?” Well, without a build server to execute all of your unit-tests, you probably shouldn't write them in the first place. The build server is a perfect place for conducting regression testing. I typically run all of my unit-tests before committing code, letting the build server run the heavy database-dependent integration-tests. During the last 10 years as a professional software developer, I've tried practically all major build servers from homemade bat-scripts over <a href="http://cruisecontrol.sourceforge.net/" target="_blank">CruiseControl</a> and <a href="http://sourceforge.net/projects/ccnet/" target="_blank">CruiseControl.NET</a> to <a href="http://www.jetbrains.com/teamcity/" target="_blank">TeamCity</a>. TeamCity is my current favorite, with great support for .NET/C#.

<strong>Helpers</strong>

Also, last year I started using <a href="http://autofixture.codeplex.com/" target="_blank">AutoFixture</a>, which is a cool library used to automate the creation of test values. I <a href="http://thomasardal.com/tag/autofixture/">blogged about AutoFixture a couple of times</a>.

<a href="http://shouldly.github.com/" target="_blank">Shouldly</a> is a nice set of extension methods for NUnit if you do not like the Assert.* syntax. I previously <a href="http://thomasardal.com/tag/shouldly/">blogged about Shouldly as well</a>.

Any opinions on missing frameworks?