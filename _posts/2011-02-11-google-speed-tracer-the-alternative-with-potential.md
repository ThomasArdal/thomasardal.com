---
ID: 38
post_title: 'Google Speed Tracer &#8211; The alternative with potential'
author: Thomas Ardal
post_date: 2011-02-11 08:11:41
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/google-speed-tracer-the-alternative-with-potential/
published: true
---
In the two previous posts, I've covered different plugins for Firefox. Even though web developers still seem to prefer Firefox over Chrome, a lot of us (me inclusive) switched to Chrome. Luckily Google does a pretty decent performance optimization extension for Chrome as well. The extension is Google Speed Tracer. You will find it at Google Code here: <a href="http://code.google.com/webtoolkit/speedtracer/">http://code.google.com/webtoolkit/speedtracer/</a>.

When installed, a green timer icon will show to the right of the omnibox. One might think that the tool is now ready to run; unfortunately you need to modify how Chrome starts by adding the --enable-extension-timeline-api parameter to your Chrome shortcut. This seems a bit stupid, but once it's done, you don't need to worry about it again.

Speed Tracer is a bit different from YSlow and Page Speed in the way it presents your page. When you click the green timer icon, the Speed Tracer tab opens and starts profiling all the activity on the active tab. Try browsing to the page you need to optimize or reload it if it was already loaded in the active Chrome tab. Speed Tracer shows you all the activities needed to render your page. This typically consists of loading scripts, images, rendering HTML and even garbage collection as illustrated on the following screenshot:

<a href="http://performancedude.com/wp-content/uploads/2011/02/speedtracer.png"><img class="alignnone size-full wp-image-120" style="margin-top: 10px; margin-bottom: 10px;" title="speedtracer" src="http://performancedude.com/wp-content/uploads/2011/02/speedtracer.png" alt="" width="480" height="597" /></a>

Unlike YSlow and Page Speed, which focuses a lot on script, styling, and images, Speed Tracer also shows the time spent by the browser in rendering the various HTML elements on the page. Next to the headlines at the left, a small icon is sometimes shown, indicating available help to fix this issue. Unfortunately these help icons don't show up that often, and when not shown, it is hard to find out how to deal with specific issues. This is the main problem with Speed Tracer. For us to be able to fix slow rendering of HTML, Google needs to provide us with hints on where the problems are and how to fix them. For instance, it would be pretty simple to implement a warning when a table is used for layout rather than a div. The tool could even generate the optimized code.

Look at the following example:

<a href="http://performancedude.com/wp-content/uploads/2011/02/speedtracer_warning.png"><img class="alignnone size-full wp-image-122" style="margin-top: 10px; margin-bottom: 10px;" title="speedtracer_warning" src="http://performancedude.com/wp-content/uploads/2011/02/speedtracer_warning.png" alt="" width="480" height="170" /></a>

Speed Tracer is trying to tell me that a paint operation is slow. Slow is measured as anything above 100 ms. But the tool doesn't tell me what parts of the painting is slow and how to fix it.

My conclusion on Speed Tracer is that it's semiuseful but has a lot of potential if Google decides to update it with more rules and tips.