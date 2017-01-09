---
ID: 363
post_title: >
  Avoid ReSharper warning when doing
  multiple asserts
author: Thomas Ardal
post_date: 2011-10-03 19:52:32
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/avoid-resharper-warning-when-doing-multiple-asserts/
published: true
---
Since I started writing my NUnit asserts using Assert.That* instead of Assert.Are*, I've been hooked on the new syntax. The constraint API makes asserts readable and helps you specifying the expected and actual values at their proper places. One thing that always displeased my eye for perfection, was the R# warning shown when doing multiple asserts on the same value:

<a href="http://thomasardal.com/wp-content/uploads/2011/10/resultnull.png"><img class="alignnone size-full wp-image-388" title="resultnull" src="http://thomasardal.com/wp-content/uploads/2011/10/resultnull.png" alt="" width="665" height="97" /></a>

R# tries to tell me, that <em>result</em> may be null. That's just stupid, right? We just asserted, that <em>result</em> is in fact not null. While this definitely is a bug in R#, there is an easy fix for this. Recently I was looking some more at the That-method overloads and found a method, which is probably known but everyone than me. The overload takes a simple bool, making it easy to suppress the R# warning:

<a href="http://thomasardal.com/wp-content/uploads/2011/10/resultnotnull.png"><img class="alignnone size-full wp-image-389" title="resultnotnull" src="http://thomasardal.com/wp-content/uploads/2011/10/resultnotnull.png" alt="" width="665" height="97" /></a>

What is the difference here? Rather than using the constraints API in NUnit for asserting the value being not null, I simply write a statement translatable to a bool value (result != null). This should make R# happy and to be honest, the code still looks pretty readable to me.