+++
title = 'Switching away from Github'
date = 2024-03-14T09:43:36+01:00
draft = false
summary = 'Putting morals to the test'
+++

After my delving into using Github for container registry, it kinda dawned on me that maybe now is the time to finally take the plunge and switch to fully open-sourced code repository hosting. It will cause a few issues and render a bit of work irrelevant, but hey-ho. Sometimes, it's worthwhile to have your actions reflect your values.

## The Github repository stays
I'm not a fan of erasing history - and as long as Github keeps it's free hosting, my old Github projects will stay where they are. (Also, I'm lazy, and fixing all the links in previous blog posts will be another day). The blog will stay on Github for the foreseeable future. 

## Choosing a new place to host the code
Priorities:
* Has to be hosted in Europe 
* Has to be based on open-source
* No vendor-lockin

#### Why Europe?
Because freedom. Freedom from American tech giants. Freedom from America's questionable practises from a legal perspective (Read: Patriot Act etc.).

#### The chosen one
After doing a bit o research, I've stumbled across [Codeberg](https://codeberg.org/). There is a general feel of transparency around this. It is hosted in Germany, known for taking civil rights seriously including GDPR.

For one, they make a point of how to switch away from their services - which is a sign of confidence in their own product which I like. The interface is fairly straight-forward (Github has changed a -lot- over the last years leading to a lot of functionality that is not relevant for me and just serves to clutter up the interface) and seems to cover what I need (I have not yet figured out how to set up a container registry, but I might have a different solution to this, that is actually more to my liking).

#### Migrating the code
I chose to use their migration tool to siphon the code from Github using a personal access token. I now have the full commit history - and as for fresh starts, I removed the old branches and PR's (they are still working for the old posts since they point to Github).

## Conclusion
So, I am now [Tofticles](https://codeberg.org/Tofticles) on Codeberg!