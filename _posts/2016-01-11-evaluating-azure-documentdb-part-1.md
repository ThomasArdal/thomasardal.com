---
ID: 1736
post_title: 'Evaluating Azure DocumentDB &#8211; Part 1'
author: Thomas Ardal
post_date: 2016-01-11 06:04:11
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/evaluating-azure-documentdb-part-1/
published: true
---
I'm currently working on way more projects and products than I probably should. One of them is a new mobile app which will conquer the universe! Well maybe just the world. For the backend I wanted to use something different than SQL Server, since data from the app doesn't really fit into a relational database. I've used Elasticsearch, CouchDB, MongoDB and RavenDB in the past, but they all suffer from the fact that you either need to know a lot about hosting them or want to pay a lot of cash for someone else to do it. Since I'm already utilizing a lot of features on Windows Azure, I wanted to build a prototype around DocumentDB, which is a new NoSQL datastore available as a service on Azure. After a couple of weeks working with DocumentDB, I want to share my experiences in this post.

<h3>Documentation</h3>
It may sound weird that I start talking about the documentation, since we as developers always want to dig into the code first. But diving into a new database, typically require a lot of browsing trough documentation and blog posts. The documentation site for DocumentDB is quite good. It shows that Microsoft really want to compete with the large amount of NoSQL options available and therefore put a lot of effort into documenting their offer. During my prototype, I found some good articles explaining how to use DocumentDB and how to use it right.

<ul>
  <li><a href="https://azure.microsoft.com/en-us/documentation/articles/documentdb-get-started/" target="_blank">https://azure.microsoft.com/en-us/documentation/articles/documentdb-get-started/</a></li>
  <li><a href="https://azure.microsoft.com/en-us/documentation/articles/documentdb-resources/" target="_blank">https://azure.microsoft.com/en-us/documentation/articles/documentdb-resources/</a></li>
  <li><a href="https://azure.microsoft.com/en-us/blog/performance-tips-for-azure-documentdb-part-1-2/" target="_blank">https://azure.microsoft.com/en-us/blog/performance-tips-for-azure-documentdb-part-1-2/</a></li>
  <li><a href="https://azure.microsoft.com/en-us/documentation/articles/documentdb-geospatial/" target="_blank">https://azure.microsoft.com/en-us/documentation/articles/documentdb-geospatial/</a></li>
  <li><a href="https://azure.microsoft.com/en-us/documentation/articles/documentdb-programming/#trigger" target="_blank">https://azure.microsoft.com/en-us/documentation/articles/documentdb-programming/#trigger</a></li>
</ul>

<h3>Schemaless</h3>
It's a common misunderstanding that NoSQL databases are always schemaless. In fact most NoSQL databases have some kind of schema or tries to understand the structure of the stored documents somehow. In DocumentDB there is NO schema. Documents are stored as JSON and there don't seem to be any features inside DocumentDB trying to figure out if multiple documents share a similar structure. This provides some interesting options where you are able to do funky stuff like persisting documents with an integer property and later reading it as a string. Serializing and deserializing is handled by Newtonsoft.Json, why every trick in the book using json.net goes in DocumentDB as well.

<h3>Administration</h3>
When setting up DocumentDB, you need to go to the Azure Portal which is the most recent version of Microsofts administration tool for Azure. I totally understand why DocumentDB isn't available in the old portal (manage.windowsazure.com), but since Portal still seems to have the occasional bug or two, it's a bit shame that you cannot switch to the old portal when something weird happens. I had multiple experiences with 500 errors in Portal when trying to create both databases and collections in DocumentDB. Luckily everything is available through code as well.

The administration UI for DocumentDB looks pretty impressive already, when thinking about the fact the DocumentDB is less than 2 years old. You are able to browse databases, collections, documents and even stored procedures and triggers.

<h3>Client Libraries</h3>
Microsoft offers client libraries for a small range of languages like .NET, JavaScript and Python. Everything runs on top of DocumentDB's REST API, which I haven't had a chance to play with. But being on the .NET platform entirely with this project, makes everything besides the .NET client superfluous. Working with the client hasn't been a nice experience. No matter how simple the task you need to execute with DocumentDB, too much code seems to be required. In fact Eric Zheng summed it very well in <a href="http://zhengziying.com/2015/04/23/the-self-link-nonsense-in-azure-documentdb/" target="_blank">this blog post</a>. I hope that v2 of the .NET client will abstract a lot of the complexity from the REST API, like NEST have done it for Elasticsearch.

In the next post, I will look at actually communicating with DocumentDB from code.