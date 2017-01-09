---
ID: 249
post_title: 'YSlow rule #8 – Avoid CSS Expressions'
author: Thomas Ardal
post_date: 2011-05-04 11:31:15
post_excerpt: ""
layout: post
permalink: 'http://thomasardal.com/yslow-rule-7-%e2%80%93-avoid-css-expressions/'
published: true
---
Time for another YSlow rule. This week we will be looking at rule number 8—avoid CSS expressions. My knowledge about CSS is very limited, and I've always seen CSS as a format for specifying style information rather than an actual programming language. It turns out that you can actually write JavaScript inside your CSS files. First of all, how stupid is that? Sounds a bit like writing JavaScript in your HTML code. I really like each language separated into its own set of files. The idea with CSS expressions should be to do dynamic styling depending on JavaScript code. I really can't see the need for this, with the powers in JavaScript, jQuery, and even on the server-side when generating the markup.

So what's the problem with doing CSS expressions? Let's look at an example from the YSlow documentation:

[css]
background-color: expression( (new Date()).getHours()%2 ? &quot;#B8D4FF&quot; : &quot;#F08A00&quot; );
[/css]

The example above set the background color to either of two hex values, depending on the current time. The problem here is that the browser executes the code over and over again, each time the user interacts with the page (scroll, click, and even move the mouse). Even though this only affects the performance inside each individual browser, it can result in a slow-performing browser.
I've never used CSS expressions myself and won't recommend anyone to do so. Also, CSS expressions are only supported by Internet Explorer 5 to 7. Microsoft has really done some cool stuff over the years, but this must be one of their greatest screwups.