---
ID: 267
post_title: 'YSlow rule #9 &#8211; Make JavaScript and CSS External'
author: Thomas Ardal
post_date: 2011-05-17 06:17:35
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-8-make-javascript-and-css-external/
published: true
---
We've briefly touched this subject a number of times already. Making your JavaScript and CSS external simply means moving it from your HTML header to their own files, referenced as an external file in the header. So why does Yahoo think that this is a good idea? We've already discussed the possibility to cache resources in the browser client-side cache. Caching scripts and styles is definitely easier than caching HTML. JavaScript and CSS are typically only changed when changing something on the site like new styling, new features, etc. Changing HTML happens all the time when users write new content, new search results are found, etc. When inlined, the scripts and styles are typically not cached or at least not for a very long time. But wait, here’s the real bonus: scripts and styles are typically shared between multiple views. Moving all that static stuff to external files will make the browser cache them, serving the cached versions when loading page number 2, 3, and so on.

The YSlow documentation argues that you can sometimes benefit from inlining your script and styles directly in the HTML. This only goes for pages with a single page view per session like the Yahoo front page. I can see the point and noticed that pretty much all search engines do this. I still think that you should get a lot of visitors in order to do this kind of thing. So unless you're Google or your front page uses an entirely different styling than the rest of your site, you should be covered by moving the static stuff to external files.

Another benefit is when debugging through Firebug. When I need to debug some inlined JavaScript, I typically spend some time wondering and then realizing that I can't debug JavaScript from the HTML tab in Firebug. This usually causes a bit of confusion and sometimes even some pulling out of precious hair strands. The solution is to navigate to the scripts tab and selecting the HTML file. I can understand why the UI works that way, but I keep forgetting. Moving all JavaScript to an external file forces me to navigate to the scripts tab right away.