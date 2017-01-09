---
ID: 897
post_title: Brand new version of HippoValidator.com
author: Thomas Ardal
post_date: 2013-01-20 20:53:19
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/brand-new-version-of-hippovalidator-com/
published: true
---
Some positive Twitter feedback on my website validator <a href="http://www.hippovalidator.com/">HippoValidator</a> and release of the great <a href="http://webdevchecklist.com/">Web Developer Checklist</a>, motivated me to do what turned out as one of the largest updates since introducing the website:

<blockquote class="twitter-tweet"><p>@<a href="https://twitter.com/thomasardal">thomasardal</a> Didn't know you built that. Awesome work!</p>&mdash; Mads Kristensen (@mkristensen) <a href="https://twitter.com/mkristensen/status/288545890573111298" data-datetime="2013-01-08T07:21:15+00:00">January 8, 2013</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

<h2>What's new?</h2>
<p>I did multiple things in the recent version.</p>

<h3>Performance</h3>
<p>Working with HippoValidator.com always felt kind of slow. Probably because it was implemented during diaper change and hosted on a free micro instance at Amazon. Upgrading to a paid plan really kicks some serious performance ass. Also I have been working a lot with RavenDB since implementing the site, which allowed me to do multiple Raven related optimizations. Finally RavenDB is upgraded from the one year old version to the new and shiny 2.0. Performance really improved in RavenDB in the most recent version.</p>

<h3>SEO Validator</h3>
<p>I always wanted to do a SEO validator, why this was the first I decided to implement. I got my inspiration from various other SEO validators, personal experience and the great <a href="http://static.googleusercontent.com/external_content/untrusted_dlcp/www.google.com/da//webmasters/docs/search-engine-optimization-starter-guide.pdf" target="_blank">Search Engine Optimization Starter Guide</a> by Google. The new validator consists of 17 custom rules as we speak, but I have a lot more to implement.</p>

<h3>Mobile Validator</h3>
<p>Mobile just keeps getting more and more important. That's why you have to ensure that your site looks good on mobile platforms these days. I really wanted to integrate the mobileOK validator from W3C, but I couldn't find any webservice to use. I decided not to let that stop me and started to implement my own. In this version only three rules have been implemented, but again I have a lot of ideas to improve the validator with more rules.</p>

<h3>Stability Issues</h3>
<p>The previous version smelled a bit like beta. I've done a lot to stabilize the site, adding a lot of logging, bug fixes and a lot of other stuff.</p>

<p>Please help me and give the new version a try: <a href="http://www.hippovalidator.com/">Validate your website</a>.</p><br/><br/>