---
ID: 1807
post_title: >
  Analyzing user agents to identify bots
  with Elasticsearch
author: Thomas Ardal
post_date: 2016-05-12 17:39:03
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/analyzing-user-agents-to-identify-bots-with-elasticsearch/
published: true
---
Most of you probably know my startup <a href="https://elmah.io">elmah.io</a>. If not, check it out and head back here when you've signed up :) At elmah.io we index errors generated at our customers websites. Among a lot of other things, we index the user agent causing the error. If you've ever logged uncaught errors (like 404's) on a webserver, you know that bots, crawlers, spiders etc. are causing a lot of them.

To minimize the number of logged errors for ourselves and our customers, I want to be able to identify bots by looking at the user agent causing an error. I know that there are lists of both whitehat and blackhat bots out there, but for this purpose, I want a single, short and fast query, identifying as many whitehat bots as possible.

To have some data to play around with, I've fetched a list of browser user agents and another list of bot user agents (source: <a href="http://www.useragentstring.com/" target="_blank">http://useragentstring.com/</a>). Both lists are converted to Elasticsearch bulk index format. Since the purpose of this post is playing around with aggregations in Elasticsearch, I won't go into detail on how to convert HTML to Elasticsearch bulk format.

To create a new Elasticsearch index to store all of the user agents, execute the following request:

<pre class="lang:bash">
PUT http://localhost:9200/useragents
</pre>

To index the data and let Elasticsearch create the mapping for the <code>browsers</code> and <code>bots</code> types, POST all of the user agents using the <code>_bulk</code> endpoint:

<pre class="lang:bash">
POST http://localhost:9200/_bulk
</pre>

<pre class="lang:javascript">
{ "index": { "_index": "useragents", "_type": "browsers" } }
{ "userAgent": "Mozilla/5.0 (Windows NT 6.1) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/41.0.2228.0 Safari/537.36" }
{ "index": { "_index": "useragents", "_type": "bots" } }
{ "userAgent": "Mozilla/5.0 (compatible; Googlebot/2.1; +http://www.google.com/bot.html)" }
...
</pre>

All user agents are successfully indexed in Elasticsearch. Since the <code>userAgent</code> field in both <code>browsers</code> and <code>bots</code> is analyzed, Elasticsearch breaks down all of the user agent strings into terms. This actually helps us to find common keywords in bot user agents.

To find the most commonly used terms in bot user agents, create a new terms aggregation on all of the bot user agents:

<pre class="lang:bash">
POST http://localhost:9200/useragents/bots/_search
</pre>

<pre class="lang:javascript">
{
    "aggs": {
        "bot_terms": {
            "terms": {
                "field": "userAgent",
                "size": 100
            }
        }
    }
}
</pre>

Elasticsearch returns a list of buckets, ordered by the most frequent terms:

<pre class="lang:javascript">
"buckets": [
{
    "key": "Mozilla",
    "doc_count": 97
},
{
    "key": "5.0",
    "doc_count": 85
},
{
    "key": "compatible",
    "doc_count": 58
}
...
]
</pre>

This is great input for our identifying bots-quest. Using terms like <code>Mozilla</code> and <code>compatible</code> probably isn't a good idea, since browser user agents includes these as well. By scrolling down through the list, we find <code>bot</code>, <code>spider</code>, <code>crawler</code> and other interesting terms. Let's create a query against all bot user agents with these terms:

<pre class="lang:bash">
POST http://localhost:9200/useragents/bots/_search
</pre>

<pre class="lang:javascript">
{
    "query": {
        "query_string": {
            "query": "userAgent: *bot* OR userAgent: *crawl* OR userAgent: *spider* OR userAgent: *search*"
        }
    }
}
</pre>

As we can see from the result, Elasticsearch finds a lot of hits:

<pre class="lang:javascript">
{
  ...
  "hits": {
    "total": 298
    ...
  }
}
</pre>

Time to test the query against browser user agents to make sure that browsers aren't threated as bots:

<pre class="lang:bash">
POST http://localhost:9200/useragents/browsers/_search
</pre>

<pre class="lang:javascript">
{
    "query": {
        "query_string": {
             "query": "userAgent: *bot* OR userAgent: *crawl* OR userAgent: *spider* OR userAgent: *search*"
        }
    }
}
</pre>

The query returns zero hits.

Using Elasticsearch for analyzing user agents and creating a smart query based on terms, turned out as a great way to identify most bots without having to maintain long lists of user agents, making API calls or similar. Querying Elasticsearch takes a few milliseconds and the result is almost as good as writing a lot of code.