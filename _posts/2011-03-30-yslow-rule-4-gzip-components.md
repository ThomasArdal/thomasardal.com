---
ID: 204
post_title: 'YSlow rule #5 &#8211; Gzip Components'
author: Thomas Ardal
post_date: 2011-03-30 05:35:17
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-4-gzip-components/
published: true
---
We all know it: zipping is a great way to compress the size of a file. This rule also applies on the web. Zipping responses from a server is natively implemented in the HTTP version 1.1 standard, which is why you don't have any excuse left not to zip all your responses. I won't go into a lot of detail on how zipping over HTTP works, but here is a short brushup.

When doing a request on a web server, the client can set the Accept-Encoding request header. If set to one of the valid values like "gzip," the client tells the server that it is capable of handling a zipped response. If the server chooses to zip the response, it must set the Content-Encoding response header to a valid value like "gzip" or "deflate." The browser checks for this header and unzips the response if necessary. This sounds a bit technical, but actually all of the above are handled by the browser. This means that all you have to do is tell your server to gzip encode your content. Pretty neat, right?

There are different ways to gzip encode web server responses. I typically use ASP.NET MVC for my projects, which does not have anything build in. I've seen different implementations in HTTP handlers and modules as well as custom action filters. I typically use <a href="http://weboptimizer.codeplex.com" target="_blank">WebOptimizer.NET</a> by a fellow Danish developer, Mads Kristensen. It has a pretty simple HTTP module, which produces standard gzip/deflate by adding the WebOptimizer dll to your dependencies and pasting the following XML to your web.config:

[xml]
&lt;system.webServer&gt;
  &lt;modules&gt;
    ...
    &lt;add name=&quot;CompressionModule&quot; type=&quot;WebOptimizer.Modules.CompressionModule, WebOptimizer&quot;/&gt;
    ...
  &lt;/modules&gt;
&lt;/system.webServer&gt;
[/xml]

It doesn't get any simpler than this! The downside is that WebOptimizer doesn't really evolve (last commit in 2009). If you have any proposals for a better and more recent implementation, please provide me with a link.

If you have access to configure the IIS, compression is natively supported. Scott Hanselman just wrote a <a href="http://www.hanselman.com/blog/EnablingDynamicCompressionGzipDeflateForWCFDataFeedsODataAndOtherCustomServicesInIIS7.aspx" target="_blank">great blog post</a> on the subject.