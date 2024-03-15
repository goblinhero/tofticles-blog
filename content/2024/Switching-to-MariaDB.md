+++
title = 'Switching to MariaDB'
date = 2024-03-15T07:27:12+01:00
draft = false
summary = 'Aesthetics are important too'
+++

The code for this post can be found [here](https://codeberg.org/Tofticles/Anex/pulls/37) and [here](https://codeberg.org/Tofticles/Anex/pulls/38).

Up until now, I've used the [PostGre](https://www.postgresql.org/) as the backing store for Anex. There are a few things that annoy me about it. It's not something that makes it unviable or anything dire like that, but... Table and column names have to be lowercase for everything to work out of the box.

Everything in Dotnet is PascalCase for type names (which most the time translates to table names) and camelCase for variables. So, I ripped out PostGre in favour of [MariaDB](https://mariadb.org/). As you can see from pull request, it was mostly a matter of correcting case on string constants and changing which .dll to use for the data access. The beauty of NHibernate.

Since this was almost too easy - I decided to disable nullability from all the projects as well. While in theory, it should lead to better code - there is a lot of caveats. As soon as you have ORMs (using reflection etc.) and public endpoints involved, you basically have to put a ? after almost all reference types anyway - and then the point of nullability is moot, in my opinion at least.