---
ID: 927
post_title: HippoValidator vNext
author: Thomas Ardal
post_date: 2013-03-19 20:49:41
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/hippovalidator-vnext/
published: true
---
Time for another update on my progress on making <a href="http://www.hippovalidator.com" target="_blank">HippoValidator</a> the best website validator out there. I just pushed a major update including a lot of features and fixes. The most important ones are listed below.
<h2></h2>
<h2>JSHint</h2>
HippoValidator already validated some aspects of the JavaScript on your site. But a validator dedicated to writing good Javascript were always missing. Until now. JSHint will now validate all script tags on your site.
<h2></h2>
<h2>CSSLint</h2>
Like JavaScript, HippoValidator also looks at your stylesheets. Both the W3C style validator as well as the Accessibility validator look at your stylesheets, but focus more on the standard and less on writing quality code. Luckily CSSLint now runs as part of the scan, helping you avoid common mistakes when writing CSS.
<h2></h2>
<h2>Settings page</h2>
While HippoValidator runs a set of quality recognized rules, you may want to disable certain rules or maybe entire validators from your scheduled scans. This is now possible through the new Settings pages, which is accessed by clicking the small edit-icon next to your schedules. The settings page looks like this:

<a href="http://thomasardal.com/wp-content/uploads/2013/03/settings.png"><img class="size-full wp-image-943 alignnone" alt="settings" src="http://thomasardal.com/wp-content/uploads/2013/03/settings.png" width="620" height="391" /></a>
<h2></h2>
<h2>Optimize images</h2>
The validators always focused a lot on telling you what's wrong and no so much on how to fix it. My vision is to provide you with both links and tools to help you fix the problems found by HippoValidator. A good example of this is the new optimize image fix, which is available if the performance validator shows the "Optimize Images" error. When showed and the user clicks the error, an extended explanation is shown like this:

<a href="http://thomasardal.com/wp-content/uploads/2013/03/optimizeimages.png"><img class="size-full wp-image-944 alignnone" alt="optimizeimages" src="http://thomasardal.com/wp-content/uploads/2013/03/optimizeimages.png" width="564" height="282" /></a>

HippoValidator automatically shows you the images which needs optimization and when clicking the "Optimize image" link, an optimized image is saved to you file system. You can replace the existing file on your website with the optimized one and gain a small performance boost.
<h2></h2>
<h2>Open source</h2>
What started as a small task, ended up taking a lot of time during this release: open sourcing some of the internals of HippoValidator. So far I've open sourced 4 of the .NET clients I've written for different validator services: JSHint, CSSLint, AChecker Accessibility Validator, W3C CSS Validator. You can find the projects at the HippoValidator organization at GitHub here: <a href="https://github.com/HippoValidator" target="_blank">https://github.com/HippoValidator</a>. I plan to open source even more and really appreciate YOUR help using and extending these validators.
<h3></h3>
<h3>What's next?</h3>
I still have a lot of ideas for new features and improvements. This is what I'm planning to do next:
<ul>
	<li><span style="line-height: 13px;">More to Azure.</span></li>
	<li>More fixes</li>
	<li>Email alerts</li>
	<li>Migrate everything to Bootstrap</li>
</ul>
Please add your ideas and bug reports to the HippoValidator UserVoice: <a href="http://hippovalidator.uservoice.com/forums/105829-general" target="_blank">http://hippovalidator.uservoice.com/forums/105829-general</a>.