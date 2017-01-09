---
ID: 661
post_title: >
  Simple code with refactoring and Monads
  for .NET
author: Thomas Ardal
post_date: 2012-10-08 11:21:43
post_excerpt: ""
layout: post
permalink: >
  http://thomasardal.com/simple-code-with-refactoring-and-monads-for-net/
published: true
---
I had a fun task a couple of weeks ago, simplifying some code that another dude wrote. It's always more fun to optimize the hell out of other people’s code, right? :) The problem code was pretty much a proof of concept and no considerations about good or bad design had gone into writing it. The problem solved by the code written was basically a LINQ-like expression, using the excellent <a href="http://www.elasticsearch.org/" target="_blank">ElasticSearch</a> .NET client <a href="https://github.com/Mpdreamz/NEST" target="_blank">NEST</a>. The syntax is a bit weird at first, but after having used the API for a while, I've decided to like it. Back to the code, this is a snippet of the code handed to me:

[csharp]
var results = _elasticClient.Search&lt;CarSearchable&gt;(
    s =&gt;
        {
            var mustQueries = new List&lt;Action&lt;FilterDescriptor&lt;CarSearchable&gt;&gt;&gt;();
            if (registrationYearFrom.HasValue || registrationYearTo.HasValue)
            {
                mustQueries.Add(
                    mq =&gt; mq.Range(
                        r =&gt;
                            {
                                r.OnField(x =&gt; x.RegistrationDate);
                                if (registrationYearFrom.HasValue)
                                {
                                    r.From(registrationYearFrom.Value.ToString());
                                }

                                if (registrationYearTo.HasValue)
                                {
                                    r.To(registrationYearTo.Value.ToString());
                                }
                            }));
            }

            if (carTypeId.HasValue)
            {
                mustQueries.Add(mq =&gt; mq.Term(x =&gt; x.CarTypeId, carTypeId.ToString()));
            }

            if (!string.IsNullOrWhiteSpace(freetextQuery))
            {
                mustQueries.Add(
                    mq =&gt;
                    mq.Query(
                        query =&gt;
                        query.Text(
                            txt =&gt;
                            txt.OnField(f =&gt; f.FreeText).QueryString(freetextQuery).Operator(Operator.and))));
            }

            s.Filter(f =&gt; f.Bool(b =&gt; b.Must(mustQueries.ToArray())));
            return s;
        });
[/csharp]

Just a quick explanation of what's going on here. I'm doing a search on ElasticSearch with a range filter, a term and a full text search. Those of you who have ever wrote queries against Solr or Lucene probably understand exactly what is going on here, but for you other guys, you’ll just see this as your standard LINQ query against some database.

Time for some refactoring. The first step was to get rid of that nasty code block adding the range filter. The refactoring tools in R# quickly transformed the code into something much cleaner:

[csharp]
var results = _elasticClient.Search&lt;CarSearchable&gt;(
    s =&gt;
        {
            var mustQueries = new List&lt;Action&lt;FilterDescriptor&lt;CarSearchable&gt;&gt;&gt;();
            var range = Range(registrationYearFrom, registrationYearTo, x =&gt; x.RegistrationDate);
            if (range != null)
            {
                mustQueries.Add(range);
            }

            if (carTypeId.HasValue)
            {
                mustQueries.Add(mq =&gt; mq.Term(x =&gt; x.CarTypeId, carTypeId.ToString()));
            }

            if (!string.IsNullOrWhiteSpace(freetextQuery))
            {
                mustQueries.Add(
                    mq =&gt;
                    mq.Query(
                        query =&gt;
                        query.Text(
                            txt =&gt;
                            txt.OnField(f =&gt; f.FreeText).QueryString(freetextQuery).Operator(Operator.and))));
            }

            s.Filter(f =&gt; f.Bool(b =&gt; b.Must(mustQueries.ToArray())));
            return s;
        });
[/csharp]

I've cheated a bit, because most of the code has been extracted to a new method. Let's look at that later. First of all it bugs me that the code is still 30 lines long and all I want to do is add three filters. The problem here is all the null checking, which makes what should be a simple code very hard to look at. <a href="https://github.com/sergun/monads.net" target="_blank">Monads</a> to the rescue! Using monads, the ugly if statements can be written in a much nicer way:

[csharp]
var results = _elasticClient.Search&lt;CarSearchable&gt;(
    s =&gt;
        {
            var mustQueries = new List&lt;Action&lt;FilterDescriptor&lt;CarSearchable&gt;&gt;&gt;();
            Range(registrationYearFrom, registrationYearTo, x =&gt; x.RegistrationDate).Do(mustQueries.Add);
            carTypeId.Do(x =&gt; mustQueries.Add(mq =&gt; mq.Term(cs =&gt; cs.CarTypeId, carTypeId.ToString())));
            freetextQuery.If(x =&gt; !string.IsNullOrWhiteSpace(x))
                         .Do(
                             x =&gt;
                             mustQueries.Add(
                                 mq =&gt;
                                 mq.Query(
                                     query =&gt;
                                     query.Text(
                                         txt =&gt;
                                         txt.OnField(f =&gt; f.FreeText).QueryString(freetextQuery).Operator(Operator.and)))));

            s.Filter(f =&gt; f.Bool(b =&gt; b.Must(mustQueries.ToArray())));
            return s;
        });
[/csharp]

Rather than checking the input values for null, I use the cool Do extension-method in Monads, which executes the given delegate, if the input value is not null. freetextQuery is a string and I want to check for both null and empty string before adding the query. The Do-method only checks for null, but luckily Monads provides us with an If-method, making it possible to provide an expression evaluating to true or false just as an ordinary if-construct. I use a lot of lines on the free text query and maybe I could have squeezed it down to fewer lines, but I'm actually pretty satisfied with the code now. Remember the extracted method? Well, that bastard is still pretty nasty:

[csharp]
private Action&lt;FilterDescriptor&lt;CarSearchable&gt;&gt; Range(int? from, int? to, Expression&lt;Func&lt;CarSearchable, object&gt;&gt; field)
{
    if (from.HasValue || to.HasValue)
    {
        return mq =&gt; mq.Range(
            r =&gt;
                {
                    r.OnField(field);
                    if (from.HasValue)
                    {
                        r.From(from.Value.ToString());
                    }

                    if (to.HasValue)
                    {
                        r.To(to.Value.ToString());
                    }
                });
    }

    return null;
}
[/csharp]

Doing a bit of Monads magic simplifies the two if statements:

[csharp]
private Action&lt;FilterDescriptor&lt;CarSearchable&gt;&gt; Range(int? from, int? to, Expression&lt;Func&lt;CarSearchable, object&gt;&gt; field)
{
    if (from.HasValue || to.HasValue)
    {
        return mq =&gt; mq.Range(
            r =&gt;
                {
                    r.OnField(field);
                    from.Do(x =&gt; r.From(x.Value.ToString()));
                    to.Do(x =&gt; r.To(x.Value.ToString()));
                });
    }

    return null;
}
[/csharp]

It's probably a matter of taste whether you like the Monads extension methods or not. I’ve got to admit: I love it! Any ideas for improvement?