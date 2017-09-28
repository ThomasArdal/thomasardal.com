---
ID: 2041
post_title: 'Lean Startup on elmah.io &#8211; Metrics'
author: Thomas Ardal
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/lean-startup-on-elmah-io-metrics/
published: true
post_date: 2017-09-14 17:24:20
---
This is the fifth post in a series about Lean Startup on <a href="https://elmah.io/">elmah.io</a>. Check out the other posts here:

<ol>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-introduction/">Introduction</a></li>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-minimum-viable-product/">Minimum viable product</a></li>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-continuous-deployment/">Continuous deployment</a></li>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-split-testing/">Split testing</a></li>
<li>Metrics</li>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-pivot/">Pivot</a></li>
</ol>

Tech nerds loves metrics, right? I often hear people saying, that you should select 2-5 KPIs and stick with those. They are probably right in some way or form, but I'm all about the numbers. I follow up on the statistics for elmah.io monthly. I'm using a simple Google spreadsheet where I input a lot of metrics manually.

I collect everything from website visits to numbers of downloads of the elmah.io NuGet packages. While that sounds like a nightmare to maintain, doing it monthly actually don't take much more than ½ hour of my time every month. Obviously, the approach doesn't scale if I want to start following up more frequently. I've looked at tools like <a href="https://baremetrics.com/" target="_blank">Baremetrics</a> and <a href="https://chartmogul.com/" target="_blank">ChartMogul</a>. But for now, it is a manual process.

<img src="http://thomasardal.com/wp-content/uploads/2017/09/saas_charts-768x318.png" alt="" width="768" height="318" class="aligncenter size-medium_large wp-image-2065" />

To help collect numbers, I'm using a combination of tools like Google <a href="https://analytics.google.com/" target="_blank">Analytics</a>, <a href="https://adwords.google.com/home/" target="_blank">AdWords</a>, <a href="https://www.intercom.com/" target="_blank">Intercom</a>, <a href="https://www.paymill.com/" target="_blank">Paymill</a> and a homemade console app for calculating lifetime customer value (LCF), customer acquisition cost, and more mandatory SaaS metrics.

An important part of Lean Startup is the build-measure-learn loop. While build-measure learn relates more to some of the other posts (like MVP and Pivot), I've included it in this post, since metrics is a great part of this loop. When developing new features using the thoughts from MVP, I usually follow up using a combination of Google Analytics and a homemade feature dashboard. I use <a href="https://serilog.net/" target="_blank">Serilog</a> and <a href="https://www.elastic.co/products/elasticsearch" target="_blank">Elasticsearch</a> internally on elmah.io, to collect all sorts of metrics around the product. Logging a message through Serilog every time an email of a specific type is sent, when an uptime check is scheduled or a feature on the UI is used, makes it possible for me to create highly specialized dashboards in <a href="https://www.elastic.co/products/kibana" target="_blank">Kibana</a>. That way I have the perfect overview of how much each feature is used, if the feature constantly throws errors, etc. Feature dashboards is something I really recommend, especially during the MVP/beta phase. While I keep the dashboards around, the new feature dashboards typically shrink into smaller changes on a range of overall surveillance dashboards I have created for monitoring purposes.

A final note about using Elasticsearch and Serilog. When I tell people, I'm often asked why I'm not eating my own dog food and using elmah.io to monitor elmah.io. The short answer: I am. The longer answer: elmah.io isn't a platform for storing metrics, logging structured log messages and creating feature dashboards on the fly. I use elmah.io to implement a successful error management process around hosting elmah.io and Serilog/Elasticsearch/Kibana for the rest. A downside of using your own software is, that if elmah.io goes down, there's no elmah.io to tell me that the service went down :)

<em>Get 20% off elmah.io during GOTO Copenhagen 2017 by using the coupon code <code>LEANSTARTUPGOTOCPH</code></em>