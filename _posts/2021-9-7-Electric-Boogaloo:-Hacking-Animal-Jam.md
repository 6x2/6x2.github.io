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

> **DISCLAIMER**: The aim of this post is not to offend or attack anyone. Instead it is to inform to you the story about how Animal Jam was pwned by us (AstroSquad). I do not condone malicious behavior. Thank you for understanding.

# Preface
As the title suggests, this post will touch on how me and a few other junior security researchers (also nicknamed "AstroSquad") were able to "hack" or "pwn" a poorly secured kids game called "Animal Jam". <!--more--> This all started when one of our researchers (psuedo named "Moon") looked into Animal Jam, he messed around with the API and told us that there wasn't a rate limit on the RPS (Request Per Second), one of us wrote a script to automate the registration process and made hundreds of bots that joined the servers which froze players clients, they shortly added rate limit globally to the entire API. That was only the start from what we started doing. On August 6 2020, another one of our researchers (psuedo named "HellSec") found the IP of one of their boxes, we assumed that we couldn't get into it at first, but within a few hours, we managed to pwn the box. Surprisingly there was not much on the box, it was a AWS box and it was the box for the Animal Jam shop (**shop.animaljam.com**). With a limited time, we made a simple deface page and overwrote it to the index page.

<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/compilation.png" target="_blank"><img class="centerImgMedium" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/compilation.png"></a>

As you can see from the complilation above, they believed we had a apparent "ip logger" and other malicious items, this gave us a notorious reputation as this group of 40 year olds in their mother's basement that are very **1337**, which was not the case. From there, we wanted to test how much the community and the company WildWorks was afraid of apparent fake threats. So around October 2020, we made a threat that we were going to "hack" Animal Jam on halloween, the community freaked and made a bunch of YouTube videos with WildWorks putting their servers on lockdown. It was the biggest lawl for us. <br>

Now that you understand the context before the main pwn; without further ado, let's get into the main pwn. On August 16, 2021, we discovered a 0day **no-auth** full account takeover on Animal Jam. How? simple. A fucking password reset endpoint. Let me repeat that, a fucking **password reset endpoint**, someone must've been some crack while writing the endpoint lol (╯°□°）╯︵ ┻━┻.
***

