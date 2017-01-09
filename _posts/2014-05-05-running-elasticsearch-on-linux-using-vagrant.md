---
ID: 1314
post_title: >
  Running Elasticsearch on Linux using
  Vagrant
author: Thomas Ardal
post_date: 2014-05-05 12:03:02
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/running-elasticsearch-on-linux-using-vagrant/
published: true
---
I have been running Elasticsearch on Windows for almost two years now. As a .NET developer and general happy Windows user, choosing a known environment have some advantages. In fact Elasticsearch runs pretty smooth on Windows, but it’s no secret, that the support for running Elasticsearch on Linux has always been better. Most of the documentation and blog posts around the web use Linux examples and most of the third-party products that Elasticsearch are recommending also run on Linux.

As a .NET developer, you’re typically not running Linux and most of the .NET folks I know, do everything to avoid getting to know anything about Linux. One option is to run Elasticsearch on Linux on your production environment and still host it on Windows on your developer machine. The ideal solution is of course to use the same OS both localhost and in production. <a href="http://www.vagrantup.com/" target="_blank">Vagrant</a> to the rescue!

<img src="http://thomasardal.com/wp-content/uploads/2014/05/logo_vagrant-81478652.png" alt="logo_vagrant-81478652" width="248" height="64" class="alignright size-full wp-image-1317" />Vagrant is an open source software product for configuring virtual machines. Below the hood, Vagrant is pretty much a wrapper for other virtualization products like VirtualBox and VMWare with some nice configuration options on top. Getting up and running with a new virtual machine, typically require you to download a preinstalled image, but it is fully supported to install your own images (both Linux and Windows). In this post I will show you how to get up and running hosting Elasticsearch on Ubuntu on your Windows machine.

Start by installing <a href="https://www.virtualbox.org/" target="_blank">Oracle VirtualBox</a> 4.3.6 and <a href="http://www.vagrantup.com/" target="_blank">Vagrant</a> 1.4.3. Feel free to use newer versions of both products, but Vagrant and VirtualBox got a long history of incompatibility. New virtual machines are created from a Vagrant configuration file (simply named VagrantFile):

<script src="https://gist.github.com/ThomasArdal/26510a4c01ea26caeb0b.js"></script>

The important part of the configuration is prefixed with config.vm.*. I’m using a preinstalled Ubuntu image named precise32. Vagrant comes with support for both Puppet and Chef, but as I don’t have much knowledge about these products, I’ll stick to running simple bash scripts. In line 7 I tell Vagrant to execute a script named bootstrap.sh, when creating the VM. In line 8 I configure Vagrant to forward requests to port 9200 on my local machine, to port 9200 on the new VM. This way requesting http://localhost:9200/, forwards the request to port 9200 on the Linux VM.

My bootstrap.sh is pretty simple too:

<script src="https://gist.github.com/ThomasArdal/8c792747d2292588490e.js"></script>

The script should be self-documenting, but the idea here is to install Java and Elasticsearch. In the last line I install <a href="http://mobz.github.io/elasticsearch-head/" target="_blank">head</a>, which is a great management plugin for Elasticsearch. This is of course optional.

To create the new VM, navigate to the folder containing your VagrantFile and execute:

<code>vagrant up</code>

This produces a fair amount of log statements. With only the important lines extracted, the log looks something like this:

<script src="https://gist.github.com/ThomasArdal/a9a2c1e77c55781c6452.js"></script>

The Ubuntu image is fetched from the web and Java, Elasticsearch and head are installed. Now the new VM is created and running. To proof everything is working, visit head in the browser:

<img src="http://thomasardal.com/wp-content/uploads/2014/05/head.png" alt="head" width="796" height="526" class="aligncenter size-full wp-image-1323" />

Voila! Your very own Elasticsearch hosted on Linux.

This post were produced by a mix of my own knowledge, a nice and much more complex bash script that my talented co-worker <a href="https://twitter.com/schultzdk" target="_blank">Jesper</a> produced for the project we are working on at <a href="http://www.ebaycareers.com/home.aspx" target="_blank">eBay</a>, as well as a great post on <a href="http://mookid.dk/oncode/archives/3518" target="_blank">installing Elasticsearch on Linux by Mookid</a>.