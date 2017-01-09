---
ID: 1209
post_title: >
  Performance monitoring .NET web
  applications with StatsD and Graphite
author: Thomas Ardal
post_date: 2013-12-04 11:11:07
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/performance-monitoring-net-web-applications-with-statsd-and-graphite/
published: true
---
During the last months at <a href="http://jobs.ebaycareers.com/dk/aarhus-jobs" target="_blank">work</a>, we’ve embraced a new technology called <a href="http://graphite.wikidot.com/" target="_blank">Graphite</a>. In simple terms Graphite is a daemon able to collect data and present them in various graphs. Want to show a chart showing number of daily requests? Graphite is your friend. Want to show the development in user sign ups? Graphite is your friend. Want to … you probably get it by now. The collection engine in Graphite called Carbon, is extremely good at collecting data as well as serves the data to other types of applications through a HTTP API. In this post I will show you how to integrate graphite into your .NET web projects.

Let’s spend a little time looking at how metrics are added to Graphite. Graphite provides different ways of feeding it. The easiest solution is to choose one of the tools able to push data into Graphite. A guy at work already picked <a href="https://github.com/etsy/statsd/" target="_blank">StatsD</a> and it turned out as a great component for aggregating data. StatsD is pretty much a simple API based on UDP, collecting data and shipping them off to Graphite. StatsD provides standard metric types like counters and timers and luckily this component handles all of the communication with Graphite. Boiled down StatsD collect, aggregate and send metrics to Graphite, which stores and serves it.

<img class="alignnone size-full wp-image-1214" alt="graphite" src="http://thomasardal.com/wp-content/uploads/2013/11/graphite.png" width="429" height="248" />

With that background info in place, we can start playing with Graphite. The software is developed for Linux, which makes it chill down every .NET developers back. Luckily, <a href="https://twitter.com/suanyeo" target="_blank">Suan-Aik Yeo</a> made it darn simple installing both Graphite and StatsD on a Windows machine. I won’t go into detail about the installation, because it is pretty well documented in the <a href="https://github.com/suan/graphite_up" target="_blank">graphite_up</a> project. In short, you will use <a href="http://www.vagrantup.com/" target="_blank">Vagrant</a> to spin up a virtual machine with Graphite and StatsD installed and configured. I had a lot of problems selecting the right versions of Vagrant and <a href="https://www.virtualbox.org/" target="_blank">VirtualBox</a>, to get graphite_up working. The following versions work with the current revision of graphite_up:
<ul>
	<li>Vagrant version 1.2.2 (<a href="http://downloads.vagrantup.com/tags/v1.2.2">http://downloads.vagrantup.com/tags/v1.2.2</a>)</li>
	<li>VirtualBox version 4.2.14 (<a href="https://www.virtualbox.org/wiki/Download_Old_Builds_4_2">https://www.virtualbox.org/wiki/Download_Old_Builds_4_2</a>)</li>
</ul>
When setup run your virtual machine:

<img class="alignnone size-full wp-image-1215" alt="graphite_up" src="http://thomasardal.com/wp-content/uploads/2013/12/graphite_up.png" width="677" height="342" />

You’re ready to rock and roll. Start Visual Studio and create a new ASP.NET MVC project:

<img class="alignnone  wp-image-1216" alt="create_mvc_project" src="http://thomasardal.com/wp-content/uploads/2013/12/create_mvc_project.png" width="669" height="462" />

In order for your project to be able to talk to StatsD, you need a package able to send UDP packages. You can use the UdpClient in .NET, but the easiest way of talking to StatsD, is to include <a href="https://github.com/goncalopereira/statsd-csharp-client" target="_blank">C# Statsd Client</a> by <a href="https://twitter.com/pereiragoncalo" target="_blank">Goncalo Pereira</a>:

<img class="alignnone  wp-image-1217" alt="install_statsd_nuget" src="http://thomasardal.com/wp-content/uploads/2013/12/install_statsd_nuget.png" width="637" height="68" />

Let’s start by doing something simple with our new project and StatsD. I want to log how long each request takes and show that as a graph in Graphite. For the purpose I’ve added a new controller named HomeController with a single method Index:

<script src="https://gist.github.com/ThomasArdal/7785332.js?footer=0"></script>

I’ve added a random sleep period to the action, in order to have something to show on the graph. Next up we need to log data to StatsD. I could add the StatsD code directly to the action, but I don’t really like having logging code, performance measuring code, exception handling code and so on in the controller directly. Let’s specify a new global filter instead:

<script src="https://gist.github.com/ThomasArdal/7785345.js"></script>

In order for StatsdClient to know where to send the data, you need to configure that in your global.asax.cs:

<script src="https://gist.github.com/ThomasArdal/7785356.js"></script>

And finally configure the new action filter as a global filter in global.asax.cs:

<script src="https://gist.github.com/ThomasArdal/7785360.js"></script>

Start the website and hit the frontpage a couple of times to start generating some data. Then navigate to <a href="http://localhost:8080/">http://localhost:8080</a>, which should open the Graphite UI. Your metrics are stored beneath the Graphite/stats/timers/myMvcApp. In the following sample, I’ve selected mean_90 on the Index action from Home and configured the graph to show data from the last 5 minutes:

<a href="http://thomasardal.com/wp-content/uploads/2013/12/graphite_graph.png"><img class="alignnone  wp-image-1221" alt="graphite_graph" src="http://thomasardal.com/wp-content/uploads/2013/12/graphite_graph.png" width="648" height="365" /></a>