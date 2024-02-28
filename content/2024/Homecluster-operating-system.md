+++
title = 'Homecluster Operating System'
date = 2024-02-28T09:09:24+01:00
draft = false
summary = 'Ubuntu rookie mistakes'
+++

I've dabbled in Linux before, but it is maybe 8-10 years ago and things have changed. So, I consider myself a rookie and will go for middle-of-the-road solutions to what I need. So, not highly optimized, barebones nor highly secure setup is the aim here. I want to get stuff up and running and thus, I aim for solutions/frameworks with a lot of support, ease-of-use and an active community.

Natural choice is then currently Ubuntu 22.04 LTS and I went with their [guide here](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview). I had an old Sandisk USB mini SSD at around 17 GB which worked for the task of making the bootable image. With the Thinkcentre cluster nodes, I simply had to have it boot from the image to replace the Windows installation. It went without hickups of any kind (except the Settings widget kept crashing - I suspect because I was not connected to the internet during install).

The general idea is that the nodes should only be connected to the (cheap, crap - pick your adjective) ISP-supplied router via cabled ethernet and to power - so, headless.

## Rookie mistake #1 RDP

So, as mentioned, I wasn't connected to the internet during install and since the Settings widget kept crashing, I had to connect the nodes one at a time to the internet to get RDP set up. I am not your typical nerd with a drawer full of old keyboards and monitors.. So, I had to physically move my screen and keyboard into the living room next to the router which also meant that I could not at the same time use my desktop for testing. (Red flags should be raised at this point along with blaring sirens).

Anyway, once connected, I did the usual updates with `apt-get` command and checked the checkboxes (that started working once it had network access) for Remote control (RDP). Repeat for the other two nodes and power them on sitting right beside the router. Moved the monitor and periphials back to the desktop.. And I could not connect to the nodes.

Using the router, I could find their IP-addresses and I could ping them, but no RDP connection with `mstsc` on Windows. Hmm.. This was not expected. Maybe a problem with the Windows RDP-client. Next step was activating the WSL and getting Ubuntu, installing Remmina (`sudo apt-get install remmina`) and spinning this up. Connection refused. Hmm!

Much searching the interwebs for the why and from a comment on a shady forum to an unrelated question (yes, the internet is dying, search-wise), it dawned on me. RDP allows you to connect to a user's -live- session on Ubuntu and not like the Windows approach of 'Ain't anyone in the office, but you go on ahead and connect to this machine, Johnny'. Very much a feature, not a bug.

## Rookie mistake #2 SSH

Okay, so back to basics. SSH'ing into the nodes (which was my long-term plan, anyway, since I just need a terminal to service the Kubernetes install, later). Since I had already actived the WSL on my Windows box, I tried to SSH into one of the nodes. Connection refused. Doh. I had not installed the SSH server on the nodes... Well, back to moving hardware back into the living room, connect to the nodes one at a time, install openssh server (`sudo apt-get install openssh-server`) - this time checking that it was actually running. Then moving the hardware back to the Windows box and finally.. 

## Great success

!['Home cluster in all it's glory'](/images/2024/heureka.png)

So, time-usage for getting it up and running:

* 0:45 - Getting the Ubuntu image and making a bootable image with ['Balena'](https://etcher.balena.io/) - including rummaging for the USB SSD
* 4:25 - Waiting for UPS to arrive (they had a time window of 11:50-13:50 and arrived at 16:15...) 
* 0:30 - Unpacking and installing Ubuntu on the nodes
* 1:30 - Fiddling with updates, RDP/SSH including moving hardware around

An afternoon well spent, I feel. Next up is installing [K8s](https://kubernetes.io/). 