+++
title = 'Detour - Getting RDP to Work on Ubuntu'
date = 2024-02-29T16:02:40+01:00
draft = false
summary = 'Always the last thing you try, that works...'
+++

So, as I wrote previously, I failed to get RDP to work for the cluster nodes. Being somewhat stubborn, I did some digging and on one of the Ubuntu forums, I found one that had a solution that coupled with a bit of tinkering from another answer got me all I wanted. (For the full explanation and a bit of background, look [here](https://askubuntu.com/questions/1407444/ubuntu-22-04-remote-desktop-headless).)

## Enter XRDP

XRDP is an alternative to the baked-in RDP in Ubuntu that I have been unable to use as my nodes run headless. XRDP allows for connections using both the Windows native mstsc RDP client and Remmina and it does not require a live session nor having a separate username/pass combo.

## Installing

Simply run:

```Bash
sudo apt install xrdp
```

Followed by:

```Bash
sudo systemctl status xrdpex
```

To check that it is running.

## Caveats 

If you do nothing, it will use the old Gnome desktop, to avoid this, create a file called `.xsessionrc` in your home directory with the following contents:

```Bash
export GNOME_SHELL_SESSION_MODE=ubuntu
export XDG_CURRENT_DESKTOP=ubuntu:GNOME
export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
```

## Conclusion

And that was all there was to it. I now have the desktop I wanted using the Windows RDP client which makes some tasks around running virtual machines quite a bit easier for a rookie Linux user.