---
title: Digital Footprint | TryHackMe
date: 2026-02-10 22-00-00 +0200
categories: [TryHackMe, Challenges]
tags: [OSINT]     
---

![](/assets/img/logos/THM-logo.png){: w="400" h="400" }

## Overview
- **Platform:** [TryHackMe](https://tryhackme.com/)
- **Challenge:** [Digital Footprint](https://tryhackme.com/room/osintchallengeiv)
- **Categories:** OSINT
- **Rating:** 
- **Difficulty:** Easy
- **Sovling Time:** 10~15 Minutes

## Challenge
> "*An ACME Jet Solutions employee uploaded a photo of a residential property believed to be linked to ACME Jet's early operations. Can you figure out where the picture was taken to confirm or debunk the rumour?*"

## Walkthrough
### Solution Approach
#### First Question <a name="first-question"></a>
> "*In which city was the photo taken?*"
![Credits: TryHackMe](/assets/img/posts/2026-02-10-DigitalFootprint/edited-house-1763031553617.jpg)


Hmmm, lets use ```exifftool``` on the image to see if there is any metadata.
```bash
exiftool edited-house-1763031553617.jpg
```
and there we go, there were metadata!
![](/assets/img/posts/2026-02-10-DigitalFootprint/exiftool.png)


if we use google maps for ```GPS Latitude: 26 deg 12' 14.76", GPS Longitude: 28 deg 2' 50.28"``` we get 
![](/assets/img/posts/2026-02-10-DigitalFootprint/city.png)

##### Flag
```THM{Johannesburg}```

***

#### Second Question 
> "*ACME Jet Solutions (warc-acme.com/jef/), is all over social meda claiming they were founded in 2025 and that they're the fastest-growing data company in Africa.
But something doesn't add up, one of their ex-employees ensures you that the company existed long before that.

Your job as an OSINT investigator is to verify their founding date using only public information.

Flag Format: THM{YYYYMMDDHHMMSS}*"


Lets try to look for WHOIS DNS records at [WHOIS](https://who.is/)

![](/assets/img/posts/2026-02-10-DigitalFootprint/whois.png)

and its empty. what about checking google maps for the coordinates we got from [First Question](#first-question)

##### Flag
```FLAG{}```

***

#### Second Question 
> "*QUESTION-REQUIRMENT-HERE*"


{SECOND-QUESTION-EXPLNATION-HERE}

##### Flag
```FLAG{}```

***

#### Second Question 
> "*QUESTION-REQUIRMENT-HERE*"


{SECOND-QUESTION-EXPLNATION-HERE}

##### Flag
```FLAG{}```


***Hope you enjoined the writeup, Don't forget to leave a comment or a star on the [github repo](https://github.com/Axl0t0l) (;***
