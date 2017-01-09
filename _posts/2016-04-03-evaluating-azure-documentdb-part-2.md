---
ID: 1754
post_title: Evaluating Azure DocumentDB – Part 2
author: Thomas Ardal
post_date: 2016-04-03 19:10:18
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/evaluating-azure-documentdb-part-2/
published: true
---
This is the second and final post in the series Evaluating Azure DocumentDB. Where <a href="http://thomasardal.com/evaluating-azure-documentdb-part-1/">Part 1</a> focused around the concepts in DocumentDB, I'll explore some actual code in this post.

<h3>Inserting documents</h3>

Before we dig into the code, there's a couple of terms in DocumentDB that you should know. Much like other NoSQL offerings, you have the concept of databases and collections. A database maps pretty much 1:1 with a database in SQL Server. I have seen collections mapped to SQL Server tables and though collections can be used this way, it's not recommended to do so. A collection in DocumentDB is more of a logical unit for storing one or more types of documents.

With the basics in place, let's insert a new document in DocumentDB:

<pre class="lang:csharp">
var documentClient = new DocumentClient(Url, Key);

await documentClient.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("test", "documents"),
    new UserDocument
    {
        Id = "User1",
        Username = "ThomasArdal",
        Firstname = "Thomas",
        Lastname = "Ardal",
    });
</pre>

To start using DocumentDB, I create a new DocumentClient, which acts as the main entry point for communicating with the server. <code>Url</code> and <code>Key</code> is copied from the Azure Portal.

In the code sample, I create a new <code>UserDocument</code>, which is a simple POCO defined elsewhere in the project. The only weird looking part here (at least to my eyes), is the <code>UriFactory.CreateDocumentCollectionUri</code> call. This is the way to tell DocumentDB which collection to put the new document into. I don't really like to specify this on every request, since it's simply an Uri created from magic strings. It's possible to query collections, but unfortunately collections don't allow for creating or querying documents. I would have preferred to be able to inject collection objects through IoC.

There's a nice management tool for DocumentDB, located on the Azure Portal. Going there shows us the new document which is indeed just JSON:

<img src="http://thomasardal.com/wp-content/uploads/2016/04/User1.png" alt="User1" width="535" height="356" class="aligncenter size-full wp-image-1788" />

<h3>Querying</h3>
DocumentDB supports SQL like syntax for querying documents. In fact Microsoft always seems to mention all of the "benefits" of using SQL rather than an NoSQL query language known from databases like CouchDB and Elasticsearch. I don't really agree on the fact that we need SQL for querying a NoSQL datastore, but luckily, the DocumentDB .NET client implements a LINQ provider. The provider still seems limited in the amount of methods available, but I assume that this will improve over time.

Let's insert a new document to have some different document types to query:

<pre class="lang:csharp">
await documentClient.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("test", "documents"),
    new OrderDocument
    {
        Id = "Order1",
        Amount = 42,
        Created = DateTime.UtcNow,
        CreatedBy = "User1",
    });
</pre>

To query all orders created by User1:

<pre class="lang:csharp">
var orderDocuments = documentClient.CreateDocumentQuery<OrderDocument>(
    UriFactory.CreateDocumentCollectionUri("test", "documents"))
    .Where(order => order.CreatedBy == "User1");

foreach (var order in orderDocuments)
{
    Console.WriteLine("Order on {0} by {1}", order.Amount, order.CreatedBy);
}
</pre>

The output shows the expected Order1:

<pre>
Order on 42 by User1
</pre>

Remember in the first post, where I said that this is truly just a JSON store with some search capabilities on top? To prove the point, we'll insert a new <code>UserDocument</code>:

<pre class="lang:csharp">
documentClient.CreateDocumentAsync(
    UriFactory.CreateDocumentCollectionUri("test", "documents"),
    new UserDocument
    {
        Id = "User2",
        Username = "ThomasArdal",
        Firstname = "Thomas",
        Lastname = "Ardal",
        CreatedBy = "User1",
    }).Wait();
</pre>

For the example, I've extended <code>UserDocument</code> with a new property named <code>CreatedBy</code>.

If we try the query again, we now get:

<pre>
Order on 42 by User1
Order on 0 by User
</pre>

If you're thinking WTF I don't blame you. We searched for <code>OrderDocument</code>s with a <code>CreatedBy</code> value of <code>User1</code>. Unfortunately the generic type on <code>CreateDocumentQuery</code> only acts as info to tell the DocumentDB client library how to deserialize the response. This means that the query finds both the <code>OrderDocument</code> and <code>UserDocument</code> with the value <code>User1</code> in the <code>CreatedBy</code> field. The .NET client then tries to map the returned <code>UserDocument</code> to a <code>OrderDocument</code>.

To fix this, you could add a <code>Type</code> property to all of your documents and include this in all queries. It's a bit shame that this process isn't automated somehow. I can understand why it's build the way it is, being a 100 % schemaless datastore. But with generics in C#, this could have been improved by having meta data fields or similar on all documents.

That's it for now. I may want to write additional posts about more complex query scenarios and/or data migration.