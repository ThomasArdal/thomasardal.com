---
ID: 2036
post_title: 'Lean Startup on elmah.io &#8211; Continuous deployment'
author: Thomas Ardal
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/lean-startup-on-elmah-io-continuous-deployment/
published: true
post_date: 2017-09-12 17:35:32
---
This is the third post in a series about Lean Startup on <a href="https://elmah.io/">elmah.io</a>. Check out the other posts here:

<ol>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-introduction/">Introduction</a></li>
<li><a href="http://thomasardal.com/lean-startup-on-elmah-io-minimum-viable-product/">Minimum viable product</a></li>
<li>Continuous deployment</li>
<li>Split testing</li>
<li>Metrics</li>
<li>Pivot</li>
</ol>

I'm on a path to release every change to production. All of my web apps are automatically deployed to <a href="https://azure.microsoft.com/da-dk/" target="_blank">Microsoft Azure</a>, using the deployment engine <a href="https://github.com/projectkudu/kudu" target="_blank">Kudu</a>. I'm using staging environments for the web apps and auto-swap into production after warming up the website (as described <a href="https://docs.microsoft.com/en-us/azure/app-service-web/web-sites-staged-publishing#configure-auto-swap" target="_blank">here</a>). I still click swap manually on the user facing websites, but that's something that I'm working on eliminating as well.

Most of the background services on elmah.io are run as <a href="https://azure.microsoft.com/en-us/services/functions/" target="_blank">Azure Functions</a>. Like web apps, Kudu automatically deploys all changes to Microsoft Azure. Unlike web apps, Functions are deployed directly to production. Why the difference you may think? Well, it's simply a matter of history. All the services written as Azure Functions, are built with continuous deployment in mind, while the web apps still miss a few points. Furthermore, all the services run async from the normal usage on elmah.io either through a schedule (like daily digest email) or are triggered by messages on an event topic (like handling a new error report from the user).

<img src="http://thomasardal.com/wp-content/uploads/2017/09/azure_deployments-768x486.png" alt="" width="768" height="486" class="aligncenter size-medium_large wp-image-2061" />

I sometimes need to do some stuff when deploying new versions of the software (like sending a Slack notification or calling the elmah.io deployments endpoint to make <a href="https://elmah.io/features/#deploymenttracking" target="_blank">Deployment Tracking</a> work). I use a combination of <a href="https://zapier.com/" target="_blank">Zapier</a> and custom <a href="https://github.com/projectkudu/kudu/wiki/Post-Deployment-Action-Hooks" target="_blank">Post Deployment scripts</a>. Zapier is a great platform for connecting different services. You can think of it as <a href="https://ifttt.com/" target="_blank">IFTTT</a> for SaaS applications. Post deployment scripts is a feature in Azure, which can run PowerShell scripts as part of the deployment magic happening inside Kudu.

<img src="http://thomasardal.com/wp-content/uploads/2017/09/slack-768x147.png" alt="" width="768" height="147" class="aligncenter size-medium_large wp-image-2059" />

I've used both <a href="https://octopus.com/" target="_blank">Octopus Deploy</a> and <a href="https://www.visualstudio.com/team-services/" target="_blank">Visual Studio Team Services</a> with great success in the past. Implementing continuous deployment using one of those tools has often surfaced in my mind, but Kudu works so good, that the cost benefits of switching simply don't add up.