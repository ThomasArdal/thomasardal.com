---
ID: 13
post_title: >
  Combining your scripts and style sheets
  with SquishIt
author: Thomas Ardal
post_date: 2011-03-04 07:09:41
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/combining-your-scripts-and-style-sheets-with-squishit/
published: true
---
One of the rules mentioned in most performance-analyzing tools is "Combine your JavaScript." Basically this means that instead of this:

[html]

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/one.js&gt;&lt;/script&gt;

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/two.js&gt;&lt;/script&gt;

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/three.js&gt;&lt;/script&gt;

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/four.js&gt;&lt;/script&gt;

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/five.js&gt;&lt;/script&gt;

[/html]

You want to do this instead:

[html]

&lt;script type=&quot;text/javascript&quot; src=&quot;/scripts/complete.js&gt;&lt;/script&gt;

[/html]

So what's the problem with approach number 1? When your browser parses the HTML and reaches the script tag, it makes an http request to fetch the script referenced in the src attribute. The rendering of the page blocks, until the script is loaded. Combining the five scripts into one removes the overhead of doing four http requests. The combined script will of course get the same size as the five individual scripts, but the browser only needs to do one request to get it.

You can manually combine your scripts, but this is tedious and error prone. Instead you should use a tool like <a href="https://github.com/jetheredge/SquishIt">SquishIt</a> by Justin Etheredge. Justin explains the concept in great detail <a href="http://www.codethinked.com/squishit-the-friendly-aspnet-javascript-and-css-squisher">here</a>. The basic idea is to keep you scripts and style sheets separate and combine them at runtime. All you need to do is download SquishIt, reference the SquishIt.Framework.dll in your project, import the SquishIt.Framework namespace in your master page, and add the following line instead of the above five script imports:

[php]
&lt;%= Bundle.JavaScript()
.Add(&quot;/scripts/one.js&quot;)
.Add(&quot;/scripts/two.js&quot;)
.Add(&quot;/scripts/three.js&quot;)
.Add(&quot;/scripts/four.js&quot;)
.Add(&quot;/scripts/five.js&quot;)
.Render(&quot;/scripts/combined_#.js&quot;)
%&gt;
[/php]
<div>SquishIt will generate a file name combined followed by a hash of the five files. All at runtime! It doesn't get much better than this. We still have our scripts in separate logical files, but the clients only have to request one file. The same API is used for style sheets as well. Here you need to call the Css() method on the bundle class instead.</div>