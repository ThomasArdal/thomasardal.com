---
ID: 1905
post_title: AppSettings in ASP.NET Core
author: Thomas Ardal
post_date: 2016-10-28 09:30:22
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/appsettings-in-asp-net-core/
published: true
---
I'm currently writing a range of blog posts about ASP.NET Core. This is a cross post of <a href="http://blog.elmah.io/appsettings-in-aspnetcore/?utm_source=thomasardal">AppSettings in ASP.NET Core</a> from the <a href="http://blog.elmah.io/?utm_source=thomasardal">elmah.io blog</a>. Also make sure to check out my later posts <a href="http://blog.elmah.io/config-transformations-in-aspnetcore/">Config transformations in ASP.NET Core</a> and <a href="http://blog.elmah.io/configuration-with-azure-app-services-and-aspnetcore/">Configuration with Azure App Services and ASP.NET Core</a>.

Most parts of elmah.io consist of small services. While they may not be microservices, they are in fact small and each do one thing. We recently started experimenting with ASP.NET Core (or just Core for short) for some internal services and are planning a number of blog posts about the experiences we have made while developing these services. This is the first part in the series about the configuration system available in Core.

Core doesn't have the concept of <code>appSettings</code>, <code>connectionStrings </code>and other known features from ASP.NET. In fact configuration isn't even located in web.config. Let's create a new Core website in Visual Studio, to have a website to play with throughout the series:

<img src="http://thomasardal.com/wp-content/uploads/2016/10/new_aspnetcore_website1-768x533.png" alt="new_aspnetcore_website1" width="768" height="533" class="aligncenter size-medium_large wp-image-1906" />

When creating the website from the template, you will get a bunch of files generated. The interesting file in this context is named <code>appsettings.json</code>. <code>appsettings.json</code> is meant to replace settings previously located in <code>web.config</code>, but the file could have been named anything. In fact Core supports both JSON, XML and ini files. To tell Core about this configuration file, a couple of lines have been added to the <code>Startup.cs</code> file when generating the project:

<pre class="lang:csharp">
var builder = new ConfigurationBuilder()
    .SetBasePath(env.ContentRootPath)
    .AddJsonFile("appsettings.json", optional: true, reloadOnChange: true)
    .AddJsonFile($"appsettings.{env.EnvironmentName}.json", optional: true)
    .AddEnvironmentVariables();
Configuration = builder.Build();
</pre>

Notice how the <code>appsettings.json</code> file is added to the <code>ConfigurationBuilder</code>. Also pay attention the line following that, adding another JSON file named <code>appsettings.{env.EnvironmentName}.json</code>. We will get back to this file in the next post.

To add application settings to the application, open <code>appsettings.json</code> and add the following lines before the <code>Logging</code> part already there:

<pre class="lang:json">
"AppSettings":{
    "Hello": "World"
},
"Logg...
</pre>

Unlike settings in <code>web.config</code>, configuration in <code>appsettings.json</code> can be hierarchical. We use this to define a new section called <code>AppSettings</code> with a key called <code>Hello</code>. Both <code>AppSettings</code> and <code>Hello</code> could have been named anything we'd like.

Core uses an options pattern to resolve settings from <code>appsettings.json</code>. To read the <code>Hello</code> setting from the configuration file, add the following line to the <code>ConfigureServices</code>-method in <code>Startup.cs</code>:

<pre class="lang:csharp">
var appSettings = Configuration.GetSection("AppSettings");
</pre>

This tells Core to read the <code>AppSettings</code> object from <code>appsettings.json</code>. Core is heavily based on dependency injection, why you no longer want to call <code>ConfigurationManager</code> from within your code. Instead you inject various options in your controllers. To register <code>AppSettings</code> in the container, create a new C# class representing the settings:

<pre class="lang:csharp">
public class AppSettings
{
    public string Hello { get; set; }
}
</pre>

Then register the settings in the container by adding the following line to <code>Startup.cs</code> (just after the call to <code>GetSection</code>):

<pre class="lang:csharp">
services.Configure<AppSettings>(appSettings);
</pre>

This tells the Core container to register the class of type <code>AppSettings</code> with the configuration section resolved from <code>appsettings.json</code>.

Now that we have settings in the container, injecting them into a controller is easy using the options pattern. Add a constructor to the HomeController looking like this:

<pre class="lang:csharp">
private readonly AppSettings _appSettings;

public HomeController(IOptions<AppSettings> appSettings)
{
    _appSettings = appSettings.Value;
}
</pre>

We inject an instance of <code>IOptions</code> with a generic type of <code>AppSettings</code>, which we just saw in <code>Startup.cs</code>. To utilize the <code>Hello</code> option, we'll send the value of <code>Hello</code> to the model in the <code>Index</code>-method:

<pre class="lang:csharp">
public IActionResult Index()
{
    ViewBag.Hello = _appSettings.Hello;
    return View();
}
</pre>

And finally show the value in <code>View.cshtml</code>:

<pre class="lang:html">
@{
    ViewData["Title"] = "Home Page";
}

<h1>Hello @ViewBag.Hello</h1>
</pre>

The result is somewhat not surprising:

<img src="http://thomasardal.com/wp-content/uploads/2016/10/my_core_website1-768x440.png" alt="my_core_website1" width="768" height="440" class="aligncenter size-medium_large wp-image-1908" />

In the next post, we'll discuss how to switch configuration based on environments.