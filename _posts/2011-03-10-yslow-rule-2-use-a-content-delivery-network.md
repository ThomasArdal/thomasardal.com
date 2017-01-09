---
ID: 166
post_title: 'YSlow rule #2 &#8211; Use a Content Delivery Network'
author: Thomas Ardal
post_date: 2011-03-10 17:43:51
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-2-use-a-content-delivery-network/
published: true
---
In this post I will try to explain to you the idea behind a content delivery network or CDN. I think that the Yahoo documentation on this rule is a bit confusing. So here's my attempt to explain YSlow rule #2: use a content delivery network.

To explain the rule, we need to start by looking at the problem. As discussed in the two previous blog posts, the browser typically needs to do a lot of requests to render a page. The YSlow documentation mentions that around 80-90 percent of the time of showing a page is used up by requesting resources from the server. That's why we need to optimize this part before looking into other stuff like your SQL or other serverlike stuff. We can do this in two ways: (1) by limiting the number of requests, as explained in the previous two posts, and (2) by speeding up the existing requests. The use of a CDN focuses on speeding up your existing requests or even avoids making them.

So what's a CDN? According to the YSlow documentation, a CDN is a number of web servers distributed across multiple locations. The idea behind this is to be able to serve different users as fast as possible, making the response times optimized for each particular client's location. Even though this may be the official definition of a CDN, I typically don't use a CDN for location-based stuff. As opposed to Yahoo, most of us develop smaller websites, targeting single countries or even regions. These sites typically don't have the need for a CDN, because the web server(s) can be placed near the users.

Why do we need to focus on this rule then? Well, a few years back we did not need to. In the meantime Google, Microsoft, and other large companies have opened up their internal CDNs for us to use. This is best illustrated by an example. You most likely use some sort of JavaScript framework like jQuery or Google Maps. On the needed pages you add a script import like this:

[html]

&lt;script src=&quot;/scripts/jquery.js&quot;&gt;&lt;/script&gt;

[/html]

As mentioned in the two previous posts, we could combine this script with other required scripts on our server. I usually use this approach a lot, but never when referencing scripts like jQuery. As I mentioned, Google (as well as others) made a CDN for us to use for common scripts like jQuery. Google's solution is called <a href="http://code.google.com/intl/en/apis/libraries/" target="_blank">Google Libraries API</a> and serves a lot of frequently used scripts. Instead of referencing jQuery like in the example above, you should reference it like this:

[html]
&lt;script src=&quot;https://ajax.googleapis.com/ajax/libs/jquery/1.5.1/jquery.min.js&quot;&gt;&lt;/script&gt;
[/html]

Why is this a good idea? There are  several reasons:

1. The browser is more likely to have the response of this URL cached, because a lot of sites now use this approach.

2. Response times on Google's servers pretty much kick everybody else's asses.

3. You don't need to worry about minimizing JavaScript yourself.

4. You don't need to use disk space for common scripts.

5. Updating to a new version is as simple as changing a URL. No new download from the jQuery website required.

Start by using everybody else's content delivery networks and build your own only if the gig requires you to.