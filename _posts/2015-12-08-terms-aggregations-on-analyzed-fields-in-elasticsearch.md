---
ID: 1715
post_title: >
  Terms aggregations on analyzed fields in
  Elasticsearch
author: Thomas Ardal
post_date: 2015-12-08 12:31:43
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/terms-aggregations-on-analyzed-fields-in-elasticsearch/
published: true
---
I recently had the chance to cleanup an Elasticsearch mapping that I've been dying to refactor for some time now. The problem were a number of <code>not_analyzed</code> fields which really should have been <code>analyzed</code>, making them available for full-text search. When doing so, I encountered a problem with a couple of terms aggregations which at first seemed odd, but turned out to make a lot of sense.

To illustrate the problem, let's look at en Elasticsearch mapping containing a <code>not_analyzed</code> field:

<pre class="lang:json">
{
  "mappings": {
    "mydocument": {
      "properties": {
        "title": {
          "type": "string",
          "index": "not_analyzed"
        }
      }
    }
  }
}
</pre>

To test this scenario, let's put some documents into the index:

<pre class="lang:json">
[
  {"title": "This is the first title"},
  {"title": "This is the second title"},
]
</pre>

And finally execute the terms aggregation:

<pre class="lang:json">
{
  "aggs": {
    "titles": {
      "terms": {
        "field": "title"
      }
    }
  }
}
</pre>

The result shouldn't be suprising. We ask Elasticsearch to split up the documents into buckets of unique titles:

<pre class="lang:json">
{
  "buckets": [
    {"key": "This is the first title", "doc_count": 1},
    {"key": "This is the second title", "doc_count": 1},
  ]
}
</pre>

So far so good, but now for the problem. I changed the title property to an analyzed field and re-indexed all of my documents. The mapping now looks similar to this:

<pre class="lang:json">
{
  "mappings": {
    "mydocument": {
      "properties": {
        "title": {
          "type": "string",
          "index": "analyzed"
        }
      }
    }
  }
}
</pre>

Notice how the value of the <code>index</code> property changed from <code>not_analyzed</code> to <code>analyzed</code>. Let's run our aggregations once more:

<pre class="lang:json">
{
  "aggs": {
    "titles": {
      "terms": {
        "field": "title"
      }
    }
  }
}
</pre>

And for the results:

<pre class="lang:json">
{
  "buckets": [
    {"key": "is", "doc_count": 2},
    {"key": "the", "doc_count": 2},
    {"key": "this", "doc_count": 2},
    {"key": "title", "doc_count": 2},
    {"key": "first", "doc_count": 1},
    {"key": "second", "doc_count": 1}
]
</pre>

At first sight I was like:

<img src="http://thomasardal.com/wp-content/uploads/2015/12/wtf.gif" alt="wtf" width="500" height="268" class="aligncenter size-full wp-image-1722" />

But then I realized that the results make perfectly sense. This isn't a blog post about how reverse indexes work in Elasticsearch, but in short <code>analyzed</code> instructs Elasticsearch to look at the value of the title property and tokenize it. The terms aggregation runs on top of the reverse index, why Elasticsearch simply reply with an answer for our (sort of stupid) question: Split the values in the reverse index into buckets containing unique terms.

To fix this, we need to store both an <code>analyzed</code> and an <code>not_analyzed</code> version if the title. You could store two individual properties for this, but Elasticsearch already provides a nice way to construct this using a multifield. With a multifield, our mapping looks like this:

<pre class="lang:json">
{
  "mappings": {
    "mydocument": {
      "properties": {
        "title": {
          "type": "string",
          "index": "analyzed",
          "fields": {
            "raw": {
              "type": "string",
              "index": "not_analyzed"
            }
          }
        }
      }
    }
  }
}
</pre>

Here we define the <code>title</code> field with both an <code>analyzed</code> version and an <code>not_analyzed</code> version named <code>raw</code>.

With the mapping above and the documents re-indexed, I run my terms aggregation on the <code>title.raw</code> field:

<pre class="lang:json">
{
  "aggs": {
    "titles": {
      "terms": {
        "field": "title.raw"
      }
    }
  }
}
</pre>

And the results look good once again:

<pre class="lang:json">
{
  "buckets": [
    {"key": "This is the first title", "doc_count": 1},
    {"key": "This is the second title", "doc_count": 1},
  ]
}
</pre>

It's funny how basic stuff still manage to surprise you once in awhile, even though having worked with a technology for years.