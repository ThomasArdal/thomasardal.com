---
ID: 799
post_title: Using Moq callbacks as Verify
author: Thomas Ardal
post_date: 2013-01-18 10:15:42
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/using-moq-callbacks-as-verify/
published: true
---
One thing using <a href="http://code.google.com/p/moq/" target="_blank">Moq</a> always bugged me. When needing to verify some method call, Moq provides a Verify-metod on the Mock object:<br/><br/>

<script src="https://gist.github.com/4562879.js"></script>

So, what's wrong with this piece of code? In fact nothing (if you ask me). The "problem" is the error message if the test fails:<br/><br/>

<img class="size-full wp-image-873 alignnone" alt="verify_error" src="http://thomasardal.com/wp-content/uploads/2013/01/verify_error.png" width="713" height="280" /><br/><br/>

Something fails! Moq is in fact pretty decent when it comes to error messages (compared to <a href="http://codevanced.net/post/Mocking-frameworks-comparison.aspx" target="_blank">other mocking frameworks</a> at least). Still, I don't think the error is obvious here. It takes some time to spot, that the first parameter of the AMethodCall-method have a spelling mistake. What we really wanted here is to do an assert on each parameter using <a href="http://www.nunit.org/" target="_blank">NUnit</a>.<br/><br/>

Even though callbacks in Moq isn't ment to fix this, it solves the problem quite well. One might argue, that we compromise a bit with <a href="http://c2.com/cgi/wiki?ArrangeActAssert" target="_blank">AAA</a>, though. Our test using callbacks look like this:<br/><br/>

<script src="https://gist.github.com/4563160.js"></script>

A bit more complex, but our error message now tells us exactly what's wrong:<br/><br/>

<img src="http://thomasardal.com/wp-content/uploads/2013/01/callback_error.png" alt="callback_error" class="alignright size-full wp-image-888" />