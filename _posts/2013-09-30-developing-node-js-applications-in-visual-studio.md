---
ID: 1132
post_title: >
  Developing node.js applications in
  Visual Studio
author: Thomas Ardal
post_date: 2013-09-30 10:09:07
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/developing-node-js-applications-in-visual-studio/
published: true
---
<span style="font-size: 13px;">I'm not much of a notepad programmer. I like having tools supporting me while developing software. That's why I needed some software for my recent node.js endeavours. There's some nice tools available, but I really wanted to be able to develop, run and debug node.js in Visual Studio. A quick sweep on Google led me to two VS extensions, both promising support for node.js inside VS: <a href="http://www.visualnode.info/" target="_blank">Visual Node</a> and <a href="http://visualstudiogallery.msdn.microsoft.com/885a8a68-e38b-4e6a-b96d-083d5572b645" target="_blank">node.js Tools for Visual Studio</a>. Visual Node is from redgate and looks really cool. The stuff is in beta at them moment and I expect it to become paid software. I decided to look at node.js Tools for Visual Studio which is open source but a lot less shiny. It's in version 0.1 and that clearly showed. During the installation I got about three different cryptic error messages, one requiring me to install a service patch from Microsoft.

When finally installed, creating a new node.js project is easy:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/nodejs_1.png"><img class="alignnone  wp-image-1145" alt="nodejs_1" src="http://thomasardal.com/wp-content/uploads/2013/09/nodejs_1.png" width="669" height="462" /></a>

When created a new project is added to the Solution Explorer, just as any other project type in VS:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/node_2.png"><img class="alignnone size-full wp-image-1147" alt="node_2" src="http://thomasardal.com/wp-content/uploads/2013/09/node_2.png" width="414" height="132" /></a>

The nice thing here is that the folder structure now reflects a node.js and not a default .NET application (with Properties, References etc.). Like NuGet provides the Package Manager Console, the node.js extension provides the Node Package Manager embedding npm. Installing new packages is as simple as installing a NuGet package:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/node_3.png"><img class="alignnone  wp-image-1148" alt="node_3" src="http://thomasardal.com/wp-content/uploads/2013/09/node_3.png" width="516" height="194" /></a>

http-server package installed. Success! Look how the folder structure represents this:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/node_4.png"><img class="alignnone size-full wp-image-1149" alt="node_4" src="http://thomasardal.com/wp-content/uploads/2013/09/node_4.png" width="416" height="189" /></a>

Great! Let's code a bit of JavaScript:

<script src="https://gist.github.com/ThomasArdal/6761640.js"></script>

Completely stolen from <a href="http://howtonode.org/hello-node">http://howtonode.org/hello-node</a> BTW. Exiting moment: hitting F5 and accessing http://localhost:8000 in the browser. The text Hello World appeared. That's easy, right? Next up: debugging! I set a breakpoint on line 6 and re-hitting F5. When visiting the URL in the browser, the following is happening:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/node_5.png"><img class="alignnone  wp-image-1153" alt="node_5" src="http://thomasardal.com/wp-content/uploads/2013/09/node_5.png" width="601" height="239" /></a>

The debugger actually stopped in line 6. Hovering variables also works:

<a href="http://thomasardal.com/wp-content/uploads/2013/09/node_6.png"><img class="alignnone  wp-image-1154" alt="node_6" src="http://thomasardal.com/wp-content/uploads/2013/09/node_6.png" width="290" height="250" /></a>

I'm happy! If you have luck to install this extension, node.js development suddently gets a first class citizen in Visual Studio.