---
ID: 136
post_title: 'YSlow rule #1 &#8211; Minimize HTTP Requests'
author: Thomas Ardal
post_date: 2011-02-23 08:00:27
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-1-minimize-http-requests/
published: true
---
In the following twenty-two posts, I will look into all the rules in YSlow. This is a pretty comprehensive plan, but I will try to keep the posts short and precise. All rules are pretty good documented in the YSlow documentation here: <a href="http://developer.yahoo.com/performance/rules.html">http://developer.yahoo.com/performance/rules.html</a>. I will try to give you my point of view and experiences with each rule rather than just repeat the Yahoo documentation.

Let's look at the first YSlow rule: "Minimize HTTP Requests." This rule covers the problem of the way a page is rendered in the browser. When you browse to a new URL, the HTML is fetched from the web server hosting the requested domain. HTML typically contains references to resources located at the same or different web servers like images, scripts, style sheets, etc. Some might think that when doing the initial request for the HTML from the web server, all referenced resources from that same domain are also returned to the browser. This is not the case. When the browser successfully obtains the HTML, it starts rendering the markup. Each time it reaches an element in the HTML containing an src attribute, a new request for the referenced resource is made to the web server. You probably can understand why this could be a potential problem: a lot of requests to the web server.

How do we minimize the number of requests from the browser? Well, there are multiple strategies, and they all depend on the specific resource causing the request.

<strong>Images</strong>

You probably need a lot of images in order to render your page. Pretty much all pages on the web consists of both text and images. Images often result in a lot of requests, because most pages use a lot of images for making your website look pretty. One of the only strategies to reduce the number of requests for images is to combine multiple images into one; this is also known as CSS sprites. All your images are placed side by side in a larger image. This sounds a bit like a hack. Well, it <em>is</em> a hack!

When combining multiple images into one, you need some way of extracting each image when fetched from the server. Here you will be able to extract regions of a larger image, using CSS. I usually don't use CSS sprites that much. There are a couple of reasons why:
<ol>
	<li>If you are a programmer like me, it will get harder to deploy new images, because you need to modify an existing image rather than just add a new file.</li>
	<li>The client code gets messy compared to multiple clean image references. You need to specify the region inside the large image to show each time a new image should be rendered.</li>
</ol>
I prefer a good caching strategy over CSS sprites at all times. I sometimes in-line images in CSS as well. More on both strategies in later blog posts.

<strong>JavaScript/CSS</strong>

Unlike images, both JavaScript and CSS are all text. The easiest way to reduce the number of requests for these types of files is to combine all references of JavaScripts into one file and doing the same with all the style sheets. This strategy is similar to the CSS sprite approach for images, but we don't need to split the combined files on the client. That is why this is the simplest and most effective way to reduce HTTP requests for scripts and style sheets.

You can combine your scripts and style sheets manually or you can use some kind of tool for the job. When doing ASP.NET (MVC) sites, I usually go for <a href="https://github.com/jetheredge/SquishIt" target="_blank">SquishIt</a>, which bundles both JavaScript and CSS files into one. In my next blog post, I will show you how SquishIt works.

I never bundle my own style sheets and scripts with external stuff like jQuery. If you use a CDN from Google, Microsoft, or third, jQuery should already be in your local cache.