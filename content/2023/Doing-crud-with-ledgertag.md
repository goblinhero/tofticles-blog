+++
title = 'Doing Crud With Ledgertag'
date = 2023-11-21T14:21:32+01:00
draft = false
summary = 'Transactions are fun!'
+++

Code for this post can be found [here](https://github.com/goblinhero/Anex/pull/21).

While waiting for the solution to deploy - a few points of interest. I've added `Create` and `Delete` endpoints for LedgerTag, so we now have CRD (Update will be it's own post as it is a bit more involved to support both the PUT and PATCH HTTP verbs - more on that later).

It is at this point that my up-front work is starting to pay off. Mostly it was adding the interface and implementations for Commands in NHibernate. You get nice error messages back if you do something silly and everyone is happy.

Next post will be a post-mortem. I managed to break the site when adding the test library, so it is temporarily disabled until I figure a solution.