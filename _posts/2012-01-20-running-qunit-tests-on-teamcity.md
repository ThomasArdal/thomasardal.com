---
ID: 436
post_title: Running QUnit tests on TeamCity
author: Thomas Ardal
post_date: 2012-01-20 18:52:31
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/running-qunit-tests-on-teamcity/
published: true
---
Let's face it, it doesn't make sense to keep and maintain unit tests not executed on a continuous basis. In my <a href="http://thomasardal.com/2012/01/16/an-introduction-to-qunit/">previous post</a>, I looked at a unit test framework for JavaScript called <a href="http://docs.jquery.com/QUnit">QUnit</a>. While commonly used unit test runners like NUnit, JUnit and MSTest have great support on <a href="http://www.jetbrains.com/teamcity/" target="_blank">TeamCity</a>, there is no build-in support for executing QUnit tests (yet). During my recent adventures with QUnit, I wanted to be able to execute my new and shiny QUnit-tests on TeamCity. It turned out that it was almost as easy as integrating the supported framework.

Red Badger implemented a nice NuGet package called <a href="https://github.com/redbadger/QUnitTeamCityDriver" target="_blank">QUnitTeamCityDriver</a>. The package consists of two JavaScript-files: QUnitTeamCityDriver.js and QUnitTeamCityDriver.phantom.js. The first file contains logic to output results from running the QUnit tests to a format understandable by TeamCity. This file should be referenced just after the reference to QUnit.js in your tests. The second file is used on TeamCity to integrate with <a href="http://www.phantomjs.org/" target="_blank">PhantomJS</a>. PhantomJS is a windowless browser with a nice JavaScript API to control it all.

Before we dig into the configuration on TeamCity, we need to specify a HTML-container referencing all the js-files containing unit-tests. This is probably the biggest flaw right now because ReSharper does a nice job avoiding this container. What you need is a HTML-file looking that looks like this:

[html]
&lt;!DOCTYPE html&gt;
&lt;html&gt;
 &lt;head&gt;
 &lt;title&gt;JavaScript Tests&lt;/title&gt;
 &lt;link rel=&quot;stylesheet&quot; href=&quot;qunit.css&quot; /&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;jquery-1.7.1.js&quot;&gt;&lt;/script&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;qunit.js&quot;&gt;&lt;/script&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;QUnitTeamCityDriver.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;
 &lt;body&gt;
 &lt;h1 id=&quot;qunit-header&quot;&gt;QUnit example&lt;/h1&gt;
 &lt;h2 id=&quot;qunit-banner&quot;&gt;&lt;/h2&gt;
 &lt;div id=&quot;qunit-testrunner-toolbar&quot;&gt;&lt;/div&gt;
 &lt;h2 id=&quot;qunit-userAgent&quot;&gt;&lt;/h2&gt;
 &lt;ol id=&quot;qunit-tests&quot;&gt;&lt;/ol&gt;
 &lt;div id=&quot;qunit-fixture&quot;&gt;test markup, will be hidden&lt;/div&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;tests.js&quot;&gt;&lt;/script&gt;
&lt;/body&gt;
&lt;/html&gt;
[/html]

The file contains some basic HTML elements and references to jQuery, QUnit and QUnitTeamCityDriver. At the bottom, I reference the tests.js-file which I implemented in my previous blog post. So what's the main problem with this file? Each time a new js-file containing test are added to our project, we need to manually add a reference to this file at the bottom. I haven't found a nicer solution to this problem, so until a better approach emerges, I’ll continue using this file.

<del datetime="2013-01-01T10:22:08+00:00">In order to get the tests running on TeamCity, you need to download and extract PhantomJS on TeamCity. Unfortunately, PhantomJS doesn't come as a NuGet package, which is why you need to do this manually. Place PhantomJS in a folder like c:\phantomjs.</del>. In order to get the tests running on TeamCity, you need to reference <a href="http://nugetfeed.org/list/packages/phantomjs.exe/details" target="_blank">phantomjs</a> through NuGet. Add a new build step just after you run your unit tests called, "Run JS tests", or another name of your choice. Your settings should look like this:

<img src="http://thomasardal.com/wp-content/uploads/2013/01/qunitteamcity.png" alt="qunitteamcity" class="alignright size-full wp-image-853" />

In the text area to the right of "Command parameters", I reference two files. The first file is the QUnitTeamCityDriver.phantom.js-file which came with the QUnitTeamCityDriver NuGet-package. The second is a reference to the html-file created previously in this blog post. Save the build-step and Run the build. If everything looks green, you will be able to see the QUnit-tests alongside your NUnit-tests in the "Test" tab under each build. Simple and nicely integrated!