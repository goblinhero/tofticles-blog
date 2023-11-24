+++
title = 'Continious Refactoring'
date = 2023-11-24T08:46:08+01:00
draft = false
summary = 'I am sure it will catch on and be the next industry standard'
+++

The code for this post can be found [here](https://github.com/goblinhero/Anex/pull/28).

If you've been following along and read some of the pull requests, you may have noticed that I refactor judiciously. Not everyone agrees with this approach, but in my mind it follows the Boyscout principle: "Leave the code better than when you checked it out". Over time, it leads to a couple of things:

### A thousand small pains leaves you stronger

Over time, you'll build up a way of doing refactorings safely. In this early part of the lifetime of the code, it is safer to do refactorings, change your mind, find better approaches etc. etc. And you get a 'feel' for the code-base - as in, you develop your own best practises, preferences and will have an easier time arguing for your choices since you most likely will have either felt the pain of doing it in another way or at least seen the drawbacks.

### The three steps of efficient coding

In the eternal words of a previous [boss](http://twinoak.dk/), "First make it work, then make it pretty and finally, make it fast". Much of our work is creating something 'new', as in we haven't tried it before. Someone else might have done something similar, but likely not exactly the same. So we steal their code from [Stack Overflow](https://stackoverflow.com/) - we get a shaky 'solution' up and running that on a sunny day, in perfect wind conditions, sort of does what we want. Then we start adding a foundation, extract into classes, generalize etc. etc. Once that is done - we start looking at optimization where necessary, often compromizing on the beauty - like denormalizing database tables, doing raw sql and so on.

### More maintainable code

Since we take the small pain of refactoring things when we notice them, we avoid having the technical debt get to the level of "Might be worthwhile to do a re-write". If we continiously keep the libraries up-to-date, update to newer versions of .NET - it is much easier to notice what broke the build.

### How to avoid breaking things

Once we are at a more stable stage in development, it becomes increasingly important to not have things break - and there is only one answer to this problem. Tests. And if that fails, more tests.