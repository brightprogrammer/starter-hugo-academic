---
title: Reverse Engineering Rustlang Binaries - A Series
subtitle: This is a series of notes of my take on understanding how to reverse
  rustlang binaries.
date: 2022-12-11T17:51:03.124Z
draft: false
featured: false
image:
  filename: featured
  focal_point: Smart
  preview_only: false
---
![](michael-d-rnkqwvo80y4-unsplash.jpg)

I've been struggling with reverse engineering rustlang binaries for a while in CTF challenges. So I'm starting a reverse engineering series where I reverse engineer several rustlang binariesa and try to understand how they actually work.

1. [P﻿art One](https://drive.google.com/file/d/19fdIMiFIpVDdJN4DrG7K_PTP1Wj0poUO/view?usp=share_link) \[forgot date] : I tried to understand the basic structure of an empty rustlang binary. This helped me understand some ground rules while reversing a rustlang binary. Like where is the actual main function of rust program stored.
2. [P﻿art Two](https://drive.google.com/file/d/1DdlAhtMK45-0rxdN74ycwc09AEX02meg/view?usp=share_link) \[forgot date] : I tried to reverse a basic Hello, World! program. I'm taking slight midifications at a time!
3. [P﻿art Three](https://drive.google.com/file/d/17n-2makdIeJpuKWf48wddtZ1t-oVWcko/view?usp=share_link) [**Sun Dec 18 2022**] : Tried reversing a program that takes input from user and prints the name with a greeting message. I think I need to explore custom function calls and simple input taking first. I suspect that rust might be using other registers instead of standard linux calling convention registers.
4. [P﻿art Four](https://drive.google.com/file/d/1M-d5eTkTpFptLubv4ekW9iejUfXqX_XI/view?usp=share_link) [**Sun Dec 18 2022**] : Tried to understand how printf works and confirmed that `new_display` is actually to place the arguments of `println!` where they must be. Also gained some useful insight in how rust stores variables and how it passes them as arguments to be used.
5. [P﻿art Five](https://drive.google.com/file/d/19LS42rsCdIwWhsTKZJ6XUJ4ojRVDKxDr/view?usp=share_link) [**Mon Dec 26 2022**] : Recently me and my team (zh3r0) played a ctf where we had a rust VM challenge. I struggled a bit but was able to understand exactly how it's working. Thanks to my previous efforts of trying to understand rust binaries, this time I had enough confidence.