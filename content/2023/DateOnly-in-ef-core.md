+++
title = 'DateOnly in Ef Core'
date = 2023-11-24T07:57:18+01:00
draft = false
summary = 'Turns out, it is supported in EF Core 8.'
+++

[Here](/2023/fiscal-period-and-dates/), I wrote about support for DateOnly in NHibernate and the lacking support in EF Core - I've since looked into the matter. 

There is indeed support for DateOnly in EF Core... Now, in .NET 8. Two major versions after DateOnly and TimeOnly were introduced in .NET 6. If you're stuck on a previous version, [ErikEJ](https://erikej.github.io/) has a solution [here](https://erikej.github.io/efcore/sqlserver/2023/09/03/efcore-dateonly-timeonly.html) (including a nice [Nuget package](https://www.nuget.org/packages/ErikEJ.EntityFrameworkCore.SqlServer.DateOnlyTimeOnly)).

All rejoice!