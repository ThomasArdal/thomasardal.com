---
ID: 347
post_title: 'YSlow rules #12 &#8211; Avoid redirects'
author: Thomas Ardal
post_date: 2011-07-23 20:01:32
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rules-12-avoid-redirects/
published: true
---
A former college of mine told me, that he had founded "The association opposed the abandonment of PerformanceDude.com". In order to make him happy, I thought that I would leave the wonderful world of unit testing for a moment, in the benefit of writing a performance related post :) We've reached YSlow rule number 12 - Avoid redirects. I think the Yahoo documentation about the subject is a bit vague, so here's my opinion on the subject.

Redirects are used to tell the browser, that a requested URL have been moved either permanent (http status code 301) or temporarily (http status code 302). When doing a request which returns a 301/302, the browser automatically makes a new http request, towards the URL returned by the 301/302. Being able to redirect URLs is really important if you want to change your URLs and your site is already indexed by search engines. If changing your URL scheme without returning a redirect, you basically lose your ranking in search engines like Google and all those hours spend getting <a href="http://thekeywordacademy.com/link-juice-explained" target="_blank">link juice</a> is wasted. In ASP.NET MVC 3 doing redirect is easy:

[csharp]
public ActionResult OldUrl()
{
    return RedirectPermanent(&quot;/the-new-url&quot;);
}
[/csharp]

Notice that the above example returns a 301 (permanent redirect). If you for some reason need to return a 302, use the Redirect-method instead.

So what is my opinion on this YSlow rule? Well unless you use some sort of obscure web-framework which does a lot of magic behind the scene, there's only one pitfall which should make you want to do something about this rule. When doing permanent redirects, ALWAYS make sure that all your internal links points to the new URL, rather than towards the old URL, letting the browser do the redirect. Implementing this anti-pattern will absolutely slow down your site!