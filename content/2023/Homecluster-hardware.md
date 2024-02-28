+++
title = 'Homecluster Hardware'
date = 2024-02-28T07:59:52+01:00
draft = false
summary = 'Choices, choices...'
+++

I've been talking with a few former colleague about what to do for hardware for a cheap home cluster. I could have gone with the cheap option and used the virtual home cluster all running on my own machine. But that's boring and too far from what I want to know about to really fit my needs. I could have gone with one of the new AMD Ryzen NUCs and virtualized a few nodes, but still.. feels wrong. I -want- to have the problems of networking between physical machines.

So, I'm old-school and a scavenger - deal with it. Buying three new small machines... Well, we only have the one planet, and it seems a bit of a waste, cash-wise. Enter [Refurbed](https://www.refurbed.dk/") for refurbished, decommisioned office machines. I was lucky to find three Lenovo Thinkcentre 910q tiny in excellent condition for a total of 3.866,43 DKK including shipping (roughly 520€/560$):

* i5-6500T 2.5 GHz 4 core CPU (the T is important as it means it is optimized for low power usage - max usage is rated at 35W where the non-T model is rated at 65W)
* 16 GB RAM (DDR4 2400 MHz - upgradable to 32 GB if need be)
* 256 GB SSD

I realize that I could have gotten 3 new AMD Ryzen 3/Intel N100 mini-PCs with (overall) better specs and lower power consumption at roughly the same price. But here's the thing - build-quality is not even remotely in the same league. Enterprise models are built to be serviceable, durable and all-around no-hassle. Just for comparison look at how heavy these machines are (1.32 kg each vs. around 810 g for the new machines) - the chassis is metal. Clearly meant for large enterprises that don't want to shift hardware too often for their office workers or waste time servicing them physically. And just look at the beauty:

![The home cluster](/images/2024/cluster.jpg)

Next up, installing a host OS [Ubuntu](https://ubuntu.com) on the three.