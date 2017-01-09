---
ID: 371
post_title: >
  Unit-testing HtmlHelper extension
  methods
author: Thomas Ardal
post_date: 2012-02-23 16:01:05
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/unit-testing-htmlhelper-extension-methods/
published: true
---
I sometimes find myself doing extension methods for the HtmlHelper class in ASP.NET MVC. I like HtmlHelper, but I don’t really dig when the methods generate HTML. This is what partial views are for, right? I usually add small utility methods related to a single view on the view model, but when dealing with shared methods, the HtmlHelper seems like the best choice.

Extension methods in general are pretty easy to unit-test, due to the fact that they are just static methods. However, extension methods on HtmlHelper have all kinds of information available through the “this HtmlHelper” parameter, which can make these methods extremely hard to test. This is the helper method I use to create a new instance of the HtmlHelper class:

[csharp]
public HtmlHelper&lt;T&gt; CreateHtmlHelper&lt;T&gt;(ViewDataDictionary viewData)
{
    var cc = new Mock&lt;ControllerContext&gt;(
        new Mock&lt;HttpContextBase&gt;().Object,
        new RouteData(),
        new Mock&lt;ControllerBase&gt;().Object);

    var mockViewContext = new Mock&lt;ViewContext&gt;(
        cc.Object,
        new Mock&lt;IView&gt;().Object,
        viewData,
        new TempDataDictionary(),
        TextWriter.Null);

    var mockViewDataContainer = new Mock&lt;IViewDataContainer&gt;();

    mockViewDataContainer.Setup(v =&gt; v.ViewData).Returns(viewData);

    return new HtmlHelper&lt;T&gt;(
        mockViewContext.Object, mockViewDataContainer.Object);
}
[/csharp]

The method uses Moq to create mock objects for various ASP.NET MVC classes, but it can easily be ported to Rhino Mocks or other mocking frameworks. Testing extension methods on HtmlHelper is now piece of pie. In the following example, I test an imaginary extension method, named SplitModel, using the CreateHtmlHelper method:

[csharp]
[Test]
public void CanSplitString()
{
    // Arrange
    var htmlHelper =
        CreateHtmlHelper&lt;string&gt;(
            new ViewDataDictionary(&quot;Hello World&quot;));

    // Act
    string[] result = htmlHelper.SplitModel();

    // Assert
    Assert.That(result.Length, Is.EqualTo(2));
}
[/csharp]