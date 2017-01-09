---
ID: 277
post_title: 'YSlow rule #10 – Reduce DNS Lookups'
author: Thomas Ardal
post_date: 2011-06-02 18:31:56
post_excerpt: ""
layout: post
permalink: 'http://thomasardal.com/yslow-rule-9-%e2%80%93-reduce-dns-lookups/'
published: true
---
Most of us (me included) don’t think much about the way DNS works. Browsers, on the other hand, use DNS pretty much every day. For the uninformed, I will start by explaining how DNS works. If you already know this, feel free to skip the following paragraph.

As you know, all computers with a network card installed are awarded an IP address, which is four (or more) numbers separated by dots. When you do a request on a webserver, you are actually making an HTTP request to an IP address. Inputting IP addresses in the browser would make browsing the web just plain horrible. The DNS was invented to avoid having to remember IP addresses. When requesting a domain name, like <a href="http://www.hippovalidator.com" target="_blank">www.hippovalidator.com</a>, your browser asks the DNS to resolve the inputted domain name to an IP address. The request is then sent to the IP address returned by the DNS server, making it easy and painless for you to request a webpage.

Now then, why do we as performance-optimizing wizards need to worry about DNS? Well, in my opinion, we don’t. According to the YSlow documentation, a single DNS request typically takes 20-120 milliseconds. This may not seem much, but imagine the following HTML header example code:

[html]
 &lt;script type=&quot;text/javascript&quot; src=&quot;http://ajax.googleapis.com/ajax/libs/jquery/1.4.2/jquery.min.js&quot;&gt;&lt;/script&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;http://s7.addthis.com/js/250/addthis_widget.js&quot;&gt;&lt;/script&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;http://static.myrating.dk/rating.js&quot;&gt;&lt;/script&gt;
 &lt;script type=&quot;text/javascript&quot; src=&quot;http://connect.facebook.net/en_US/all.js&quot;&gt;&lt;/script&gt;

&lt;script src=&quot;http://cdn.uservoice.com/javascripts/widgets/tab.js&quot;&gt;&lt;/script&gt;

[/html]

The browser needs to make 5 requests to the DNS server in order to fetch the referenced scripts. Worst case scenario, this will (according to YSlow) take as much as 600 milliseconds. This is a substantial amount of time! How do you avoid this?

Option 1: Reference less stuff from other domains.

Option 2: Copy the referenced stuff from the other domains to your own domain (in the example above, the scripts could all reside on the local domain).

So let’s head back to my point about DNS not being important. In fact, I don’t think you should implement any of the solutions mentioned above. Why?

1. Because of DNS caching. When your browser asks a DNS server for an IP address, the result is cached in the browser’s local DNS cache. Chances are, your ISP already has the result cached as well.

2. Because of prefetching in all modern browsers. Every single browser that I can think of does some sort of prefetching. In Chrome, a background thread parses all domain names in a requested HTML document and resolves the domain names while you look at the page. IE does something similar.

3. Because reducing DNS lookups also reduces the number of simultaneous downloads in the browser. As I mentioned in a previous post, the browser is not able to make unlimited simultaneous requests to the same domain. That’s why my recommendation is to use jQuery (and similar from Google’s or Microsoft’s CDN) and place all of your static files like images, scripts, and style sheets on a subdomain. This would result in more DNS lookups, but it would optimize the browser’s possibilities to fetch the needed resources.

The conclusion is this... Don’t be alarmed when YSlow warns you about reducing DNS lookups!