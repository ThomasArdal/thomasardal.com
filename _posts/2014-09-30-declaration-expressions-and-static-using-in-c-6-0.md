---
ID: 1447
post_title: 'Declaration Expressions and Static Using in C# 6.0'
author: Thomas Ardal
post_date: 2014-09-30 10:57:56
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/declaration-expressions-and-static-using-in-c-6-0/
published: true
---
<blockquote>One feature considered and ultimately not added to C# 6 was declaration expressions.</blockquote>

In the previous post, inspired by <a href="http://gotocon.com/aarhus-2014/presentations/show_presentation.jsp?oid=6227" target="_blank">The Future of C#</a> talk at GOTO, I looked at <a href="http://thomasardal.com/primary-constructors-in-c-6-0/">Primary Constructors</a>. This post is about two nice little improvements in C# 6.0 called Declaration Expressions and Static Using.

As usual let's start by looking at a piece of code:

<pre class="lang:c# decode:true " >bool result;
if (bool.TryParse("true", out result))
{
    Console.WriteLine("This is a bool with value. " + result);
}
else
{ 
    Console.WriteLine("Not a bool, yo! " + result);
}</pre> 

To check if the string value is a bool, we use the <code>bool.TryParse</code> method, which returns <code>true</code> if the string can be parsed as a bool. The parsed value is set on the out parameter named <code>result</code>.

Having to declare the <code>result</code> bool outside the scope of the if-else always bugged me. With Declaration Expressions the future suddenly seems brighter:

<pre class="lang:c# decode:true " >if (bool.TryParse("true", out var result))
{
    Console.WriteLine("This is a bool with value. " + result);
}
else
{ 
    Console.WriteLine("Not a bool, yo! " + result);
}</pre> 

It may be hard to spot other differences than the missing declaration of <code>result</code>, but take a closer look just after the <code>out</code> keyword. A new <code>var</code> keyword crept in between <code>out</code> and <code>result</code>, declaring the <code>result</code> bool in the scope of the if-else only. Please also notice, that the <code>result</code> variable is now only accessible inside the if-part, but also the else-part.

Not surprisingly the decompiled code looks similar to the first example:
 
<pre class="lang:c# decode:true " >bool flag;
if (bool.TryParse("true", out flag))
{
    Console.WriteLine("This is a bool with value. " + flag);
}
else
{
    Console.WriteLine("Not a bool, yo! " + flag);
}</pre> 

Before we wrap up, let's spend just a second looking at another new feature of C# 6.0: Static Using. I began my career coding Java, but shifted towards .NET back in 2006. A couple of years ago, I had to write some Java code (don't ask) and one of the new features which had been introduced in the Java language since I left it where Static Imports. After my quick Java endeavors, I actually missed Static Imports in .NET, why I'm happy that we now get Static Using. To boil down, static using let you import static methods from other types directly into your current class. Let's look at how we can improve the bool example to utilize static using:
 
<pre class="lang:c# decode:true " >using System.Console;
 
...
 
if (bool.TryParse("true", out var result))
{
    WriteLine("This is a bool with value. " + result);
}
else
{ 
    WriteLine("Not a bool, yo! " + result);
}</pre> 

We start by declaring a new <code>using</code> statement. Unlike your normal using, the static using actually resolves to a type name and not a namespace, in this case the <code>Console</code> type. When utilized, all of the static members of <code>Console</code> are available to our current type, making it possible to execute <code>WriteLine</code> without having the prepend it with <code>Console</code>. The used type needs to be declared static itself, which means that you can't declare a using of a non-static class, even it contains static methods. This makes it ideal for utilities like <code>Console</code> and <code>Math</code>.