---
ID: 1252
post_title: Setting up a NuGet server with ProGet
author: Thomas Ardal
post_date: 2014-03-10 07:34:24
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/setting-up-a-nuget-server-with-proget/
published: true
---
Ever wondered how to setup your own <a href="http://www.nuget.org/" target="_blank">NuGet</a> server? nuget.org have <a href="http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds" target="_blank">a guide</a> and I actually hosted a couple of feeds that way in the past. At my <a href="http://www.ebaycareers.com/" target="_blank">day job</a> we implement a lot of micro services. Some of these services distribute client libraries through NuGet and they all gets deployed to different environments using <a href="http://octopusdeploy.com/" target="_blank">Octopus</a> deploy. Octopus is based on NuGet as well, why we need to be able to host multiple private feeds for various purposes. Having wet dreams about build environments already, made this an excellent challenge for a guy like me: settings up multiple NuGet feeds to support both private feeds and Octopus.

I began looking at different technologies to implement this:
<ul>
	<li><a href="http://www.sonatype.org/nexus" target="_blank">Nexus</a>: supports NuGet, but it is pretty expensive.</li>
	<li><a href="https://www.myget.org/" target="_blank">MyGet</a>: provides NuGet feeds in the cloud. Big companies like eBay doesn't really like hosting anything outside its own cloud.</li>
	<li><a href="http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds" target="_blank">Self-hosted feeds</a>: quickly becomes a pain, because I would need multiple web projects hosted on an IIS, in order to get multiple feeds.</li>
	<li>Implementing it myself: would be fun but too time consuming.</li>
</ul>
That’s when a co-worker mentioned <a href="http://inedo.com/proget/overview" target="_blank">ProGet</a>. What is ProGet?
<blockquote>ProGet is a NuGet package repository that lets you host and manage your own personal or enterprise-wide NuGet feeds.</blockquote>
Great! Someone already build the tool I have wanted to build since NuGet launched. Let’s look at how to setup a new ProGet installation:
<ol>
	<li>Navigate to <a href="http://inedo.com/proget/overview">http://inedo.com/proget/overview</a> and download ProGet.</li>
	<li>For starters check the Free Edition:<br/><img class="alignright size-medium wp-image-1253" alt="01" src="http://thomasardal.com/wp-content/uploads/2014/03/01-580x456.png" width="580" height="456" /></li>
    <li>Input your email and name:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/02-580x456.png" alt="02" width="580" height="456" class="alignright size-medium wp-image-1254" /></li>
    <li>Just accept the default installation directory, unless you have some weird perversion about where to put your files.<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/03-580x456.png" alt="03" width="580" height="456" class="alignright size-medium wp-image-1255" /></li>
    <li>ProGet can run entirely self-hosted or using external ressources. In this installation I'm choosing to use an embedded SQL Express:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/04-580x456.png" alt="04" width="580" height="456" class="alignright size-medium wp-image-1258" /></li>
    <li>Like in the previous step, I'm choosing to use the integrated web server. ProGet can run on IIS as well but in my case, I just want a simple setup without external dependencies. As default ProGet wants to run on port 81. I've changed the port to 80, because ProGet will be the only HTTP aware process on the host:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/05-580x456.png" alt="05" width="580" height="456" class="alignright size-medium wp-image-1259" /></li>
    <li>ProGet is hosted as a Windows Service. In the account settings, I’m going with the default settings, which is running the service as Network Service:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/06-580x456.png" alt="06" width="580" height="456" class="alignright size-medium wp-image-1260" /></li>
    <li>Review the settings and hit Install:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/07-580x456.png" alt="07" width="580" height="456" class="alignright size-medium wp-image-1261" /></li>
    <li>That's it. ProGet were successfully installed. Hit the slightly oversized Launch ProGet button:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/08-580x456.png" alt="08" width="580" height="456" class="alignright size-medium wp-image-1262" /></li>
    <li>The frontpage of ProGet is actually pretty useless for my purpose. I always use Visual Studio and <a href="http://npe.codeplex.com/" target="_blank">NuGet Package Explorer</a> to browse packages. Adding packages to ProGet, is something I would like to do as a build step on TeamCity or similar. Hit the Gear icon in the upper right corner:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/09-580x425.png" alt="09" width="580" height="425" class="alignright size-medium wp-image-1266" /></li>
    <li>Login and you will be presented by the great part of ProGet. The admin lets you setup system settings, user and roles and probably the most exciting feature: feeds. Click the Manage Feeds link:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/11-580x395.png" alt="11" width="580" height="395" class="alignright size-medium wp-image-1267" /></li>
    <li>The feeds page shows a list of all your NuGet feeds. As default ProGet adds a single feed named Default. You can setup multiple feeds, which in my case is used to make individual feeds for third-party packages, internal packages and deployment packages. Hit the small pencil icon next the Default feed:<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/12-580x395.png" alt="12" width="580" height="395" class="alignright size-medium wp-image-1269" /></li>
    <li>The Feed Properties page lets you configure the Default feed. ProGet have a great feature, where it will actually cache all downloaded packages locally. This way you can use ProGet as a proxy/cache of the unstable nuget.org.<br/><img src="http://thomasardal.com/wp-content/uploads/2014/03/13-580x395.png" alt="13" width="580" height="395" class="alignright size-medium wp-image-1280" /></li>
    <li>The Connectors paragraph makes it possible to add multiple NuGet sources to the Default feed. This way you can use ProGet to aggregate NuGet packages from multiple sources. Unfortunately, ProGet doesn't support the NuGet feed published by TeamCity, because Jetbrains choose not to support NuGet version 2.</li>
</ol>

There you have it. A walk through of the basic ProGet features. ProGet turned out as a great product, able to host internal packages, cache third-party packages and aggregate multiple feeds.