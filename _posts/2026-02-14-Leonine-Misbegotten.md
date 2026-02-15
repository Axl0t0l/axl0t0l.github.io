---
title: Leonine Misbegotten | 0xFun CTF 2026
description: a writeup for "Leonine Misbegotten" challenge in 0xFun CTF 2026.
date: 2026-02-14 21-00-00 +0200
categories: [0xFun CTF 2026, WarmUp]
tags: [Crypto]  
image: assets/img/logos/0xFun.png
permalink: /posts/0xfun/LeonineMisbegotten
---

![](/assets/img/logos/axl0t0l.png){: w="400" h="400" }

## Overview
- **Platform:** [0xFun](https://ctf.0xfun.org/)
- **Challenge:** [Leonine Misbegotten](https://ctf.0xfun.org/challenges#Leonine%20Misbegotten-64)
- **Categories:** Crypto
- **Rating:** 9/10
- **Difficulty:** Easy
- **Solving Time:** ~20 Minutes

## Challenge
> "*At the end of Castle Morne awaits a fearsome lion with a goofy tail. His attacks come fast, feel random, yet follow a careful rhythm.*"

## Walkthrough
### Recon

If we look at this code it doesn't look really complicated just simple encoding logic with checksums.

#### The Code
```py
from base64 import b16encode, b32encode, b64encode, b85encode
from hashlib import sha1
from random import choice
from secret import flag

SCHEMES = [b16encode, b32encode, b64encode, b85encode]

ROUNDS = 16
current = flag.encode()
for _ in range(ROUNDS):
    checksum = sha1(current).digest() # this is to help you check the integrity of your decryption
    current = choice(SCHEMES)(current) 
    current += checksum

with open("output", "wb") as f:
    f.write(current)
```
#### Code Output
![Get files here!!](/assets/img/posts/2026-02-14-Leonine-Misbegotten/output)

### Solution Approach
> "*Flag format: ```0xfum{}```*"

#### Encoding Script
**The Setup:**

It starts with the flag.
It defines four possible encoding schemes: Base16, Base32, Base64, and Base85.
The Loop (The "Wrapping" Phase):
The script runs 16 times (ROUNDS = 16). In each round:

* Checksum: It calculates a sha1 hash of the current data. This hash is always 20 bytes long.
* Encoding: It randomly picks one of the four schemes and encodes the data.
* Appending: It tacks the 20-byte checksum onto the end of the newly encoded string.

The Output:
After 16 rounds of this, the final, heavily encoded string is saved to a file named output.

#### Decoding Script Logic
To get the flag back, you have to work backward from the output file. Since each round adds the checksum after encoding, the logic for one decryption step looks like this:

* Separate: Take the last 20 bytes off the data—that's your checksum.
* Identify & Decode: Try decoding the remaining string using all four methods (Base16, 32, 64, 85).
* Verify: For each successful decode, calculate the SHA1 hash of the result. If it matches the checksum you sliced off in step 1, you know you found the correct scheme for that layer.
* Repeat: Do this 16 times until you hit the original flag string.

> You can ask any AI to write you the script.
{: .prompt-info }

#### Decoding Script
```py
import base64
import hashlib

# Load the encoded data from the output file
with open("output", "rb") as f:
    current = f.read()

# The four schemes used in the original script
SCHEMES = [
    base64.b16decode,
    base64.b32decode,
    base64.b64decode,
    base64.b85decode
]

# Work backwards through the 16 rounds
for i in range(16):
    # The checksum is the last 20 bytes
    checksum = current[-20:]
    # The encoded data is everything before the checksum
    encoded_data = current[:-20]
    
    found = False
    for decode_func in SCHEMES:
        try:
            # Attempt to decode
            decoded = decode_func(encoded_data)
            # Verify the integrity using the SHA1 checksum
            if hashlib.sha1(decoded).digest() == checksum:
                current = decoded
                found = True
                print(f"Round {16-i} decrypted successfully.")
                break
        except Exception:
            continue
            
    if not found:
        print(f"Failed to decrypt at round {16-i}")
        break

print(f"\nFlag: {current.decode()}")
```
and there we got the flag!!
![](/assets/img/posts/2026-02-14-Leonine-Misbegotten/flag.png)

#### Flag
```0xfun{p33l1ng_l4y3rs_l1k3_an_0n10n}```


***Hope you enjoined the writeup, Don't forget to leave a comment or a star on the [github repo](https://github.com/Axl0t0l) (;***