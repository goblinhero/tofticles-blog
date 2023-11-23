+++
title = 'Explicit Trumps Implicit'
date = 2023-11-23T12:08:04+01:00
draft = false
summary = 'State your purpose, mortal!'
+++

The code can be found [here](https://github.com/goblinhero/Anex/pull/26).

Things are easier now than ever for documenting your API properly. While I am not generally a fan of attributes, they do good service in Dto's and `ProducesResponse` decorations of your controllers. It makes it a lot easier for third party developers to use your API, when you are clear and concise in your communication. In olden days, we wrote comments all over the place - which over time typically became out-of-date or downright misleading when a hotfix wasn't documented and so on.

The more you can avoid a developer scratching their head, the happier they are. As one of my former colleagues used to say: "Write your code like a psycho is the one to maintain it and they have your home address".

By using these attributes, you also make it possible to auto-generate clients for your code instead of you having to maintain them yourself.