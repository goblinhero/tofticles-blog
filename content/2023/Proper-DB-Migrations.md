+++
title = 'Proper DB Migrations - Part one: Preparations'
date = 2023-11-22T10:15:03+01:00
draft = false
summary = 'Beginning to smell a lot like production!'
+++

The code for this post can be found [here](https://github.com/goblinhero/Anex/pull/23).

As earlier advertised, I had put in a SchemaExport for easy early testing, but now is the time to find something more robust: Enter [FluentMigrator](https://fluentmigrator.github.io/index.html). 

### What's it do?

It's a thin wrapper around raw SQL with a fluent interface, so instead of writing migrations with raw SQL with all the pitfalls that entails, you get a nice syntax, options for reusing snippets and so on. I've used it in the past and have been pretty satisfied with the simplicity of it all.

### The 'fun' of PostGres

I'm not terribly fond of a few things about PostGres - I understand why it is as it is, but that does not have me liking it. I'm talking about the case-sensitivity on all levels. So, `SELECT version FROM ...` could work while `SELECT Version FROM ...` wouldn't. This means you have to be a bit more careful about your migrations to have it fit with NHibernate's expectations - and coming from a MSSQL background, it will take some getting used to.

### Id's - the eternal question

There's pros and cons to everything, but let's compare the (most common) options:

#### Emails, strings and other natural keys

Don't. Just don't. It's 2023, you should know better.

#### Guids

Guids are an interesting option. They offer (the potential for) idempotency, ie. protection from clients accidently calling twice with the same data on a HTTP:POST endpoint and making doubles. This requires that the clients set the id before invoking the request. It's a loss of control but with quite nice benefits. 

Guids are, however, notorious for destroying index-performance on databases, since they are random in nature - so the physical storage of the rows can be terribly inefficient. It can be worked around, either by forcing the Database to reindex or in part by using sequential* guids.

#### Int64

Longs are another fairly decent option. They offer human readability, consider:

    /api/ledgertag/5433/posts

vs.

    /api/ledgertag/599f5149-6ee9-4e66-b499-804ca0fd428d/posts

Longs take up 4 bytes vs. Guids 16 bytes which is a significant difference, but not one large enough to settle the argument except for very specific cases.

#### DB-generated vs. assigned

Guids offer both options out of the box while you have to add the assigned model for longs (keeping track of the next number is the problem). The problem with DB-generated is generally that you need two roundtrips every-time you insert a row (since you need to retrieve the assigned id, afterwards) and handling bulk-inserts can be tricky.

For this project, I'll be using longs, mostly due to the readability and I'll use NHibernate's built-in option for using sequences with the [HiLo](https://www.thomashuysmans.be/nhibernate-hilo/) algoritm.

\* basically, the first part of the Guid is replaced by a time-dependant portion, so all ids created in the same time-period will be near each other in the index.