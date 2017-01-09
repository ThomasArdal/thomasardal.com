---
ID: 1454
post_title: 'Exception Filters in C# 6.0'
author: Thomas Ardal
post_date: 2014-09-30 12:31:45
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/exception-filters-in-c-6-0/
published: true
---
In the last post inspired by <a href="http://gotocon.com/aarhus-2014/presentations/show_presentation.jsp?oid=6227" target="_blank">The Future of C#</a> talk at GOTO, I introduced you to <a href="http://thomasardal.com/declaration-expressions-and-static-using-in-c-6-0/">Declaration Expressions and Static Using</a>. In this post I will take a look at Exception Filters in C# 6.0. Exception Filters is one of two improvements for exception handling, the other being able to do await calls from catch blogs. Await in catch blocks is something that should have been in the framework from the time async/await where originally introduced, why I don't want to pay too much attention to it in this post. Makes sense implementing it.

So what's this exception filtering all about? You probably wrote code looking similar to this at some point:

<pre class="lang:c# decode:true " >try
{
    DoSomeHttpRequest();
}
catch (System.Web.HttpException e)
{
    switch (e.GetHttpCode())
    {
        case 400:
            WriteLine("Bad Request");
        case 500:
            WriteLine("Internal Server Error");
        default:
            WriteLine("Generic Error");
    }
}</pre> 

The catch blog in this example switches over a HTTP status code and provide different behavior based on the status code. Notice that all handling of the HttpException is located inside a single catch block.

The above example written using Exception Filters could look something like this:

<pre class="lang:c# decode:true " >try
{
    DoSomeHttpRequest();
}
catch (System.Web.HttpException e) if (e.GetHttpCode() == 400)
{
    WriteLine("Not Found");
}
catch (System.Web.HttpException e) if (e.GetHttpCode() == 500)
{
    WriteLine("Internal Server Error");
}
catch
{
    WriteLine("Generic Error");
}</pre> 

In the updated example we define a catch block for each condition in the switch block. The new part is the if statement added to the right of each catch specification. The idea here is to tell .NET to only execute each catch block if both the exception type matches and the statement inside the if match. Like usual .NET will evaluate each catch block from the top, falling through each block until both exception type and filter matches. In case the HTTP status code is different than 400, 404 and 500, the generic catch block is executed.

Let's see how this code looks decompiled:

<pre class="lang:c# decode:true " >try
{
	Program.DoSomeHttpRequest();
	return;
}
catch
{
	Console.WriteLine("Generic Error");
	return;
}
object arg_0B_0;
HttpException expr_10 = arg_0B_0 as HttpException;
int arg_28_0;
if (expr_10 == null)
{
	arg_28_0 = 0;
}
else
{
	HttpException e = expr_10;
	arg_28_0 = (((e.GetHttpCode() == 400) &gt; false) ? 1 : 0);
}
endfilter(arg_28_0);
object arg_3A_0;
HttpException expr_3F = arg_3A_0 as HttpException;
if (expr_3F != null)
{
	goto IL_46;
}
int arg_57_0 = 0;
goto IL_57;
IL_46:
HttpException e2 = expr_3F;
arg_57_0 = (((e2.GetHttpCode() == 500) &gt; false) ? 1 : 0);
IL_57:
endfilter(arg_57_0);</pre> 

Talking about ugly code! Neither ILSpy nor <a href="http://tryroslyn.azurewebsites.net/" target="_blank">tryroslyn.azurewebsites.net</a> is able to show the entire code ("Not Found" etc. is missing from the decompiled code), but looking through the generated IL, the whole thing is still there.