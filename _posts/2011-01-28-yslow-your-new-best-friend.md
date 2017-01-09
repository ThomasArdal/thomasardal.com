---
ID: 33
post_title: 'YSlow &#8211; your new best friend'
author: Thomas Ardal
post_date: 2011-01-28 06:07:23
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-your-new-best-friend/
published: true
---
<a href="http://performancedude.com/wp-content/uploads/2011/01/yslow_overview.png"></a>The first tool that you should definitely check out is YSlow from Yahoo. YSlow is a plug-in for Firebug, which is a must-have for all web developers. You can find the Firebug plug-in here: <a href="http://getfirebug.com/">http://getfirebug.com/</a> and the YSlow plug-in here: <a href="http://developer.yahoo.com/yslow/">http://developer.yahoo.com/yslow/</a>. Firebug is activated by clicking the small bug icon in the bottom right part of the Firefox window. When visible you will get a new tab in Firebug as illustrated below:

<a href="http://performancedude.com/wp-content/uploads/2011/01/yslow_overview3.png"><img class="alignnone size-full wp-image-64" style="margin-top: 10px; margin-bottom: 10px; margin-left: 0px; margin-right: 0px;" title="YSlow tab" src="http://performancedude.com/wp-content/uploads/2011/01/yslow_overview3.png" alt="" width="480" height="176" /></a>

You start the analyzer by browsing to the site you want to analyze and enable the "Autorun YSlow each time a web page is loaded" or by presseng the "Run test" button. In the following example, I've analyzed the URL "http://www.myrating.dk/indhold/3422-boller_i_karry" from one of my websites:

<a href="http://performancedude.com/wp-content/uploads/2011/01/yslow_test.png"><img class="size-full wp-image-67  alignnone" style="margin-top: 10px; margin-bottom: 10px;" title="YSlow test" src="http://performancedude.com/wp-content/uploads/2011/01/yslow_test.png" alt="" width="480" height="380" /></a>

(By the way, "Boller i karry" is a great Danish dish. If you want to make it, a translated recipe is available here: <a href="http://translate.google.com/translate?js=n&amp;prev=_t&amp;hl=en&amp;ie=UTF-8&amp;layout=2&amp;eotf=1&amp;sl=da&amp;tl=en&amp;u=http%3A%2F%2Fwww.myrating.dk%2Findhold%2F3422-boller_i_karry&amp;act=url">http://translate.google.com/translate?js=n&amp;prev=_t&amp;hl=en&amp;ie=UTF-8&amp;layout=2&amp;eotf=1&amp;sl=da&amp;tl=en&amp;u=http%3A%2F%2Fwww.myrating.dk%2Findhold%2F3422-boller_i_karry&amp;act=url</a>.)

As shown in the screenshot, YSlow analyzes the content of the current page and shows you an overall performance score as well as a detailed list of rules, each graded from A to F (A is best). This page is rated C, which is actually not that bad. Striving for the best grade on all your pages will most likely be a waste of time. I usually go for an A or B on simple pages and B or C on complex pages, which uses a lot of external JavaScripts, etc.

You can click each rule, which shows a detailed description in the area at the right. YSlow typically shows a detailed description, explaining what you've done wrong as well as some hints on how to fix it. Note the "Read More" link at the bottom of each description, which will direct you toward a more detailed description at the developer.yahoo.com website.

YSlow has tabs as well. As default the Grade tab is selected. Make sure to check out the other tabs as well. The Components tab gives you a grouped overview of all your resources like HTML, JavaScript, and CSS. The Statistics tab is sort of the same but shown in a pie chart.  The hidden treasure is found beneath the Tools tab. There you'll find URLs for different tools to help you optimize CSS, JavaScript, and much more. My favorite tool is the link titled "All Smush.it," which will redirect you to the Smush.it service from Yahoo. I will write a blog post about image compression sometime in the future, but shortly told, the Smush.it service analyzes all your images and compresses them further, letting you download all the optimized images in a single zip file.

I will go into each YSlow rule in detail in upcoming posts, but right now you know enough to start analyzing your own sites.