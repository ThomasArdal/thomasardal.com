---
ID: 108
post_title: 'Google Webmaster Tools &#8211; What&#8217;s your site performance?'
author: Thomas Ardal
post_date: 2011-02-18 06:26:30
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/google-webmaster-tools-whats-your-site-performance/
published: true
---
In the last three posts, I’ve looked at tools for optimizing performance on a website. This week I will show you a nice performance-related feature in Google Webmaster Tools. Google Webmaster Tools is a free tool from Google that provides different tools for webmasters, such as submitting a sitemap, generating a robots.txt, and other stuff. Webmaster Tools is typically not used for looking at performance issues, but if you expand the Labs tree note in the navigation menu, you will see a link named Site performance. If you click this link, a graph will show, which looks something like this:

<a href="http://performancedude.com/wp-content/uploads/2011/02/chart.png"><img class="alignnone size-full wp-image-109" title="chart" src="http://performancedude.com/wp-content/uploads/2011/02/chart.png" alt="" width="480" height="110" /></a>

The screenshot shows the site performance graph for one of my Danish websites, <a href="http://www.myrating.dk">www.myrating.dk.</a> Notice that unlike YSlow, Page Speed, and Speed Tracer, the result showed above covers the average load times for <em>all</em> your pages and not only one page at the time. The chart is divided into two areas: a fast green area and a red slow area. If the trend line is below the red dotted line in the green area, this means that your site is fast. If above the red line, your site is slow. The statistics is generated from all users who have installed the Google Toolbar and enabled the PageRank feature. This means that you need a lot of visitors on your site for this graph to show a realistic result.

Below the graph you will find up to ten URLs, which Google have suggestions for you to optimize:

<a href="http://performancedude.com/wp-content/uploads/2011/02/urls.png"><img class="alignnone size-full wp-image-127" title="urls" src="http://performancedude.com/wp-content/uploads/2011/02/urls.png" alt="" width="480" height="239" /></a>

You can expand each URL, giving you a detailed explanation on what to improve. The rules used on Google Webmaster Tools look like a subset of the rules used on Google Page Speed. I don’t see much idea in this feature, unless you don’t have Page Speed installed.

It would be great if Google would implement some sort of alert, where you could receive an email or similar, if response times suddenly decreased.