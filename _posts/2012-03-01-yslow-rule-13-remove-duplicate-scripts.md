---
ID: 540
post_title: 'YSlow rule #13 – Remove Duplicate Scripts'
author: Thomas Ardal
post_date: 2012-03-01 20:08:38
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-13-remove-duplicate-scripts/
published: true
---
It’s been 7 months since the <a href="http://thomasardal.com/yslow-rules-12-avoid-redirects/">last post</a> in my YSlow series. So why did I stop blogging about performance optimization? Well, actually there are two answers to that question. First is because of my passion for unit-testing, which I started blogging about at pretty much that time. Secondly, I have to admit that I didn’t know what to write about. YSlow rule #13 – Remove Duplicate Scripts.

So what’s the problem with including the same JavaScript twice (or more)? When the browser reaches a script import, it makes an HTTP request for that script. Not all browsers are smart enough to catch this mistake. Even with modern and fast internet connections, making a lot of requests slows down the experience of your website. If you have your caching strategy under control, the duplicate request shouldn’t be much of a problem. If not, the remote request is actually made multiple times. I typically see two different patterns when people accidentially include the same script twice.

<strong>Pattern 1: Including scripts in both layout/master page and partial views</strong>

Partial views in ASP.NET MVC are a great way to extract common reusable controls into a single file. A partial view has the possibility to load external JavaScript files as well. A common mistake by web developers is including a reference to the same script in both the layout/master page-file as well as a partial view. I haven’t really found a great solution for catching this mistake, other than running YSlow on your page. If you read this and know a solution to this problem, please let me know in the comments.

<strong>Pattern 2: Including both a minified and unminified version of a script</strong>

I’ve seen multiple websites doing something like this:

[html]

&lt;script type=&quot;text/javascript&quot; src=&quot;https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.js&quot;&gt;&lt;/script&gt;
&lt;script type=&quot;text/javascript&quot; src=&quot;https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js&quot;&gt;&lt;/script&gt;

[/html]

You typically see this pattern from webdev-cowboys who need to track down a bug in the JavaScript by adding the unminified version of jQuery for debugging. When the bug is fixed, they forget to remove the newly added reference. The mistake won’t show itself in the browser because the last imported script overrides the first. I was a bit disappointed by the new bundling support in ASP.NET MVC 4 when I found out that the bundler always adds the minified version when available. Making it choose the full version in Debug configuration and the minified version in Release configuration would have been the optimal choice for me.

I’m working to catch this bug in my upcoming website validator <a href="http://www.hippovalidator.com/" target="_blank">HippoValidator.com</a>.