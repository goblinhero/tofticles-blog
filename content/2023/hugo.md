+++
title = 'Hugo'
date = 2023-11-15T14:19:20+01:00
draft = false
summary = 'Choosing a static blog generator'
+++

Choosing between static blog generators is a fairly straight-forward process. You basically choose a flavour (read: Find something that has decent traction and is well-supported). I decided to go with [Hugo](https://github.com/gohugoio/hugo) since it supports markdown syntax, has responsive design and seems fairly extensible. 

I'm probably not going to go crazy with templating and styling, so a basic theme is fine for my purpose. There is a nice and diverse selection of sort-of [supported themes](https://themes.gohugo.io/) each with it's own idea and execution. I'm basic, so went with [Anatole](https://github.com/lxndrblz/anatole/wiki) linking it as a Github sub-repo. Even with my non-existant front-end skills, I managed to get it to serve, locally - and then it was a simple matter of setting up an Azure Static Web App, linking it to the Github repo and everything was basically ready to go. One little user-error was forgetting that by default it only shows non-draft posts. 

But past that little, self-inflicted hick-up, I'm up and running. As for the result... Well, you are reading it, right now.