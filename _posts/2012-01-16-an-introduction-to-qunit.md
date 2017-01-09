---
ID: 430
post_title: An introduction to QUnit
author: Thomas Ardal
post_date: 2012-01-16 18:37:13
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/an-introduction-to-qunit/
published: true
---
One of the areas that I never really dug into is unit testing JavaScript code. I have a background as a backend developer and for me, NUnit has been sufficient for years. During the last couple of years, I started implementing a few websites and also switched my working place from Trifork to eBay so I could expand my web knowledge. The switch opened up an entire new world for me, the world of JavaScript. I had previously written some JavaScript before starting at eBay, but I never really had the time to sit down and fully understand the language. Therefore, I never really used the full potential of JavaScript, but I still wrote a lot of the application on the server (in c#). That's why I decided to look at unit-testing JavaScript.

Some developers argue that JavaScript shouldn't be unit-tested because it is part of the UI. These people typically use something like Selenium to test the client, but in my experience, Selenium tests are like integration-tests: hard to write and maintain. C’mon guys, this is one of the reasons why we write unit-tests of the server, to be able to test single units and ease the debugging process when bugs are introduced. Unit-testing the UI becomes essential in today’s thick HTML5 applications.

For my JavaScript unit test experiment, I needed a framework and while there are <a href="http://en.wikipedia.org/wiki/List_of_unit_testing_frameworks#JavaScript" target="_blank">a lot to choose from</a>, I wanted to try out <a href="http://docs.jquery.com/QUnit" target="_blank">QUnit</a>. QUnit is from the guys who brought you jQuery and is actually used to test jQuery itself. I simply can't say it enough, but I LOVE jQuery. For this one reason I decided to try and see if I would love QUnit as well.

QUnit turned out to be pretty straight forward. Let's look at a simple test. Save the following JavaScript to a file called tests.js (or a name of your choice):

[javascript]
test(&quot;Can Add Two Strings&quot;, function () {
  // Arrange
  var str1 = &quot;Hello&quot;;
  var str2 = &quot;World&quot;;

  // Act
  var result = str1 + &quot; &quot; + str2;

  // Assert
  equal(result, &quot;Hello World&quot;);
});
[/javascript]

Admittedly, this is probably not the best test, but it illustrates the basic features of QUnit. Every test is specified by calling a test-method from QUnit with a name of the test and a callback containing the actual test. Like NUnit, the asserts are specified by calling methods. <em>equal</em> matches the Assert.AreEqual method in NUnit and there are similar methods like ok for IsTrue and so on. I really like the simple yet strong-type way of writing the test method. No method name prefixes. No attributes. Also, the QUnit guys choose the correct order of the parameters for the equal method. In NUnit, the expected value goes first, which always confused me.

Unit tests exist to be run. So, what do we do to execute the test above? QUnit supplies us with a style sheet and some JavaScript files, which makes the execution pretty straightforward. An even better solution is to use the QUnit-runner in ReSharper (ReSharper 6.x only, sorry Stone Age dudes). In order for ReSharper to know that the tests.js file contains test-methods, you need to add a script reference on the top of the js-file:

[javascript]
/// &lt;reference path=&quot;CodeUnderTest.js&quot;/&gt;
[/javascript]

The CodeUnderTest.js-filename tells ReSharper that the file contains unit-tests. The file-path is there for information only and should not exist on the file system. In order to get IntelliSense while writing your tests, it's a good idea to add a reference to the qunit.js-file as well. This reference is not mandatory though, because ReSharper comes bundled with its own version of QUnit. Well now for the good part: Look Ma, my QUnit test is runnable through ReSharper:

<img class="alignnone size-full wp-image-482" title="qunit2" src="http://thomasardal.com/wp-content/uploads/2012/01/qunit2.png" alt="" width="591" height="368" />

Working with QUnit has been like a dream come true. I wouldn't have guessed that testing JavaScript could be that easy. There's no excuse not to start implementing unit-tests of JavaScript!