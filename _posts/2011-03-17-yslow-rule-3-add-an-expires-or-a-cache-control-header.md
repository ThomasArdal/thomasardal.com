---
ID: 180
post_title: 'YSlow rule #4 &#8211; Add an Expires or a Cache-Control Header'
author: Thomas Ardal
post_date: 2011-03-17 06:29:19
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/yslow-rule-3-add-an-expires-or-a-cache-control-header/
published: true
---
We have reached rule number 4 in our journey into the exciting world of the YSlow rule set. Rule number 4 is all about caching. We have tackled the caching area a bit in some of the previous posts, but in this post I will dig more into this important area.

Showing webpages is all about getting resources like HTML, JavaScript, and images from the server and combining them in a showable way in the browser. I've previously talked about how to minimize the number of requests to the server, but what do we do when we have removed all the unnecessary requests with SquishIt and similar tools? One of the answers is caching, which is applicable for pretty much all pieces of a webpage. I will not explain any basics about HTTP caching. If you are not familiar with the subject, you should check out <a href="http://www.mnot.net/cache_docs/">Mark Nottingham's great Caching Tutorial</a>.

So what's the buzz about the Expires and Cache-Control headers? Both terms are part of the response header, which is used to tell clients if the response can be cached. Rather than explaining you all the theories, I would rather show you how I've implemented caching in one of my Danish ASP.NET MVC-based websites named <a href="http://www.myrating.dk" target="_blank">myrating</a>.

<strong>The problem</strong>

On my "product" pages I show similar items in a column to the right of the product info. Similar content is pretty performance heavy to calculate because of the algorithm, which is based on tags, ratings, and even more stuff. As a result, loading the product page was very slow, caused by the small similar items box.

<strong>The solution</strong>

I decided to do two things. The first solution was to wait for the rest of the page to show and load the info box async. I will show you how to do this in an upcoming post, but right now we will focus on the second solution: caching the response. I could have implemented the caching in an HTTP module but decided to implement a new action filter for ASP.NET MVC. This way I can annotate all the methods in my ASP.NET MVC project, the result of which can be cached. The action filter looks like this:

[csharp]

public class CacheableAttribute : ActionFilterAttribute
{
    public override void OnActionExecuted(ActionExecutedContext filterContext)
    {
        filterContext.HttpContext.Response.Cache.SetExpires(DateTime.Now.AddDays(7));
        filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.Public);
        filterContext.HttpContext.Response.Cache.SetValidUntilExpires(true);
    }
}
[/csharp]

In line 5 I set the Expires header to a week from now. I've hardcoded this time span but could as well have implemented a property for making this configurable.

In line 6 I set the cacheability to public. This basically means that all parties can cache this, including both browsers and proxies.

In line 7 I set valid until expires to true to avoid cache invalidation sent from the browser. This way I control how long the response is cached, and not the browser.

Making responses cacheable for one week is now piece of cake. All I need to do is to add the Cacheable attribute to the action methods which can be cached like this:

[csharp]

[Cacheable]
public ActionResult FindSimilarItems(Guid id)
{
    ...
}
[/csharp]