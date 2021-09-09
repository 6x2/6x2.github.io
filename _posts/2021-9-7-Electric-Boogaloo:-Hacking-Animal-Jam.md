---
layout: post
title: "Electric Boogaloo: Hacking Animal Jam"
subtitle: "~ AKA, how we pwned Animal Jam ~"
date: 2021-9-7
author: chob
tags: pentesting
finished: false
excerpt_separator: <!--more-->
---

> **DISCLAIMER**: The aim of this post is not to offend or attack anyone. Instead it is to inform and explain to you about how Animal Jam was pwned by us (AstroSquad). I do not condone malicious behavior. Thank you for understanding. 

# Preface
As the title suggests, this post will touch on how me and a few other junior security researchers (also called nicknamed "AstroSquad") were able to "hack" or "pwn" a poorly secured kids game with many apparent predators. <!--more--> This all started when one of our researchers (psuedo named "Moon") found the game known as "Animal Jam", he decided the take a look into the game and in a couple of hours found that the API had no rate-limit on the RPS (request per second), which he informed us about and one of us wrote a C++ script to generate accounts which allowed us to get hundreds of bots onto the Animal Jam player servers and just because of how unoptimized Animal Jam is, it allowed us to freeze players games and even allowed us to crash them. That was only the start although, a few months later, another one of our researchers (HellSec) found the IP of one of their boxes, we assumed that we couldn't get into it but with a few minutes we figured it was a AWS box and we became dedicated to pwning the box and few hours we were able to pwn the box. Surprisingly their was not much on the box, but we looked into the files anyways and figured out that it was the shop for Animal Jam (shop.animaljam.com).
