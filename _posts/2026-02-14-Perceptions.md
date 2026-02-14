---
title: Perceptions | 0xFun CTF 2026
description: a writeup for "Shell" in 0xFun CTF 2026.
date: 2026-02-14 21-00-00 +0200
categories: [0xFun CTF 2026, WarmUp]
tags: [Web]  
image: assets/img/logos/0xFun.png
permalink: /posts/0xfun/Perceptions
---

![](/assets/img/logos/0xFun.png){: w="400" h="400" }

## Overview
- **Platform:** [0xFun](https://ctf.0xfun.org/)
- **Challenge:** [Perceptions](https://ctf.0xfun.org/challenges#Perceptions-60)
- **Categories:** Web
- **Rating:** 9/10
- **Difficulty:** Easy
- **Sovling Time:** ~20 Minutes

## Challenge
> "*Take a look at the blog I created! It has a neat backend and, interestingly, seems to use fewer ports.*"

## Walkthrough
### Recon

The website looks very noraml, just we have a login page, later on that later.

Something looks fishy in ```Secret Post```, by viewing the page source we can see some credentials:
![](/assets/img/posts/2026-02-14-Perceptions/creds.png) 

### Solution Approach
> "*Flag format: ```0xfun{}```*"

From recon we can try credentials we found in the login page

> Username is ```Charlie```, you can see this at top of the site.
{: .prompt-info }

![](/assets/img/posts/2026-02-14-Perceptions/site.png) 

Nothing has changes..... Wait they mentioned in a blog something about many things running on many ports, lets try to SSH into the server with the provided credentials.
```bash
ssh -p 48385 Charlie@chall.0xfun.org
```
![](/assets/img/posts/2026-02-14-Perceptions/shell.png) 

And it worked!! Lets find the flag.

![](/assets/img/posts/2026-02-14-Perceptions/flag.png) 


#### Flag
```0xfun{p3rsp3c71v3.15.k3y}```


***Hope you enjoined the writeup, Don't forget to leave a comment or a star on the [github repo](https://github.com/Axl0t0l) (;***