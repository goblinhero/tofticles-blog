+++
title = 'Kubernetes Choices'
date = 2024-03-07T12:28:07+01:00
draft = false
summary = 'Oh no.. flashbacks to too many choices..'
+++

Alright, so [Kubernetes](https://kubernetes.io/) (K8S) is.. complicated. Lots of moving parts, different utilities, CLIs etc. etc. I am not too heavily invested in deep-diving into all the specifics and kinda (for now, at least) just want to get something up and running. Choosing between a myriad of options for storing the state of the cluster.. Not so much.

Enter [K3S](k3s.io) which is kinda what Ubuntu is for Linux. A lot of opinionated choices to have you worrying about as little as possible getting this beast up and running. You still have the option to do all the things you can do with the full K8S setup, but you have very sensible defaults that will get you 80-90% of the way. 

I find the analogy with Ubuntu pretty neat, because you still need to understand the basics of what K8S is and the fundemental koncepts to understand what K3S does - like you still need to know what a command-line is and how to operate it to have 'fun' in Ubuntu. In contrast with Ubuntu (and where the analogy kinda breaks down) is that K3S is the lightweight distro of K8S - or more consisely, it is more like a barebone version (but still fully compliant with the specs).

To understand the basics of K8S, I highly recommend [TechWorld with Nana](https://www.youtube.com/watch?v=s_o8dwzRlu4). She has a ton of videos, but this one I found short and still comprehensive enough to understand the basic concepts. (She was recommended to me by a former colleague who has extensive DevOps experience).

Getting K3S installed on my nodes... I'll save that for another post. I have technically not done so, yet.