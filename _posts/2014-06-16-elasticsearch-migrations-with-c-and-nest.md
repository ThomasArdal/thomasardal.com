---
ID: 1361
post_title: 'Elasticsearch migrations with C# and NEST'
author: Thomas Ardal
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/elasticsearch-migrations-with-c-and-nest/
published: true
post_date: 2014-06-16 07:27:32
---
It should be so easy: NoSQL databases and their schemaless approach to the world. Say goodbye to tables, primary keys, foreign keys, and most importantly - Migration! Unfortunately the reality is not quite as black and white. I've been working with NoSQL databases such as <a href="http://www.elasticsearch.org/" target="_blank">Elasticsearch</a>, <a href="http://ravendb.net/" target="_blank">RavenDB</a>, <a href="http://www.mongodb.org/" target="_blank">MongoDB</a> and <a href="http://couchdb.apache.org/" target="_blank">CouchDB</a> more or less constantly over the last five years. The past year more intense during the development of my cloud logging project <a href="https://elmah.io/">elmah.io</a>. I've not yet come across a NoSQL database which does not need some sort of data migration.

In this blog post I will dig down into data migrations in Elasticsearch, which is the great search engine we use on <a href="https://elmah.io/">elmah.io</a>. The examples are written in C# with the official Elasticsearch client <a href="http://nest.azurewebsites.net/" target="_blank">NEST</a>, but the procedure will be the same with other programming languages.

Imagine a relational database where you need to change the zipcode column from an int to a string. You typically write a migration script and execute it against your database either manually or through a migration tool like DbUp. The script handles the data type change, but you are also forced to handle the data change of any existing rows in the table. You probably know the process.

In Elasticsearch, you can choose between two different solutions when you need to make schema changes:

<ol>
<li>Leave it to Elasticsearch to evolve the schema.</li>
<li>Do a PutMapping call on Elasticsearch with an updated schema.</li>
</ol>

In practice I always choose options 2, because it gives me full control over how and when changes are happening. Another thing is, that Elasticsearch chooses data types and analyzers for new fields itself, which is not always what you wanted.

The Elasticsearch blog has an excellent <a href="http://www.elasticsearch.org/blog/changing-mapping-with-zero-downtime/" target="_blank">solution</a> for versioning indexes, which I will use as a foundation in the following C# examples. Briefly, this consists of appending a version number to your index name, and then create new versions of the index when changes should be made that require indexing. In order not to have to release a new version of your software, designating a new active index, you create an alias (typically index name without the version number). In this case the alias ​​will always identify the active index. For a detailed description of the approach I recommend you to read the content on the link above.

Let's start by creating a new index with a schema:
 
<pre class="lang:c# decode:true " >var connectionSettings = new ConnectionSettings(new Uri("http://localhost:9200/"));
connectionSettings.SetDefaultIndex("customers");
var elasticClient = new ElasticClient(connectionSettings);
 
elasticClient.CreateIndex("customers-v1");
elasticClient.Alias(x =&gt; x.Add(a =&gt; a.Alias("customers").Index("customers-v1")));
elasticClient.Map&lt;Customer&gt;(d =&gt;
	d.Properties(p =&gt; p.Number(n =&gt; n.Name(name =&gt; name.Zipcode).Index(NonStringIndexOption.not_analyzed))));</pre> 

In the example I create a new index called customers-v1 and add a mapping (schema) for the C# type Customer with a single field Zipcode. The result is shown in the following screenshot:

<img src="http://thomasardal.com/wp-content/uploads/2014/06/screenshot1.png" alt="screenshot1" width="1114" height="683" class="aligncenter size-full wp-image-1363" />

Then I insert a Customer document:
 
<pre class="lang:c# decode:true " >elasticClient.Index(new Customer { Zipcode = 8000 });</pre> 

Now we imagine that I need to change the type of Zipcode from Number to String and simultaneously migrate existing documents. I can not directly change the type of an existing field in Elasticsearch why I write a migration using the Reindex method in NEST:
 
<pre class="lang:c# decode:true " >var reindex =
	elasticClient.Reindex&lt;Customer&gt;(r =&gt;
    	r.FromIndex("customers-v1")
        	.ToIndex("customers-v2")
        	.Query(q =&gt; q.MatchAll())
        	.Scroll("10s")
        	.CreateIndex(i =&gt;
            	i.AddMapping&lt;Customer&gt;(m =&gt;
                	m.Properties(p =&gt;
                    	p.String(n =&gt; n.Name(name =&gt; name.Zipcode).Index(FieldIndexOption.not_analyzed))))));
var o = new ReindexObserver&lt;Customer&gt;(onError: e =&gt; { });
reindex.Subscribe(o);</pre> 

The Reindex method needs from and to index names, as well a what documents to reindex. In this case I tell it to match all documents. As part of the Reindex call, the v2 index is automatically created and a PutMapping call is made. This corresponds to the CreateIndex and Map methods called in the previous example.

Last but not least, we change the alias to point to the new index:
 
<pre class="lang:c# decode:true " >elasticClient.DeleteIndex(d =&gt; d.Index("customers-v1"));
elasticClient.Alias(x =&gt; x.Add(a =&gt; a.Alias("customers").Index("customers-v2")));</pre> 

The first line deletes the old index, which also deletes the alias. Then I re-create the alias and set it to point to the customers-v2 index.

When put together it becomes clear, that data migration is far from a closed chapter in NoSQL databases like Elasticsearch. This is not a bad thing and as it turns out, migrations is a lot easier since many changes can be made without executing scripts against the database. Furthermore, there is great build-in support for re-indexing, making it easy for those situations where you need to rebuild an index.

The complete code sample can be found at <a href="https://gist.github.com/ThomasArdal/1f69720da208f1863ae7" target="_blank">https://gist.github.com/ThomasArdal/1f69720da208f1863ae7</a>.