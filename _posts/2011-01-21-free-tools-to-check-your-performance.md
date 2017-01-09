---
ID: 22
post_title: Free tools to check your performance
author: Thomas Ardal
post_date: 2011-01-21 06:44:15
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/free-tools-to-check-your-performance/
published: true
---
So how do you spot if your site has performance issues or not? You probably have an idea if your site is responsive or not, but how does it compare to other sites, and which potential problems do you have? This is a starter's guide to some of the tools I've used for the job. Blog posts with detailed info on each tool will be published later.

By the way, this post only focuses on the tools and techniques used for updating one page at a time. If you're running a serious site, I highly recommend you to stress test your site using either real users or simulated clients. More on this subject in a future post as well.

<strong>YSlow</strong>

YSlow is probably one of the best and mostly used tools for analyzing performance problems. YSlow is a plug-in for the incredible Firefox plug-in Firebug. Yes, that's right, a plug-in in a plug-in. If you don't have Firebug installed, do it now. You won't regret it! YSlow does its job by analyzing the current page and showing you all the issues worth fixing. You will get a total score from 1 to 100 (100 is the best) based on not your response time, but the way you've written your code. Example: you are referencing fifteen JavaScript files, hosted at your own domain. This scores low, because you could combine these into one reference instead. Not all rules in YSlow are equally important, but start from the top and starting seing your response times improving.

<strong>Google Page Speed</strong>

Google Page Speed is very similar to YSlow. It's a plug-in for Firebug and analyzes the current page and shows issues. I don't really know why Google decided to make a clone of YSlow, rather than contributing to the YSlow project. But the Page Speed plug-in has a couple of great features, not natively implemented in YSlow. One cool feature is the ability to let Page Speed generate optimized images on the fly for your page, which saves you the time in Photoshop.

<strong>Google Speed Tracer</strong>

Google Speed Tracer is the new kid on the block. It's a plug-in for Google Chrome, which is nice if you're using Chrome instead of Firefox. Speed Tracer is similar to YSlow and Page Speed but also includes a great profiler. The tool is definitely usable but still needs some work in order to beat the Firefox plug-ins. I like the new view on things though, rather than just doing another YSlow clone.

<strong>Google Webmaster Tools</strong>

This is not really a performance tool, but Google Webmaster Tools have an interesting feature for investigating your site performance over time. Beneath the menu Labs | Site performance you will find a graph, showing the average response times for your site in the last six months. It's a good idea to follow this graph at least once a month, to verify that you didn't break something with one of your software updates. You will need a lot of visits for this graph to show a realistic image, though. The data are collected through visitors of your site, using the Google Toolbar.

<strong>The rest</strong>

This post only focused on optimizing the web part of your application. If you're connecting to a back end, a database, or similar, you could have performance problems here as well. In the upcoming posts on the various performance tools, I will show you how to spot these kinds of problems.