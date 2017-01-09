---
ID: 381
post_title: Announcing NuGetFeed.org
author: Thomas Ardal
post_date: 2012-01-24 18:46:49
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/announcing-nugetfeed-org/
published: true
---
I don't think I ever got to announcing <a href="http://nugetfeed.org/" target="_blank">NuGetFeed.org</a>. NuGet Feed is a great extension to <a href="http://nuget.org/" target="_blank">NuGet</a>, which most .NET developers should know by now.  With NuGetFeed.org you will be able to follow your favorite NuGet package releases with a RSS reader of your choice.  Also, you will find a couple of plugins at the main NuGetFeed.org site.  One is for Google Chrome, which decorates the NuGet.org site with RSS links (it should really have been there from the beginning).  Another plugin is for Visual Studio, which lets you export all of your referenced NuGet packages to NuGetFeed.org.

<a href="http://thomasardal.com/wp-content/uploads/2012/01/nuget.png"><img class="alignnone size-full wp-image-504" title="nuget" src="http://thomasardal.com/wp-content/uploads/2012/01/nuget.png" alt="" width="620" height="490" /></a>

I wanted to tell you the story behind NuGetFeed.org, because it has been an interesting experience for me.  About four months ago, I spoke with two of my co-workers, <a href="https://twitter.com/#!/schultzdk" target="_blank">Jesper</a> and <a href="https://twitter.com/#!/chripede" target="_blank">Christian</a>, about the lack of possibilities of getting notified, when someone releases a new version of their NuGet package.  You have to manually open the NuGet Package Manager and search for updates, which I don't do that often.  I must have done a good job convincing them, because we quickly agreed to do something about the problem.  We could have joined the NuGet team, made pull requests or similar, but we decided to try another approach.

While Jesper started working on the graphics for the NuGetFeed.org website, Christian looked into doing the actual RSS feeds through the NuGet API.  I decided to use my existing knowledge about developing extensions for Google Chrome to make a kick-ass chrome plugin with links to NuGetFeed.org (pretty much a greasemonkey script) that would enhance visitors’ experience at the NuGet.org site.  Also, one of the initial ideas was to develop an add-in for Visual Studio, making it possible to export all of a project’s references to NuGetFeed.org.

We chose <a href="http://www.asp.net/mvc" target="_blank">ASP.NET MVC</a> for developing the website and <a href="http://twitter.github.com/bootstrap/" target="_blank">Twitter Bootstrap</a> for most of the UI.  Both frameworks totally kicks ass and allowed us to develop something that looks pretty nice, in only a short period of time.  For hosting, we decided to go with <a href="http://appharbor.com/" target="_blank">AppHarbor</a>, the Danish start-up which makes Azure look like mainframe-programming.  AppHarbor supports <a href="http://git-scm.com/" target="_blank">Git</a>, which is our preferred VCS at the moment.  Also, a single instance at AppHarbor is provided free of charge, as well as free <a href="http://www.mongodb.org/" target="_blank">MongoDB</a> hosting at <a href="https://mongohq.com/home" target="_blank">MongoHQ</a>.

After a week or so, the main service and plugins started to take shape.  Remember, this was done in out spare time.  I ended up using too much time on the Visual Studio add-in and, honestly, I was very disappointed with how difficult it was.  There's a new and shiny Visual Studio SDK using MEF, but I couldn't find documentation on the extension points we needed for our add-in.  Therefore, I ended up developing a VSPackage, which is the old way of doing things.  The whole thing felt a lot like coding COM, which I’d forgot all about years ago.  I think the reason for the lack of great add-ins for Visual Studio is caused by old API's which are difficult to develop against.  I really hope that Microsoft will improve the SDK over time, letting me port my nasty VSPackage to something sweeter.

Anyway, a week or so after the first night of coding, the first version of both the main site and the Chrome plugin were finished. We went live and got a few visits per day.  A couple of days after, the Visual Studio add-in went live on the Visual Studio Gallery and visits suddenly went through the roof.  I was amazed to see the impact in Analytics the day after the Visual Studio add-in release. People must be using the gallery after all.  Also, all new add-ins were published to the MSDN News Twitter account, which have almost 6,000 followers now.

A few days later, we were lucky enough to get mentioned on <a href="https://twitter.com/#!/calcock" target="_blank">Chris Alcock</a>’s blog <a href="http://blog.cwa.me.uk/" target="_blank">The Morning Brew</a> (TMB).  For those of you who don't know TMB, it's a great developer blog with a daily post summarizing yesterday’s most important news.  Again, we reached an all time record on the visit count on NuGetFeed.org.
<div>

NuGetFeed.org has been running for almost 6 months now and I'm happy to see that someone is using our service every day.

</div>