---
ID: 1852
post_title: >
  Automating Elasticsearch backup using
  Windows Azure and PowerShell
author: Thomas Ardal
post_date: 2016-07-07 08:54:47
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/automating-elasticsearch-backup-using-windows-azure-and-powershell/
published: true
---
As some of you know, I'm hosting an Elasticseach cluster on Azure using Windows Server virtual machines. When working as an external consultant, I always recommend my customers to host Elasticsearch on Linux. So why not follow this rule myself? Well, if shit hits the fan, I need to be able to get my cluster up and running as quickly as possible and sadly, I don’t have enough Linux competences in order to do that. I know it’s basically a bad excuse not to learn Linux and that is definitely something that I want to change.

Well, the whole Linux vs. Windows discussion isn’t really the topic of this post. Implementing a backup strategy for your Elasticsearch cluster on the other hand is. Being on Azure and all, using the features available there makes a whole lot of sense. In this post, I’ll show you how to set up daily backups using Elasticsearch, Azure blob storage and PowerShell. In the examples I’ll use Elasticsearch 2.x, but options for 1.x and 5.x are available as well.

The easiest way to utilize Elasticsearch and Azure is to use the official Azure plugin for Elasticsearch called <a href="https://www.elastic.co/guide/en/elasticsearch/plugins/2.3/cloud-azure.html" target="_blank">cloud-azure</a>. To install this: execute the following from your Elasticsearch bin directory:

<pre class="lang:bash">
plugin install cloud-azure
</pre>

If running a cluster, install the plugin on all nodes. Before being able to create a backup, the node should be restarted, but we will wait a bit doing so.

The cloud-azure plugin adds a couple of features to your Elasticsearch cluster. The feature relevant for this post is being able to backing up your Elasticsearch data on Azure blob storage. To do so, create a new storage account:

<img src="http://thomasardal.com/wp-content/uploads/2016/07/create_storage_account.png" alt="create_storage_account" width="943" height="695" class="aligncenter size-full wp-image-1854" />

A few important things to notice here. For <em>Deployment model</em>, I’ve chosen Resource manager. I’ve used <em>Classic</em> in the past and that seemed to run at the time as well. In <em>Performance</em> it’s very important to choose <em>Standard</em> over <em>Premium</em>, since cloud-azure doesn’t seem to work with premium storage.

When created, copy the storage account name and key:

<img src="http://thomasardal.com/wp-content/uploads/2016/07/keys.png" alt="keys" width="1210" height="519" class="aligncenter size-full wp-image-1856" />

Now for Elasticsearch. To tell the cloud-azure plugin which storage account to use, update your elasticsearch.yml like so:

<pre class="lang:yaml">
cloud:
    azure:
        storage_account: myelasticsearchbackup
        storage_key: +4lYrUnfvhkortd8fEdL3RHTSsLCBhpZSPvHzVt78KKAtjiLHsY+yWq23LttKWd77jp9CI+xCK0Y6qohLOMTew==
</pre>

One final thing missing before being able to make a backup. We need to tell Elasticsearch to use the cloud-azure plugin to create backups. Make a HTTP request to your Elasticsearch using Postman, Sense or whatever client you prefer:

<pre class="lang:bash">
PUT http://localhost:9200/_snapshot/myname
{
    “type”: “azure”
}
</pre>

(you probably want another name than <em>myname</em> but that’s really up to you)

That’s it! We are ready to automate Elasticsearch backups using PowerShell. To create a backup, we need another HTTP request looking something like this:

<pre class="lang:bash">
PUT http://localhost:9200/_snapshot/myname/mybackup
</pre>

Again, you’d want to replace <em>mybackup</em> with something else. The important thing to notice is, that this name needs to be unique and you should generate a new name every day. For my PowerShell script, I’m using a date as part of the name:

<pre class="lang:powershell">
$server = "localhost"
$name = Get-Date -format yyyy-MM-dd
$url = "http://${server}:9200/_snapshot/azure/snapshot-${name}"

Invoke-RestMethod -Method Put -Uri $url
</pre>

The <code>Invoke-RestMethod</code> cmdlet serves perfectly for calling Elasticsearch from PowerShell. Notice how I format todays date and build an URL ending with <code>snapshot-2016-07-07</code> (or whatever date this is executed).

If I run the script from PowerShell, I get the following response from Elasticsearch:

<pre class="lang:json">
{
  “accepted”: true
}
</pre>

This doesn’t mean that everything is backup up, but rather that Elasticsearch accepted the request to back up the cluster. Sit tight and watch files start flowing into your new storage account.

To schedule this, I’ve set up a scheduled task in Windows, but you can schedule this anyway you’d like. The important thing with this script is, that it only runs once every day, since multiple executions would produce the same backup name.

As a final note, you shouldn’t worry about space consumption that much. Elasticsearch is smart enough to create incremental backups, which will only add changes to the storage account every day.