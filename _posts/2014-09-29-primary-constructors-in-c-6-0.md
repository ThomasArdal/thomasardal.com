---
ID: 1442
post_title: 'Primary Constructors in C# 6.0'
author: Thomas Ardal
post_date: 2014-09-29 18:33:49
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/primary-constructors-in-c-6-0/
published: true
---
In my <a href="http://thomasardal.com/auto-properties-with-initializers-in-c-6-0/">previous post</a>, inspired by the <a href="http://gotocon.com/aarhus-2014/presentations/show_presentation.jsp?oid=6227">Future of C#</a> talk at GOTO, I looked at Initializers, making it possible to assign a value to an auto-property at declaration, just as with a field. This post is about another piece of syntactic sugar in the up-coming version 6.0 of C#: Primary Constructors.
 
Continuing the example from before, assigning values to the Movie class through a constructor, could look like this:
 
<pre class="lang:c# decode:true " >public class Movie
{
    public string Title { get; private set; }
 
    public List&lt;string&gt; Genres { get; private set; }
 
    public Movie(string title, List&lt;string&gt; genres)
    {
        Title = title;
        Genres = genres;
    }
}</pre>

A constructor assigns values to the two auto properties Title and Genres.
 
Let’s look at how Primary Constructors in C# 6.0 can be used to optimize the code above:
 
<pre class="lang:c# decode:true " >public class Movie(string title, List&lt;string&gt; genres)
{
    public string Title { get; } = title;
 
    public List&lt;string&gt; Genres { get; } = genres;
}</pre> 

Once again the constructor is missing and no need to specify a private setter. Notice the parameters added just right of the class definition. This tells the compiler to add a constructor to the Movie class with two parameters: title and genres. We use our knowledge of the previous blog post, to assign the constructor parameters to properties through initializers. Magic.
 
The decompiled code looks as expected:
 
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
    
    public Movie(string title, List&lt;string&gt; genres)
    {
    	this.&lt;Title&gt;k__BackingField = title;
    	this.&lt;Genres&gt;k__BackingField = genres;
    	base..ctor();
    }
}</pre> 

A constructor is generated with the same parameters as specified on the primary constructor and values assigned each auto-property. As with initializers, all of the syntactic sugar only exists on compile time, making it possible to execute this bad boy on previous versions of .NET.
 
Let's look at a more complex example before we move on. Only one primary constructor per class is allowed. You do have the possibility to specify additional constructors through the usual syntax and you can even use <em>this</em> to invoke your primary constructor as illustrated in the following example:
 
<pre class="lang:default decode:true " >public class Movie(string title, List&lt;string&gt; genres)
{
    public string Title { get; } = title;
 
    public List&lt;string&gt; Genres { get; } = genres;
 
    public int Runtime { get; private set; }
 
    public Movie(string title, List&lt;string&gt; genres, int runtime) : this(title, genres)
    {
        Runtime = runtime;
    }
}</pre> 

You probably get it by now but to be explicit and consistent, let's look at the decompiled code:
 
<pre class="lang:c# decode:true " >public class Movie
{
    private readonly string &lt;Title&gt;k__BackingField
    private readonly List&lt;string&gt; &lt;Genres&gt;k__BackingField;
    private readonly int &lt;Runtime&gt;k__BackingField
    
    public string Title
    {
        get { return this.&lt;Title&gt;k__BackingField; }
    }
    
    public List&lt;string&gt; Genres
    {
        get { return this.&lt;Genres&gt;k__BackingField; }
    }
    
    public int Runtime
    {
        get { return this.&lt;Runtime&gt;k__BackingField; }
        private set { this.&lt;Runtime&gt;k__BackingField = value; }
    }
 
    public Movie(string title, List&lt;string&gt; genres)
    {
        this.&lt;Title&gt;k__BackingField = title;
        this.&lt;Genres&gt;k__BackingField = genres;
        base();
    }
 
    public Movie(string title, List&lt;string&gt; genres, int runtime) : this(title, genres)
    {
        this.Runtime = runtime;
    }
}</pre> 

In the following post I will check out <a href="http://thomasardal.com/declaration-expressions-and-static-using-in-c-6-0/">Declaration Expressions and Static Using</a>.