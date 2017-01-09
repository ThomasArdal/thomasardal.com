---
ID: 1891
post_title: >
  My experience with Azure Functions so
  far
author: Thomas Ardal
post_date: 2016-10-13 11:31:25
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/my-experience-with-azure-functions-so-far/
published: true
---
This is a post about <a href="https://azure.microsoft.com/en-us/services/functions/" target="_blank">Azure Functions</a> - the new "serverless" computing feature from Microsoft currently in Preview. I was introduced to Functions (WebJobs vNext at the time) back in February as part of an insider preview for Microsoft MVPs. Having read a bit about AWS Lambdas, I had great expectations to Functions. During the last couple of months, I've played with Functions a couple of times in the hopes of finding a better solution of scaling service bus consumers on my startup <a href="https://elmah.io/">elmah.io</a>. When I tried out the Functions preview a couple of months ago, I were disappointed with the poor user experience and the constant flow of error messages in the portal.

Today I gave Functions another try and probably spread my excitement a bit early on Twitter:

<blockquote class="twitter-tweet" data-lang="en"><p lang="en" dir="ltr">Playing with Azure Functions for the third time today. Finally feels stable and offering the features that I need (besides point-to-site).</p>&mdash; Thomas Ardal (@ThomasArdal) <a href="https://twitter.com/ThomasArdal/status/786466837802455040">October 13, 2016</a></blockquote>
<script async src="//platform.twitter.com/widgets.js" charset="utf-8"></script>

My conclusion still is that Functions aren't ready for production. You may think "well yeah, it says Preview on the website". And you're right. You should probably never use Preview software in production, but I think that other Preview services on Azure have worked quite well actually.

Here are my comments about Functions so far.

<ul>
	<li>I don't understand why Functions needs their own dedicated site (<a href="http://functions.azure.com" target="_blank">functions.azure.com</a>). Everything else is located around the Portal and even though the functions UI is integrated on the Portal, it feels weird navigating between the two UI's. I hope this is something that will change in the future. What’s even worse is, that if you create functions on the functions subdomain, a lot of choices about resource group and storage account aren’t available.</li>
	<li>Functions still feel unstable. Not as bad as a couple of months ago, but I still received weird error messages in the UI from time to time.<br/><img src="http://thomasardal.com/wp-content/uploads/2016/10/error.png" alt="error" width="362" height="193" class="aligncenter size-full wp-image-1899" /></li>
	<li>There's no tool support what so ever. Implementing Functions happens in a textbox in the portal. You can use Visual Studio to edit the JSON files and make automatic deployments with Kudo (like web apps), but there's no help creating a function inside Visual Studio. Microsoft <a href="https://twitter.com/crandycodes/status/786514542956347393" target="_blank">reported</a> on Twitter, that tool support will arrive in the next version of VS, though. Being able to install NuGet packages in a Function is a must have and that is only possible by manually adding a Project.json file.</li>
	<li>Point-to-site VPN isn't supported in the dynamic plan on Functions. This means that you won’t be able to communicate securely from a Function to a virtual network on Azure. I know that we har shifting against serverless architectures, but most companies still have virtual machines running why this is essential. At least to me.</li>
</ul>

Listing all of the bad things, sort of give the impression that I don’t like Functions. That’s not the case, though. I believe that Functions will be HUGE. Very much looking forward to the RTM. As usual, Microsoft are very active on Twitter and all of my answers and frustrations were answered by either Azure Support or MS employees.