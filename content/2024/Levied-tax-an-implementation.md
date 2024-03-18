+++
title = 'Levied Tax an Implementation'
date = 2024-03-18T14:10:20+01:00
draft = false
summary = 'A more involved piece of functionality'
+++

The last many posts have been about infrastructure, so this time around, I needed to do some actual code for actual domain issues. The code can be found [here](https://codeberg.org/Tofticles/Anex/pulls/40).

#### What is Levied Tax?

As many companies will know, you need to keep track of taxes, that you need to administer (and pay) for the local government/administration. The most general one in Denmark is Value Addded Tax (VAT) or 'moms' in the local tongue.

Whenever you sell for a hundred Danish crowns (DKK) you need to send twenty of those to the government. Likewise, whenever you buy for a hundred DKK for an expense that is eligible, you get the twenty DKK back.

#### Seems like a lot of code for a simple concept

Well, yes. But the problem is that it is not really a simple concept. The code here is only for registrering the amounts - later will come reporting of the amounts to the government and registrering the payment, creating settlements and statements showing you have calculated it correctly. And this is C#, so verbosity is the norm (at least it isn't Java :P).

#### Supported scenarios

VAT in Denmark is generally 25%. However, historically it has changed and probably will again. This is why the applied percentage is put into a spearate table/type with support for taking effect at a specific time. Basically, you don't want to end up in a situation where you cannot register new entries until you have everything registrered up to the date before the new percentage takes effect. Everything in bookkeeping can be both in the past, in the future and right now, so support for being able to do everything, everywhere all at once (I should really record that for a movie title, some time) is paramount. Also, generally you will want to be able to specify it for outgoing and ingoing VAT to make it easier to make the statement later. And if you are unlucky enough to be in Norway, you'll want to be able to have several different types since the VAT for selling ice cream depends on whether it is consumed in the shop or brought outside... (No really, that is perfectly normal.. in Norway).

#### Other types of levied tax

There are a lot of different taxes in Denmark at least - things like sugar tax, vehicle registration tax and the list goes on. This is the reasoning behind not simply calling the classes VAT and VAT Percentage.

This is by no means the last code I'll have to do on VAT - this is just the first building block.