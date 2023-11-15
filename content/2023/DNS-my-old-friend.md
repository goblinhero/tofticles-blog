+++
title = 'DNS My Old Friend'
date = 2023-11-15T19:35:34+01:00
draft = false
summary = 'It is true - I had forgotten all about this'
+++

I am by no means a frontend developer. I thrive in my comfortable backend (sounded way better in my head, I'll admit). First thing up was buying the domain - I knew my old [employer](https://dandomain.dk) has a special on Danish domains for the first year, so that part was fairly cheap.

And then... Yeah, then what. I know, theoretically, how DNS works. And I had found a guide as to how to add what Azure calls a "Custom domain" (which part of it that is custom is a good question). So, after fumbling around (I had to add a TXT DNS entry to prove to Azure that I own the domain, and then adding A and CNAME record pointing to the IP and the randomly generated Azure-domain, respectively).

The most annoying part? My own incompetence forgetting to set the TTL to a lower number than 3600 seconds after I made a temporary change to have a subdomain pointing to the Azure site... So! A few hours later, and everything is now working. But yeah, I do not enjoy this part of developing.