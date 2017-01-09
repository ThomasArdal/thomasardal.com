---
ID: 1439
post_title: 'Auto-properties with Initializers in C# 6.0'
author: Thomas Ardal
post_date: 2014-09-29 13:53:24
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/auto-properties-with-initializers-in-c-6-0/
published: true
---
In the <a href="http://thomasardal.com/how-to-enable-c-6-0-language-preview-in-visual-studio-14-ctp-3/" target="_blank">previous post</a> (inspired by the <a href="http://gotocon.com/aarhus-2014/presentations/show_presentation.jsp?oid=6227" target="_blank">Future of C#</a> talk at GOTO), I showed you how to enable the C# 6.0 Language Preview in Visual Studio 14 CTP 3. In this post, I'll introduce you to the first of the new features in C# 6.0: Auto-Properties with Initializers.

So what does Auto-Properties with Initializers try to improve? Let's look at a simple example where I want to create a new object containing a couple of auto-properties with default values:
 
<pre class="lang:c# decode:true " >public class Movie
{
    public string Title { get; private set; }
 
    public List&lt;string&gt; Genres { get; private set; }
 
    public Movie()
    {
        Title = "The Big Lebowski";
        Genres = new List&lt;string&gt; { "Comedy", "Crime" };
    }
}</pre> 

A constructor assigns default values to the two auto properties Title and Genres. In the real world, the actual values would probably be parameters to the constructor, but I'll save that scenario for a later post.

Let's look at how initializer in C# 6.0 can be used to optimize the code above:
 
<pre class="lang:default decode:true " >public class Movie
{
    public string Title { get; } = "The Big Lebowski";
 
    public List&lt;string&gt; Genres { get; } = new List&lt;string&gt; { "Comedy", "Crime" };
}</pre> 

Notice that the constructor is missing. The value of the properties is assigned directly on each property. Another detail worth mentioning is, that auto-properties no longer require both a getter and a setter.

The highlighted advantage of this in other posts and articles seems to be the saved number of lines, but I think that there's a more obvious advantage writing default values this way (besides supporting primary constructors): We now have a uniform way of assigning values to properties and fields. I 

So how did Microsoft implement this behind the scene? Let's take a look at the compiled code through ILSpy:
 
<pre class="lang:default decode:true " >public class Movie
{
    private readonly string &lt;Title&gt;k__BackingField
    private readonly List&lt;string&gt; &lt;Genres&gt;k__BackingField;
    
    public string Title
    {
        get { return this.&lt;Title&gt;k__BackingField; }
    }
 
    public List&lt;string&gt; Genres
    {
        get { return this.&lt;Genres&gt;k__BackingField; }
    }
    
    public Movie()
    {
        this.&lt;Title&gt;k__BackingField = "The Big Lebowski";
        this.&lt;Genres&gt;k__BackingField = new List&lt;string&gt;
        {
            "Comedy",
            "Crime"
        };
        base..ctor();
    }
}</pre> 

Basically an exact copy of the code written in example 1. This being syntactic sugar, should make it possible to execute code including initializers to execute on previous versions of .NET.

In the next post I will look at <a href="http://thomasardal.com/primary-constructors-in-c-6-0/">Primary Constructors</a>, which fits perfectly with initializers.