---
title: Digital Footprint | TryHackMe
date: 2026-02-10 22-00-00 +0200
categories: [TryHackMe, Challenges]
tags: [OSINT]     
---

![ ](/assets/img/logos/THM-logo.png){: w="400" h="400" }

## Overview
- **Platform:** [TryHackMe](https://tryhackme.com/)
- **Challenge:** [Digital Footprint](https://tryhackme.com/room/osintchallengeiv)
- **Categories:** OSINT
- **Rating:** 9/10
- **Difficulty:** Easy
- **Sovling Time:** 20~25 Minutes

## Challenge
> "*An ACME Jet Solutions employee uploaded a photo of a residential property believed to be linked to ACME Jet's early operations. Can you figure out where the picture was taken to confirm or debunk the rumour?*"

## Walkthrough
### Solution Approach
#### First Question
> "*In which city was the photo taken?*"
![Credits: TryHackMe](/assets/img/posts/2026-02-10-DigitalFootprint/edited-house-1763031553617.jpg)


Hmmm, lets use ```exifftool``` on the image to see if there is any metadata.
```bash
exiftool edited-house-1763031553617.jpg
```
and there we go, there were metadata!
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/exiftool.png)


if we use google for ```GPS Latitude: 26 deg 12' 14.76", GPS Longitude: 28 deg 2' 50.28"``` we get:
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/city.png)

##### Flag
```THM{Johannesburg}```

***

#### Second Question 
> "*ACME Jet Solutions (warc-acme.com/jef/), is all over social meda claiming they were founded in 2025 and that they're the fastest-growing data company in Africa.
But something doesn't add up, one of their ex-employees ensures you that the company existed long before that.

Your job as an OSINT investigator is to verify their founding date using only public information.

Flag Format: THM{YYYYMMDDHHMMSS}*"


Lets try to look for WHOIS DNS records at [WHOIS](https://who.is/)

![ ](/assets/img/posts/2026-02-10-DigitalFootprint/whois.png)

and its empty. what about checking [Wayback Machine for the file?](https://archive.org/details/warc-acme.com-jef)
and we got it!!
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/waybackmachine.png)
##### Flag
```THM{20160210224602}```

***

#### Third Question 
> "*Further Investigation uncovers another image believed to be connected to the company's international expansion.

Research reveals that to the right of the iconic landmark is a building that played a big role in the fight for independence of a particular country. Signs on the external wall provides the name of the building. 

Submit the name of building translated into English as the flag.

The flag format is THM{Landmark}*"
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/landmark-1763035881792.JPG)


We simply reverse search the image on google, and we get:
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/thespire.png)

we can see office post there, it fits for a perfect landmark.

##### Flag
```THM{General Post Office}```

***

#### Fourth Question 
> "*After uncovering ACME Jet Solutions origins and tracing their online presence through archived websites and international landmarks, investigators believe that an internal document was accidentally leaked by one of the company's developers. 

The document may contain crucial information about the individual responsible for maintaining their systems.*"


We have a compressed file here lets unzip it, and we got this:
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/fileexplorer.png)

```meta.xml``` looks interisting, lets open it. And we got this first clue!:
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/markwilliams7243.png)

after digging through his socials, I found the flag on his YouTube!!
![ ](/assets/img/posts/2026-02-10-DigitalFootprint/youtube.png)

##### Flag
```THM{Y0u_f0und_7h3_fin4l_fl4g!}```


***Hope you enjoined the writeup, Don't forget to leave a comment or a star on the [github repo](https://github.com/Axl0t0l) (;***
