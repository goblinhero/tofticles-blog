+++
title = 'To REST or Not to REST'
date = 2023-11-23T10:07:39+01:00
draft = false
summary = 'And people do not understand how wars start...'
+++

First rule of Representational State Transfer - noone agrees what it -is- - everyone has their own definition and opinions. One of the problem areas concern how to use uri's:

According to [Microsoft](https://learn.microsoft.com/en-us/azure/architecture/best-practices/api-design#organize-the-api-design-around-resources) - it is simple:

    https://adventure-works.com/orders // Good
    https://adventure-works.com/create-order // Avoid

Okay, would have helped if they wrote that we are talking about HTTP:POST (ie creating orders), but sure - DRY (Don't Repeat Yourself) is usually a good practise - that would lead to:

    HTTP:GET    Reading list stuff (don't change anything, GDI!)
    HTTP:POST   Creating stuff (new stuff)
    HTTP:PUT    Update stuff (read: replace in it's entirety)
    HTTP:PATCH  Update stuff (but be lazy about it - no sense listing all the things that hasn't changed)
    HTTP:DELETE Delete stuff

And I tend to agree. This is a good starting point. But right now, if we only have five different ways to do things with a given resource, what if we want two different ways of creating this stuff?

    HTTP:POST   /copy_from/{id}
    HTTP:POST   /

Or even worse - we have a thing and we can do things to it (with side-effects):

    HTTP:PATCH  orders/{id} (the general one)
    HTTP:PATCH  orders/{id}/deliver
    HTTP:PATCH  orders/{id}/cancel

Are we breaking the rules? Should we be ashamed? I'd argue.. 'No'. We are being expressive about what is possible to do and what the consequences are. The POST example is clear that we are creating a new thing (HTTP:POST) and that we are copying the state of another of the same thing with .Id being {id}. With the PATCH example - we are being public about what can be done with the Order. We are telling the user of our API, that we are not deleting anything (PATCH) with the /cancel endpoint, but the state will change.

The alternative would be to basically have a switch-case in our endpoints and littering our DTOs with properties for the action that can be taken. We'd have made it more obscure (what happens if I set both deliver:true and cancel:true in the body?) and have inflicted physical pain on the developer having to a) code this and b) maintain it. 