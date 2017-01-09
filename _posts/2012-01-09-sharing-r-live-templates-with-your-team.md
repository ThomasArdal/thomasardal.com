---
ID: 434
post_title: 'Sharing R# Live Templates With Your Team'
author: Thomas Ardal
post_date: 2012-01-09 07:10:54
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/sharing-r-live-templates-with-your-team/
published: true
---
I found myself repeatable adding the same Live Template to R# over and over again: The unit-test template. This template adds a method with a method name starting with <em>Can</em> as well as the arrange/act/assert comments, which I'm pretty much in love with. Each time I re-install my old machine or simply get a new one, this is the first template I import. The other day I wanted to share this template with my team-members, but wanted to do something else than just exporting the template and sharing it by mail. It turns out that R# have a great feature to share stuff between team members. I already use this to share R# settings, as well as StyleCop settings (which is available as a R# plugin). The sharing feature also supports live templates, which I was not aware of.

To share your live templates with your team members, navigate to the Templates Explorer in the ReSharper menu inside Visual Studio. On my machine, the <em>This computer</em> layer was selected as shown here:

<a href="http://thomasardal.com/wp-content/uploads/2012/01/livetemplate1.png"><img class="alignnone size-full wp-image-452" title="livetemplate1" src="http://thomasardal.com/wp-content/uploads/2012/01/livetemplate1.png" alt="" width="666" height="619" /></a>

Select the test-template and export it by clicking the <em>Export</em>-icon. Change the layer to <em>Solution 'Your SLN' team-shared</em> and import the file. Close Visual Studio to make sure that the DotSettings file is updated. If not already added, add the file named &lt;your sln&gt;.DotSettings to source control and the live template is shared with your team members, the next time they get that file. Simple and neat, right?