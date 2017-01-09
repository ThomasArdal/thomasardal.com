---
ID: 1293
post_title: >
  The ultimate guide to share Visual
  Studio settings within a team or
  organization
author: Thomas Ardal
post_date: 2014-04-07 12:19:22
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/the-ultimate-guide-to-share-visual-studio-settings-within-a-team-or-organization/
published: true
---
This guide will show you how to share code style settings between your team mates, using a variety of popular tools.

I bet you are working as part of a team. Also you are probably not an exact replica of your co-workers, am I right? Some like talking a lot about code style and some don't, but let's face it: if you are working on shared code base, you need some way of managing the style of the code shared across the team. The level of control depends on the discipline of the team members and sometimes external requirements from stakeholders or even customers.

<h2>Visual Studio</h2>

Visual Studio support settings sharing out of the box. Unfortunately you can't define different settings per solution or project. There's a <a href="https://connect.microsoft.com/VisualStudio/feedback/details/272773/visual-studio-user-settings-per-project" target="_blank">feature request</a> to make it happen, but Microsoft turned it down for unknown reasons.

To share you settings, select <em>Tools | Import and Export Settings...</em>

<img src="http://thomasardal.com/wp-content/uploads/2014/04/01.png" alt="01" width="627" height="561" class="aligncenter size-full wp-image-1294" />

Select the <em>Export selected environment settings</em> option and hit Next.

<img src="http://thomasardal.com/wp-content/uploads/2014/04/02.png" alt="02" width="627" height="561" class="aligncenter size-full wp-image-1296" />

Select the settings you want to share across your team. In the screenshot I've selected<em> All Settings | Options | Text Editor | All Languages</em>. This way all of the language formatting stuff is shared, while general settings about window placement and dimension are left to each developer to decide. Click next.

<img src="http://thomasardal.com/wp-content/uploads/2014/04/03.png" alt="03" width="627" height="561" class="aligncenter size-full wp-image-1297" />

Name the settings file a pick a directory. When done click Finish.

Your Visual Studio settings are now stored in a file named Exported-2014-04-07.vssettings in c:\temp\settings. Now it would be really sweet if you could add this file to your solution and Visual Studio automatically, add the file to source control and Visual Studio automatically picked up the settings for all of your co-workers. Keep dreaming! Everyone needs to import your settings file using the same dialog as used for exporting.

I really hope that Microsoft reconsiders supporting per solution team settings in the root directory.

<h2><a href="http://www.jetbrains.com/resharper/" target="_blank">ReSharper</a></h2>

Everyone uses ReSharper nowadays. Unlike Visual Studio itself, ReSharper has great support for sharing settings across a team. Select <em>ReSharper | Manage Options...</em>. You probably already spend some time tuning your current installation of ReSharper settings. To make everyone use your current settings, select This computer in the list and copy settings to <em>Solution "your project" team-shared</em>.

<img src="http://thomasardal.com/wp-content/uploads/2014/04/04.png" alt="04" width="700" height="500" class="aligncenter size-full wp-image-1298" />

Select the settings you want shared and hit OK.

<img src="http://thomasardal.com/wp-content/uploads/2014/04/05.png" alt="05" width="700" height="500" class="aligncenter size-full wp-image-1299" />

When the dialog is closed and the solution is saved, a new file called your_solution_name.sln.DotSettings is saved in the root directory of your solution. Add the DotSettings file to your version control system and ReSharper automatically discover and use the settings file on all checkouts of your repository.

<h2><a href="http://stylecop.codeplex.com/" target="_blank">StyleCop</a></h2>

Disclamer: Some like StyleCop, others don't. This is not an attempt to convince you to use StyleCop. It works on the project I'm working on, but I don't know if it works at your project. Try it and use it if you like it.

Sharing StyleCop settings work a bit like ReSharper setting. In fact, the StyleCop plugin for Visual Studio is ReSharper aware and installs itself as a ReSharper plugin. When first installed, the plugin will tell you if ReSharper and StyleCop settings conflicts in any way. You mostly want to let the plugin solve these conflicts. If not you will <a href="https://www.youtube.com/watch?v=YedqV4Gl_us" target="_blank">enter a world of pain</a> where StyleCop complains about formatting which ReSharper automatically enforce and vice versa. You get the picture.

When installed, the StyleCop plugin creates a new file in each project directoy named Settings.StyleCop. You may have to open the settings dialog to make the file appear, but right clicking each project and choose StyleCop Settings. As default all rules are enabled, but once you start deselecting some of the rules (trust me you'll end up there), you will see the Settings file starts updating. As with the ReSharper settings file, you should add this file to source control. StyleCop will automatically pick of this file when opening your solution.

<h2><a href="http://vswebessentials.com/" target="_blank">Web Essentials</a></h2>

Web Essentials is a great Visual Studio plugin for web developers. Like ReSharper and StyleCop, Web Essentials also supports team settings. In fact I <a href="http://webessentials.uservoice.com/forums/140520-general/suggestions/3146560-support-team-settings" target="_blank">proposed</a> that feature back in 2012 :)

To share your Web Essentials settings, select Web Essentials | Create solution settings. This creates a file in the solution root directory named WE2013-settings.xml. This file should be added to source control and after that, Web Essentials pick up the settings automatically.