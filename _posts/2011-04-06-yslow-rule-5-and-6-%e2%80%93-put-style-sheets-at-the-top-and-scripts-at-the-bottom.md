---
ID: 234
post_title: 'YSlow rule #6 and #7 – Put style sheets at the top and scripts at the bottom'
author: Thomas Ardal
post_date: 2011-04-06 06:56:51
post_excerpt: ""
layout: post
permalink: 'http://thomasardal.com/yslow-rule-5-and-6-%e2%80%93-put-style-sheets-at-the-top-and-scripts-at-the-bottom/'
published: true
---
This week I have chosen to combine rule numbers 6 and 7. Why? Because they focus on pretty much the same thing: where to put style sheet and script references. According to rule number 6, you should put all your styling in the head section of your HTML document. The YSlow documentation <a href="http://developer.yahoo.com/performance/rules.html#css_top" target="_blank">explains why</a>. There's no magical formula you should know in order to obey this rule, so just do it.

The difficult thing about the two rules is putting JavaScript at the bottom. And by bottom I do not mean the bottom of the head section, but just before the closing body tag. In order to do this, first you need to <a href="http://en.wikipedia.org/wiki/Unobtrusive_JavaScript" target="_blank">write your JavaScript unobtrusive</a>, which simply means that you separate your view (HTML) from your controller (JavaScript). When split into these two logical components, you won't have any nasty document.write statements inlined in your HTML. Placing the script references should now be simple, but you typically still need to execute some JavaScript. Don't worry, jQuery comes to your rescue with the ready function:

[js]

$(document).ready(function() {

// Place some code here

});

[/js]

The function specified inside the ready method is executed after the DOM has been loaded.

When doing ASP.NET MVC, you most often use a master page for all the common stuff like your menu, header, footer, etc. I've used content place holders to ease the implementation of JavaScript in my views, with a master page, which looks like this:

[html]
&lt;%@ Master Language=&quot;C#&quot; Inherits=&quot;System.Web.Mvc.ViewMasterPage&quot; %&gt;
&lt;html&gt;
&lt;head&gt;
&lt;!-- Styles and stuff --&gt;
&lt;/head&gt;
&lt;body&gt;
  &lt;!-- Your HTML goes here --&gt;
  &lt;script type=&quot;text/javascript&quot; src=&quot;https://ajax.googleapis.com/ajax/libs/jquery/1.5.2/jquery.min.js&quot;&gt;&lt;/script&gt;

  &lt;script type=&quot;text/javascript&quot;&gt;
    $(document).ready(function() {
      &lt;asp:ContentPlaceHolder ID=&quot;domready&quot; runat=&quot;server&quot;&gt;&lt;/asp:ContentPlaceHolder&gt;
    });
  &lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
[/html]

In your view, you will be able to add a content place holder and write JavaScript right inside. The code will run when the DOM is ready.