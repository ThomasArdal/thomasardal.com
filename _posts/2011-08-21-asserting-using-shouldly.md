---
ID: 356
post_title: Asserting using Shouldly
author: Thomas Ardal
post_date: 2011-08-21 18:09:25
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/asserting-using-shouldly/
published: true
---
I've been praising the "new" Assert.That syntax, since introduced in NUnit 2.4. Expressing your tests using a constraint based API, rather than the old Are* and Is* API, makes reading your tests piece of pie. Take a look at the following test sample:

[csharp]
var result = &quot;Hello World&quot;.Split(new[] {' '});
Assert.AreEqual(2, result.Length);
Assert.AreEqual(&quot;Hello&quot;, result[0]);
Assert.AreEqual(&quot;World&quot;, result[1]);
[/csharp]

It doesn't take 10 years of programming to understand the code here, but I've always had a problem with the order of the parameters to the AreEqual-method. The problem is, that the expected value goes before the actual value. First it is not readable. Second when people by accidently swap the order of the parameters, problems starts happening. As long as the test is running successfully, there's no problem. The static AreEqual-method on the Assert class, simply calls equals on the two objects and equals doesn't care about the order of the parameters. The problem arises when the objects aren't equal. NUnit throws an exception, with an error message indicating what went wrong. If you've swapped the parameters, NUnit says something like: "Expected 'A', Should have been 'B'". When in fact expected it's the other way around. I have once spend two hours of debugging, just to find out about the switched parameters. DOUGH! For these two reasons, switching to the Assert.That-syntax, makes it all worth it. The previous code sample can be written like this:

[csharp]
var result = &quot;Hello World&quot;.Split(new[] {' ');
Assert.That(result.Length, Is.EqualTo(2));
Assert.That(result[0], Is.EqualTo(&quot;Hello&quot;));
Assert.That(result[1], Is.EqualTo(&quot;World&quot;));
[/csharp]

Notice how the actual value now goes first, and the constraint (Is.EqualTo) goes last. No confusion about the order of the parameters here. Also the code is now pretty much self explained (Assert that result length is equal to 2).

But we can do better than this. I stumbled upon a framework called <a href="http://shouldly.github.com/" target="_blank">Shouldly</a> last year, but didn't really saw the potential until a few months ago. Shouldly uses .NET extension methods to simplify the assert syntax even more, making it clean and readable. Our sample written with Shouldly looks like this:

[csharp]
var result = &quot;Hello World&quot;.Split(new[] {' '});
result.Length.ShouldBe(2);
result[0].ShouldBe(&quot;Hello&quot;);
result[1].ShouldBe(&quot;World&quot;);
[/csharp]

Why is this better than the Assert.That syntax one might wonder - well, it is probably a matter of taste. I really like the idea with using extension methods to achieve the shortest possible syntax for asserting a value. Why do you need to keep repeating yourself by writing <em>Assert.</em> before every single assert? With Shouldly you don't! That's fluent baby!