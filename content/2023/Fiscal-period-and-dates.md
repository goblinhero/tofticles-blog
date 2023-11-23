+++
title = 'Fiscal Period and Dates'
date = 2023-11-23T14:39:18+01:00
draft = false
summary = 'C# and date handling... I need a rope and a shovel.'
+++

The code can be found [here](https://github.com/goblinhero/Anex/pull/27).

If you haven't watched [Tom Scott](https://www.youtube.com/watch?v=-5wpm-gesOY), yet - go do so. You'll come back knowing at least a bit about the pains we are talking about when it comes to handling date and time.

In accounting we use economical dates, ie. there is no time part. Same as you do with your birthday - you (probably) don't keep track at which point in the day you actually started breathing oxygen. Time-zones are a constant pain if you use the built-in `DateTime` type in C#, because a DateTime can be in one of three states:

    Local           Basically, what is the time according to the server's timezone taking into account DST and all that crap. (important distinction - your client might be in another zone)
    Utc             Basically, what is the time on the date-line - also known as Zulu-time (this is used by the military among others when we -really- don't want to arrive an hour late or early)
    Unspecified     Yeah, this one is wonky - basically, we have not (yet) decided (sadly, this is the default)

### Databases are smart

For as long as I can remember, databases have supported date-only columns (for good reason - back then storage was -expensive-, so if you could save a few bytes here and there, you absolutely did!). MSSQL, PostGres, MySQL and MariaDB calls it date as the data type.

### Enter DateOnly to save the day?

In .NET 6, Microsoft added DateOnly and TimeOnly. All celebrate - however, as is sometimes the issues, the type was introduced, but support for it? Not-so-much. The default serializer does not support it. Entity Framework does not support it. NHibernate does not support it. The list goes on. For SwashBuckle and the serializer, [maxc137](https://www.nuget.org/profiles/maxc137) has come to the rescue with his [NuGet package](https://www.nuget.org/packages/DateOnlyTimeOnly.AspNet). Brilliant work - easy to install and the code does what it needs to do. Just remember to update your SwashBuckle version to at least 6.5.0 (or use the extra syntax as maxc137 writes on NuGet).

Honestly, I do not know how to add support in EF Core, but I do know how to do so in NHibernate - they have IUserType. Before this I already use it for DateTimes to ensure they are stored in UTC. The implementation can be found in the PR, it is not rocket science, so should be fairly easy to implement. And now we have Fiscal Periods in Anex.