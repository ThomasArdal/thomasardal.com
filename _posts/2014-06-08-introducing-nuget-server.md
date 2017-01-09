---
ID: 1332
post_title: Introducing NuGet Server
author: Thomas Ardal
post_date: 2014-06-08 19:48:02
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/introducing-nuget-server/
published: true
---
A couple of months ago I had to install a simple NuGet server on an internal server. There's a few options working out of the box like ProGet and MyGet. For various reasons I couldn't host my feed in the cloud and ProGet sort of felt like overkill combined with the fact that I've had varying experiences with query performance. <a href="http://docs.nuget.org/docs/creating-packages/hosting-your-own-nuget-feeds" target="_blank">docs.nuget.org</a> has a great tutorial on how to install your own feed, using the <a href="http://www.nuget.org/packages/NuGet.Server/" target="_blank">NuGet.Server</a> package.

I decided to go with the self hosted feed from docs.nuget.org. An approach I've tried in the past so the solution seemed straight forward. Following the guide I realized that it consists of at least 5 manual steps not even including installing and setting up IIS. Business idea appeared out of the blue: A simple NuGet server based on NuGet.Server hosted outside IIS as a Windows service and installed through an installer:

Meet <a href="http://nugetserver.net/">NuGetServer.net</a>.

<img src="http://nugetserver.net/gfx/logo.png" width="128" height="128" class="alignright" />I'll probably never get rich developing NuGet Server, but at least I got to play with Cassini, Topshelf and Inno Setup.

For 5 bucks you get a new and shiny NuGet server installed through a wizard hosted as a Windows service. No fancy features like license filtering and feed aggregation in there, but on the other hand you know that you are running on the official NuGet server software.

Plans for future releases:

<ul>
<li>Make a cooler configuration page</li>
<li>Support pushing packages (should really be a NuGet.Server thing)</li>
<li>Auto-update (consider this: a NuGet server updating the internal NuGet.Server package through NuGet === sweet!!1)