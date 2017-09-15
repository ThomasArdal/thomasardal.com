---
ID: 2043
post_title: 'Lean Startup on elmah.io &#8211; Pivot'
author: Thomas Ardal
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/lean-startup-on-elmah-io-pivot/
published: true
post_date: 2017-09-15 16:02:13
---
This is the sixth and final post in a series about Lean Startup on <a href="https://elmah.io/">elmah.io</a>. Check out the other posts here:

<ol>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-introduction/">Introduction</a></li>
<li>Minimum viable product</li>
<li>Continuous deployment</li>
<li>Split testing</li>
<li>Metrics</li>
<li>Pivot</li>
</ol>

When looking back on the last 4 years, elmah.io has pivoted multiple times. My original idea with elmah.io (hence the name), were to implement a remote logger for <a href="https://elmah.github.io/" target="_blank">ELMAH</a> (the open source project). Being a frequent ELMAH user, I got tired of the limited search capabilities and setting up SQL Servers. So elmah.io was born.

<img src="http://thomasardal.com/wp-content/uploads/2017/09/pivot.gif" alt="" width="320" height="320" class="aligncenter size-full wp-image-2068" />

First pivot came around 2014. I talked with a lot of people who liked the product, but didn't see huge benefits over ELMAH. I think it actually had back then already, but I was not good enough at communicating that.

<blockquote>It's a common mistake for entrepreneurs with technical background as myself, to think that visitors think that the product is as cool as you do yourself. Communicating features are often communicated in a lot of technical terms, rather than highlighting the why's.</blockquote>

The pivot where a number of integrations for popular .NET logging frameworks and a major redesign to support structured log messages with severities other than just errors. At the moment, my belief in the product where, to compete with successful products like Logentries and even <a href="https://www.elastic.co/products/elasticsearch" target="_blank">Elasticsearch</a>. I even started thinking about (and even coded a bit) on a new metrics product, which were meant as a supplement for the logging part, able to log everything from website clicks to performance numbers.

During the next year or so, people started to be confused. No-one (even me) knew if elmah.io was a cloud logging platform for structured log messages or an error management system for websites. After spending quite a lot of time being confused and trying to explain to users what elmah.io was and it wasn't, I decided to change the path of the product once again. In many ways, this pivot returned elmah.io back to its original vision: Providing error management for .NET web applications. This time not only using ELMAH, but for any logging- and web-framework.

In the last year, elmah.io has gained a lot of new features. Features like Deployment Tracking and Uptime Monitoring were added to the suite, and while they can be looked upon as separate products, they very much support the vision of delivering the best error management system for .NET web applications.