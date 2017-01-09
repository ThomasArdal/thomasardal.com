---
ID: 1187
post_title: >
  Implementing date histogram facets with
  ElasticSearch and NEST
author: Thomas Ardal
post_date: 2013-10-02 10:18:37
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/implementing-date-histogram-facets-with-elasticsearch-and-nest/
published: true
---
My pet project <a href="https://elmah.io/" target="_blank">elmah.io</a> stores every error in Elasticsearch and the search GUI contains a perfect example for doing faceted search: the errors graph in top of the search result.

<a href="http://thomasardal.com/wp-content/uploads/2013/10/errors_per_day.png"><img src="http://thomasardal.com/wp-content/uploads/2013/10/errors_per_day.png" alt="errors_per_day" width="729" height="240" class="alignleft size-full wp-image-1190" /></a>

At the moment the code for this sucker isn’t the stuff I’m most proud of. I simply extract every error created during the last 14 days and doing some LINQ to Objects magic on top of that. Not really optimal. What I really want to do, is letting Elasticsearch create a date histogram containing a dictionary of values where the date is key and the amount of errors created on that date as value. The query turned out to be really simple, once I got a hang on faceted search using <a href="https://github.com/Mpdreamz/NEST" target="_blank">NEST</a> (the excellent .NET client for Elasticsearch):

<script src="https://gist.github.com/ThomasArdal/6791576.js"></script>

Thanks to Martijn Laarman for the help on StackOverflow.