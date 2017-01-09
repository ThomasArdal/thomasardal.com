---
ID: 1760
post_title: >
  Updating a NuGet package across multiple
  projects/solutions
author: Thomas Ardal
post_date: 2016-01-28 08:13:53
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/updating-a-nuget-package-across-multiple-projectssolutions/
published: true
---
I'm currently working for at customer having separated their code into multiple projects and solutions all in the same Git repository. This posts is not about how to structure your code or if I like this way to separate responsibility or not. But I guess most of you have worked or work on a codebase like this.

A huge problem when splitting up your code like this, is when needing to update a NuGet package to a newer version. Do you update the version only in the project needing the new version or across all usages of this package? At least my opinion is that you want to use the same version of a package everywhere, but updating multiple projects/solutions takes time. To ease this process, I developed a small powershell script able to solve this problem:

<pre class="lang:powershell">
param(
    [Parameter(Mandatory=$true)]
    [string]$packageId
)

Get-ChildItem *.sln -recurse | %{.\\nuget.exe restore $_.fullname}

Get-ChildItem packages.config -Recurse `
  | Where-Object {$_ | Select-String -Pattern $packageId} `
  | %{.\\nuget.exe update -Id $packageId $_.FullName}
</pre>

The script accepts the NuGet package ID as input. Before actually updating the package, I restore packages on all solution files found in my repository. Having all packages restored is a prerequisite to actually updating anything through NuGet.

In line 8, I search for all packages.config files containing the NuGet package ID. On all results, I run the NuGet <code>update</code> command.