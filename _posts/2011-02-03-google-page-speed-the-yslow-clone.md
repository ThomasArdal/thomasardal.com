---
ID: 35
post_title: 'Google Page Speed &#8211; the YSlow clone'
author: Thomas Ardal
post_date: 2011-02-03 09:01:48
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/google-page-speed-the-yslow-clone/
published: true
---
Page Speed is a plug-in developed for Firebug by Google. If you don’t use Firebug, get it (<a href="http://getfirebug.com/">http://getfirebug.com/</a>)! It’s the coolest web developer plug-in for Firefox out there. The Page Speed tool is downloaded from <a href="http://code.google.com/speed/page-speed/download.html">http://code.google.com/speed/page-speed/download.html</a>. Google Page Speed is very similar to Yahoo!’s YSlow, which I talked about in my previous blog post. I don’t say that competition is a bad thing, but developing a tool based on pretty much the same set of rules just seems stupid.

Page Speed analyzes your web page just as YSlow. Your page will get a grade from 1 to 100 (100 is the best), based on how you managed to follow certain rules for doing fast rendering of web pages. The rules are very similar to the ones available in YSlow. Some of the descriptions have a bit more information, like showing you how many percentage a file can be compressed. If you find old screenshots of Page Speed, it had a nice Activity tab, showing you a lot more information about the resources needed for your page to load than the Net tab in Firebug. In 2010 Firebug got those missing information, and Google deprecated the Activity tab.

One nice feature in Page Speed is its ability to natively optimize images, which can be compressed, as illustrated on the following screenshot:

<a href="http://performancedude.com/wp-content/uploads/2011/01/pagespeed_compress.png"><img class="alignnone size-full wp-image-74" style="margin-top: 10px; margin-bottom: 10px;" title="pagespeed_compress" src="http://performancedude.com/wp-content/uploads/2011/01/pagespeed_compress.png" alt="" width="480" height="180" /></a>

This feature is available in YSlow as well, but through an external service.

So which tool should you use, YSlow or Page Speed? The answer is probably both. Try testing your pages in both tools and check for differences. I usually start by analyzing my page in YSlow, optimize it, and finish off by analyzing it in Page Speed. If you prefer Page Speed over YSlow, try doing it the other way around.