---
ID: 325
post_title: Unit testing attribute decorations
author: Thomas Ardal
post_date: 2011-06-28 18:02:05
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/unit-testing-attribute-decorations/
published: true
---
This is my first blog post about unit testing. And so, without further ado here's a small utility I wrote the other day.

<strong>Problem</strong>

Attributes in .NET as well as annotations in Java are great. I simply love the strengths of action filters in ASP.NET MVC, which are simply just a way to do <a href="http://en.wikipedia.org/wiki/Aspect-oriented_programming" target="_blank">aspect oriented programming</a> through attributes. You typically want to write a unit test of the method annotated with an attribute, as well as a unit test of the attribute implementation. But sometimes it would be a disaster if someone by mistake removed or out commented the attribute definition from a method. So why not write a unit test of the presence of the attribute? In my case I had a number of ASP.NET MVC actions, annotated with the Authorize attribute. This simply tells the framework to verify that only logged on users can call the method.

<strong>Solution</strong>

My solution was to write some reflection code, which verifies the presence of a specified attribute on the method under test. I'm a big fan of DRY and therefore wanted to make a reusable method for this scenario. Inspired by <a href="http://shouldly.github.com/" target="_blank">Shoudly</a> (a framework for writing cleaner unit tests, which I will probably write more about in the future), I wrote the following extension method:

[csharp]
public static void ShouldHave&lt;T, TT&gt;(this T obj, Expression&lt;Func&lt;T, TT&gt;&gt; exp, params Type[] attributes)
{
    var memberExpression = exp.Body as MethodCallExpression;
    foreach (var attribute in attributes)
    {
        Assert.That(
            memberExpression.Method.GetCustomAttributes(attribute, false).Any(),
            Is.True,
            attribute.Name + &quot; not found on &quot; + memberExpression.Method.Name);
    }
}
[/csharp]

This small method makes unit testing the presence of an attribute <a href="http://www.youtube.com/watch?v=9_wpcQcTmPg" target="_blank">piece of pie</a>:

[csharp]
var controller = new HomeController();
controller.ShouldHave(x =&gt; x.Index(), typeof(AuthorizeAttribute));
[/csharp]

Nice! We now have a strongly typed test, which verifies the AuthorizeAttribute on the Index method.