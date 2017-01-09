---
ID: 293
post_title: 'YSlow rule #11 &#8211; Minify JavaScript and CSS'
author: Thomas Ardal
post_date: 2011-06-16 18:24:39
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-10-minify-javascript-and-css/
published: true
---
Way down the list of YSlow rules, we've reached rule number 11, which is important and a no-brainer to implement. The rule is titled "Minify JavaScript and CSS." So what is minification, and why do we need it?

Both JavaScript and CSS files are packed with unnessecary characters like spaces, comments, tabs, etc. Minification is the process of identifying which characters can be cut out. One part of minification can also be done by obfuscating the code, as we know it from obfuscators for Java and C#. In short, obfuscation is the process of shortening everything that can be shortened: method names, variable names, etc. These different methods combined lead to a more compressed output file, which leads us to question number 2: why do we need it? Fetching not only JavaScript but also CSS is a time-consuming task in the browser. The render thread blocks every time the browser loads a script, which can cause the user experience to regenerate. Speeding up the load time for JavaScripts and style sheets causes the browser to render the page quicker.

Minifying your JavaScripts and style sheets is easy. You can use one of the various minification tools online like <a href="http://www.minifyjs.com/javascript-compressor/" target="_blank">this</a>, <a href="http://jscompress.com/" target="_blank">this</a>, or <a href="http://www.jslab.dk/tools.minify.php" target="_blank">this</a>. If you want to use this approach, Google for "JavaScript minify" and use the tool you think is the best.

There's a downside to minification as well. When you want to debug your JavaScript, the minified source code looks pretty messy and is hard to read. Luckily there's a better way than hardcoding your minified source: minifying on the fly. I've previously talked about Mads Kristensen's WebOptimizer, which contains nice HTTP modules for both JavaScripts and style sheets. I usually go with <a href="https://github.com/jetheredge/SquishIt" target="_blank">SquishIt</a>, which I wrote about in my blog post "<a href="http://performancedude.com/2011/03/04/combining-your-scripts-and-style-sheets-with-squishit/">Combining your scripts and style sheets with SquishIt</a>." SquishIt combines your scripts and style sheets into a single file, which reduces the amount of HTTP requests done by the browser. In addition, SquishIt also minifies the generated file. What's really genius about the minification in SquishIt is its ability to avoid merging and minification when working on a localhost. Here you will have the ability to work with individual files as well as not-minified scripts, which helps you debug.