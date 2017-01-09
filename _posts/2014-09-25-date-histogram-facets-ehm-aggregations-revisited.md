---
ID: 1422
post_title: 'Date histogram facets &#8230; ehm aggregations revisited'
author: Thomas Ardal
post_date: 2014-09-25 06:17:47
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/date-histogram-facets-ehm-aggregations-revisited/
published: true
---
Goight through last years blog posts written during <a href="http://gotocon.com/" target="_blank">GOTO</a>, I found a post terrible out-dated post named <a href="/implementing-date-histogram-facets-with-elasticsearch-and-nest/">Implementing date histogram facets with Elasticsearch and Nest</a>. A lot happened since then. Elasticsearch turned 1.0 and follwing that, aggregrations were introduced. Aggregations is facets on steroids and therefore deprecates facets.

Let's revisit the example from last year, to generate a date histogram using aggrations. In this sample I want to group the errors by hour, count the number of errors per hour and show a nice date histogram with the result. The query using aggregrations look like this:
 
<pre class="lang:c# decode:true " >var result = elasticClient.Search&lt;ErrorDocument&gt;(search =&gt; search
  .SearchType(SearchType.Count)
  .Query(q =&gt; q
    .Range(range =&gt; range
      .OnField(field =&gt; field.Time)
      .GreaterOrEquals(DateTime.UtcNow.AddHours(-24))
      .LowerOrEquals(DateTime.UtcNow)
    )
  )
  .Aggregations(a =&gt; a
    .DateHistogram("histogram", s =&gt; s
      .Field(field =&gt; field.Time)
      .Interval("hour")
      .MinimumDocumentCount(0)
    )
  )
);
 
var dateHistogram = result.Aggs.DateHistogram("histogram");</pre> 

So what diverge from the original example? In line 2 I specify the search type as count. This is not new and should have been in the old example as well. Setting the search type to count indicates to Elasticsearch, that I don't need to access the actual documents of the query.

The query is straight forward. The exiting part happens in line 10. Here I add a new aggregation to the search object, telling Elasticsearch to generate a date histogram of the documents returned by the query. The field specification is similar to the faceted example and tells which field I want to group by. The interval is also similar to facets, telling the aggregation which granularity should be used for the groups.

The best improvement is probably in line 14, telling Elasticsearch to set the minimum document count to zero. In plain English, this tell the aggregation to also include groups including zero results. With faceted date histogram, having a group (an hour in this example) with no document hits, the facet result would simply not include that group. This would force you to iterate over the result of a faceted date histogram and fill in the blanks.

To sum up, the aggregated example have the following advantages to the faceted example:

<ul>
<li>Operates on the queries and filters of the search, which avoids having to specify queries and filters on each aggregation.</li>
<li>Minimum document count, makes it easy to generate a histogram where some of the buckets can include no hits.</li>
</ul>