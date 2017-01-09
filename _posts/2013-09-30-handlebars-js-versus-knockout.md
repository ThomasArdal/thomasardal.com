---
ID: 1161
post_title: Handlebars.js versus Knockout
author: Thomas Ardal
post_date: 2013-09-30 13:09:59
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/handlebars-js-versus-knockout/
published: true
---
<a href="http://thomasardal.com/wp-content/uploads/2013/09/knockout.jpg"><img class="alignright size-full wp-image-1165" alt="knockout" src="http://thomasardal.com/wp-content/uploads/2013/09/knockout.jpg" width="70" height="70" /></a><a href="http://thomasardal.com/wp-content/uploads/2013/09/handlebars.png"><img class="alignleft size-full wp-image-1166" alt="handlebars" src="http://thomasardal.com/wp-content/uploads/2013/09/handlebars.png" width="70" height="70" /></a>I'm still at the GOTO conference and having a blast, despite still being a bit struck by the flu. I attended another presentation about node.js. The presenter also demoed ember.js, Grunt and Azure and it was sort of interesting. What caught my attention where not really the key subjects, but the template engine used in emberjs: <a href="http://handlebarsjs.com/" target="_blank">Handlebars.js</a>. I've been using <a href="http://knockoutjs.com/" target="_blank">Knockout</a> heavily during the last couple of years and loved every minute of it. In the old days, Knockout utilized the jQuery template engine, which I never really liked. Luckily Knockout now bundles its own shiny template engine whooping everyones ass. I wanted to spend a little time comparing Handlebars.js to Knockout.

The Knockout example:

<script src="https://gist.github.com/ThomasArdal/6763225.js"></script>

Nice and simple. For each comment in an array named comments, add a new h2 containing a link to the title and a div containing the comment body.

Pros:
<ul>
	<li>No magic strings</li>
	<li>HTML validates</li>
	<li>Compact</li>
</ul>
Cons:
<ul>
	<li>Not possible to set attributes without data-bind</li>
	<li>Annoying use of attr binding to concatenate strings</li>
</ul>
Now for the Handlebars.js example:

<script src="https://gist.github.com/ThomasArdal/6763252.js"></script>

Same story. Iterate through the comments and show the details.

Pros:
<ul>
	<li>Possible to write template code directly in attributes</li>
	<li>Nice string concatenation using variables</li>
</ul>
Cons:
<ul>
	<li>Magic strings ({{#each comments}})</li>
	<li>More lines</li>
</ul>
So what's the best approach? Personally I really don't like the magic strings spread around the Handlebars.js code. On the other hand having the possibility to output the variables where they should be displayed, compared to the data-bind approach in Knockout, really makes the code simpler. I'll definitely stick with Knockout, but Handlebars.js sure has some nice features I would like to see adopted by Knockout.