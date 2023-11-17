+++
title = 'Anex Now With Database'
date = 2023-11-17T07:44:34+01:00
draft = false
summary = 'NHibernate for the win!'
+++

The merge request can be found [here](https://github.com/goblinhero/Anex/pull/1).

Okay - so quite a bit going on here. I've added a database with MSSQL Express just to test that my code actually works. A little caveat if you try to run the code yourself, if you run with the Docker file, remember that it is on an isolated network, so it will not be able to connect directly to your local SQL server. I'm not saying that I wasted three hours yesterday trying in vain to get it connected - I'm simply saying, find a guide if you want to attempt it.

Back to the code. I like Command-Query-Responsibility-Separation, so my ISessionHelper follows that pattern - for now it only supports querying, again to just do a vertical slize and replace my static dictionary with something backed by a database.

A good question would be, why I call it a ISessionHelper and not a repository - I'm glad you thought of that. I consider the ISession a generic repository since you are working with Dto's and Entities directly in the query syntax. And this brings me to: Why NHibernate when Entity Framework is there. Another good question. It comes down to a few factors outlined below:

 ### I don't -like- Entity Framework. 
 
 I mean, it gets the job done, but it rubs me in all the wrong ways. I've seen it lead down dark paths if you use it the default way, publishing your tables directly. Just the fact that there is a .SaveChanges call is a code-smell to me.

 ### I know NHibernate

 I've used it extensively since ~2010 and while there are subtleties that I'm not a fan of (like the fact that NHibernate is a port from Java, originally) - I like the philosophy of it. The framework is built around the idea that the domain model (the entities, the business logic - pick your naming) should be as oblivious as possible to how it is persisted. It is different concerns and shouldn't be mixed. Yes, my entities have .Id and .Version, which are infrastructure designs, but I'm not -that- purist. Also, the event-model of NHibernate is really nice to work with for cross-cutting concerns like security and validation (I've included a few listeners to show what I mean).

 ### The Onion architecture

 By far one of my favorite ways to build software. The first article I read about it was by Jeffrey Palermo (it can be found [here](https://jeffreypalermo.com/2008/07/the-onion-architecture-part-1/)). If you take it to the max (and of course we do) - the domain model should have no dependencies beside the BCL (Base Class Library). I find it easier to adhere to this, using NHibernate over EF.

 Alright, back on track - you'll notice that I have LedgerTag and LedgerTagDto. The reasoning is simple - I want to protect the entities as much as possible, and this means that they should -never- be sent over the wire. You know what they do to data on the internet, right? Also, if you look in the mapping files, you'll notice that the Dto's are marked as immutable, so I do not risk accidently writing data to the database from a loose Dto that someone mangled. Only entities are allowed to be persisted.

 I think that's enough for now - next up is hosting this on Azure which was the whole reason for all of this.