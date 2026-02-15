---
title: The Slot Whisperer | 0xFun CTF 2026
description: a writeup for "The Slot Whisperer" in 0xFun CTF 2026.
date: 2026-02-14 21-00-00 +0200
categories: [0xFun CTF 2026, Crypto]
tags: [Crypto]  
image: assets/img/logos/0xFun.png   
permalink: /posts/0xfun/TheSlotWhisperer
---

![](/assets/img/logos/axl0t0l.png){: w="400" h="400" }

## Overview
- **Platform:** [0xFun](https://ctf.0xfun.org/)
- **Challenge:** [The Slot Whisperer](https://ctf.0xfun.org/challenges#The%20Slot%20Whisperer-148)
- **Categories:** Crypto
- **Rating:** 9/10
- **Difficulty:** Easy
- **Sovling Time:** ~15 Minutes

## Challenge
> "*The oldest slot machine in the casino runs on predictable gears. Watch it spin, learn its rhythm, and predict what comes next.*"

## Walkthrough
### Recon

It looks like normal *flawed* random number generator

#### The Machine Script
```py
#!/usr/bin/env python3

class SlotMachineLCG:
    def __init__(self, seed=None):
        self.M = 2147483647
        self.A = 48271
        self.C = 12345
        self.state = seed if seed is not None else 1

    def next(self):
        self.state = (self.A * self.state + self.C) % self.M
        return self.state

    def spin(self):
        return self.next() % 100


if __name__ == "__main__":
    print("Slot Machine LCG Test")
    print("=" * 40)

    lcg = SlotMachineLCG(seed=12345)

    print("First 10 spins:")
    for i in range(10):
        spin = lcg.spin()
        print(f"Spin {i+1:2d}: {spin:2d}")
```

### Solution Approach
> "*Flag format: ```0xfun{}```*"

The parameters provided are:
* M = 2,147,483,647 (A Mersenne Prime, 2^31-1)
* A = 48,271
* C = 12,345
The machine doesn't show us the full internal state Sn it only shows us ```spin = S_n % 100```.
The Vulnerability LCGs are entirely deterministic. If you know the internal state S_n at any point, you can calculate all future "random" numbers.
While we are only given the last two digits of the state (the remainder when divided by 100), the modulus M is small enough for a brute-force attack.
#### Solving Script
```py
#!/usr/bin/env python3

# LCG Parameters provided in the challenge
M = 2147483647
A = 48271
C = 12345

# The sequence observed from the nc connection
observed = [29, 10, 42, 89, 33, 73, 10, 92, 19, 73]

def get_next_state(state):
    return (A * state + C) % M

def solve():
    print(f"[*] Brute-forcing state for sequence: {observed}")
    
    # The first spin is 39. Therefore, state_0 = 100 * k + 39
    # We iterate through all possible values of k
    for k in range(M // 100 + 1):
        state = 100 * k + observed[0]
        if state >= M:
            break
        
        current_state = state
        match = True
        
        # Verify if this candidate state produces the rest of the sequence
        for i in range(1, len(observed)):
            current_state = get_next_state(current_state)
            if current_state % 100 != observed[i]:
                match = False
                break
        
        if match:
            print(f"[+] Found internal state: {state}")
            # Calculate the next 5 spins
            predictions = []
            temp_state = current_state # state after the 10th spin
            for _ in range(5):
                temp_state = get_next_state(temp_state)
                predictions.append(temp_state % 100)
            return predictions

    return None

if __name__ == "__main__":
    result = solve()
    if result:
        print("[!] Next 5 spins (predict these):")
        print(" ".join(map(str, result)))
    else:
        print("[-] State not found. Double check your input sequence.")
```
and it worked!!
![](/assets/img/posts/2026-02-14-The-Slot-Whisperer/script.png)
![](/assets/img/posts/2026-02-14-The-Slot-Whisperer/flag.png)

#### Flag
```0xfun{sl0t_wh1sp3r3r_lcg_cr4ck3d}```


***Hope you enjoyed the writeup, Don't forget to leave a comment or a star on the [github repo](https://github.com/Axl0t0l) (;***