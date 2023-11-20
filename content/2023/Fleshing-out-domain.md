+++
title = 'Fleshing Out Domain'
date = 2023-11-20T13:25:50+01:00
draft = false
summary = 'Bookkeeping is fun!'
+++

The state of the code for this post can be found [here](https://github.com/goblinhero/Anex/pull/9)

So, having things up and sort of running - I need a few more things, before we can finish setting up the infrastructure on Azure. 

### Domain

Time to spill the beans - what is going on here: I'm a former accountant which means you get to learn a bit about bookkeeping and ERP-systems. The background for our current bookkeeping standards are based on principles invented by monks back in the 1600s, so not exactly modern stuff. However, the ideas are robust and make for some relatively simple logic if what we are doing is correct. 

Accountants work a lot with audit trails and traceability. Basically, whichever number you come up with in a report concerning your fiscal statements has to be backed by fact and there should be a straight line between the fact and what is presented. This is why I've implemented the `DisallowDeleteTransactionListener` and `InvalidTransactionalUpdateException`, so that even if I mess up in the code, I do not accidently alter or delete already posted transactional data. 

The classes, I've implemented in this part are the basic building blocks - basically, we have drafts that will turn into final posts and transactions. As an aside, the name 'Anex' is [Xena](https://xena.biz) spelled backwards, which was the ERP-system I built on for around 9 years from 2010 - many ideas here are inspired by that work (and the hard-earned lessons back then).

### Tests

Any good project needs a test suite. I've added some very basic tests here - I've opted for xUnit and NSubstitute for frameworks, but I have no strong opinions on the matter (I know there is quite a bit of controversy around Moq at the moment). Nothing really exciting going on here, but at least now we have some tests to put in the pipe line.

I know the code will probably look fairly complex for what it does, but this is about as complicated as it gets. From now on it is adding more rules and more classes around the basic concepts. I've eaten a lot of up-front costs to have this working, so that I can concentrate on more important matters later. 

Next post will revolve around setting up tests in the pipeline and making sure the infrastructure works and can be deployed safely.