+++
title = 'Azure - More ranting'
date = 2023-12-22T13:22:38+01:00
draft = false
summary = 'Yeah, it is just about the holidays, so best get it out of my system...'
+++

So, my test subscription ran out. And Azure is a pain in the buttocks to work with when that happens. Ideally, I wanted to just scrap whatever I had and start fresh with a paid subscription.

Work had happened, so a couple of weeks had passed after the end of the old subscription - and it turns out, you cannot delete things on a subscription that is suspended. First Wat achieved. It will, however, be automatically deleted at some unknown point in the future when Microsoft gets around to it. Second Wat achieved.

Since today is the day I wanted this to work (this blog is supposed to be free to run as Azure has free Static Web Apps - yeah, sorry about the outage, people), I had no option, but to add my credit card to un-suspend the subscription, remove the old crap and then recreate the blog.

Oh, and of course there's a bill waiting to happen, because although it was a free account, my suspended applications were still running after the subscription ended... Third Wat achieved. It's nothing huge, maybe 5€ or so, but it is enough to make me grumble and write up this rant.

Oh, and don't get me started on how sluggish things are on the Azure Portal. I create a new subscription, I get an alert that it is created. I cannot create resources on it. I can jump to the subscription, but I cannot create resources on it (it is not present in the dropdown). Fourth Wat achieved. I log out, log back in, now I can see all the deleted things, but not the new subscription.

I give it ten minutes, then log out, log back in, and finally I can see my new shiny subscription. Then remove the old Github Action, create a new Static Web App, point it to the repo, fiddle with the DNS and we're back online.

But hey, I'm off on vacation in half an hour, so it isn't all bad. Happy holidays, everyone!