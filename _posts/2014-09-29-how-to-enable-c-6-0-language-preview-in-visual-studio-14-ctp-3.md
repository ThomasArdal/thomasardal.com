---
ID: 1430
post_title: 'How To Enable C# 6.0 Language Preview in Visual Studio 14 CTP 3'
author: Thomas Ardal
post_date: 2014-09-29 10:52:14
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/how-to-enable-c-6-0-language-preview-in-visual-studio-14-ctp-3/
published: true
---
<img src="http://thomasardal.com/wp-content/uploads/2014/09/3426.image_3C5B41B8.png" alt="3426.image_3C5B41B8" width="150" height="135" class="alignright size-thumbnail wp-image-1435" />I attended a session at GOTO by Mads Torgersen, the program manager on the Microsoft C# team. The session where about <a href="http://gotocon.com/aarhus-2014/presentations/show_presentation.jsp?oid=6227" target="_blank">The Future of C#</a> and I have been wanting to dig into this myself since Visual Studio 14 CTP 3 came out. I plan on writing a number of blog posts about the new features of C# 6.0. A lot of people already wrote about this subject and there will of course be an overlap with something you have already read. I will try to give my point of view on the new features, as well as dig a little deeper into how this is implemented.

This being the first post and all, I want to start by sharing the formula for getting access to C# 6.0 inside Visual Studio. I were a bit surprised, the Visual Studio 14 CTP 3 didn't allowed access without any configuration. VS 14 CTP 3 includes a new version of the .NET framework called vNext, which could have been in the framework selector on the New Project wizard. Either I'm missing something, or Mads didn't tell about this little magic trip to play with the new version.

To start writing some C# 6.0 wonders, <a href="http://support.microsoft.com/kb/2967191" target="_blank">download</a> and install Visual Studio 14 CTP 3.

Create a new Console Application:

<a href="http://thomasardal.com/wp-content/uploads/2014/09/CreateProject.png"><img src="http://thomasardal.com/wp-content/uploads/2014/09/CreateProject-580x400.png" alt="CreateProject" width="580" height="400" class="aligncenter size-medium wp-image-1431" /></a>

Right click the new project and Unload:

<a href="http://thomasardal.com/wp-content/uploads/2014/09/UnloadProject.png"><img src="http://thomasardal.com/wp-content/uploads/2014/09/UnloadProject.png" alt="UnloadProject" width="560" height="611" class="aligncenter size-full wp-image-1433" /></a>

Right click the new project and edit the csproj file:

<a href="http://thomasardal.com/wp-content/uploads/2014/09/EditProject.png"><img src="http://thomasardal.com/wp-content/uploads/2014/09/EditProject-580x251.png" alt="EditProject" width="580" height="251" class="aligncenter size-medium wp-image-1434" /></a>

Locate the PropertyGroup looking like this:
 
<pre class="lang:xhtml decode:true " >&lt;PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' "&gt;</pre> 

Add a new element to that property group named LangVersion with the value Experimental:
 
<pre class="lang:xhtml decode:true " >&lt;PropertyGroup Condition=" '$(Configuration)|$(Platform)' == 'Debug|AnyCPU' "&gt;
    &lt;PlatformTarget&gt;AnyCPU&lt;/PlatformTarget&gt;
    &lt;DebugSymbols&gt;true&lt;/DebugSymbols&gt;
    &lt;DebugType&gt;full&lt;/DebugType&gt;
    &lt;Optimize&gt;false&lt;/Optimize&gt;
    &lt;OutputPath&gt;bin\Debug\&lt;/OutputPath&gt;
    &lt;DefineConstants&gt;DEBUG;TRACE&lt;/DefineConstants&gt;
    &lt;ErrorReport&gt;prompt&lt;/ErrorReport&gt;
    &lt;WarningLevel&gt;4&lt;/WarningLevel&gt;
    &lt;LangVersion&gt;Experimental&lt;/LangVersion&gt;
&lt;/PropertyGroup&gt;</pre> 

Reload the project and you are ready to rock and roll.

In the next post I take a look at <a href="http://thomasardal.com/auto-properties-with-initializers-in-c-6-0/">Auto-Properties with Initializers</a>.