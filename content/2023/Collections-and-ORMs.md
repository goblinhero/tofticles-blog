+++
title = 'Collections and ORMs'
date = 2023-11-27T12:31:04+01:00
draft = false
summary = 'Collections are bad, mkay..'
+++

This is a bit of an aside, but there is still a bit of code [here](https://github.com/goblinhero/Anex/pull/30).

The naive approach to solving the mapping between the relational structure and the object ditto is to have bidirectional mappings, where the child has a property for the parent and the parent has a colleciton of childs.

### The challenges with bi-directional

Let's say you have an order with order lines. If you have a bi-directional relationship, you run the risk of a child pointing at the wrong parent - or several parents having the same child in their respective collections. It is not unsolvable, but it is something that needs constant energy to ensure since the object model does not care, but the relational one does. 

### Problematic collections in general

So, let's say you have the same order with order lines. You've cleverly made the order line a component, so it does not have an `.Id`, but rather an `.OrderId`. Basically, the order is in charge of saving, updating and deleting orderlines. Now, everytime you need to touch one of the orderlines, you need to first load the order, get all the orderlines, do your changes and save the changes back to the database to ensure it is still in a valid state.

Also, if you have the same structure in your API, you risk a 3rd party developer deleting the orderlines everytime they update the order head properties, because they mistakenly sent an empty list along with the order. Also, paging is a pain in the bottocks - basically, you need to a) load the entire collection and then do paging to the client or b) do custom shenanigans where you allow it on reads, but not writes.

### An alternative

So, what do we do - we need to have parent/child objects - there is no way around it. Well, when it is at all possible, I go with the child knowing about the parent but not the other way around. So, the order line has an `.OrderId`, but the order only operates on orderlines with statechanges, so a typical method would look like this:

    public void Deliver(ICollection<OrderLine> orderLines)
    {
        //Handle the state-change
    }

Funnily, this makes it a lot easier to support things like partial delivery (you simply only supply the lines that need to be delivered now - and invoke the method with a secondary set, later). This is not without cost, though. Things like ordering can become a pain (as in, if the order of the orderlines is important, you do not have a centralized place to enforce this).

### Don't you have -any- collection?

Well, as always, there are caveats - for things like `EconomicTransaction` in Anex, it actually has a collection of `LedgerPost`, because it's validity is dependant on the state of the LedgerPosts. Roughly speaking, things that are immutable (business-wise) - write once, read often tend to be with collections - and otherwise, the child knows about the parent. But the most important is not having bidirectional relationships if at all possible - it is a huge complexity cost and you gain very little for it.