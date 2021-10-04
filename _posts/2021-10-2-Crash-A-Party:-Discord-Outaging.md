---
layout: post
title: "Crash A Party: Discord Outaging"
subtitle: "~ AKA, how a group of people caused Discord servers to outage ~"
date: 2021-10-2
author: chob
tags: pentesting
finished: true
excerpt_separator: <!--more-->
---

> **DISCLAIMER**: The aim of this post is not to offend or attack anyone. Instead it is to inform to you about how a group were able to exploit Discord. I do not condone malicious behavior. Thank you for understanding.

# Preface
As the title suggests, this post will touch on how a group of people exploited Discords gateway to cause server outage. <!--more--> <br>

On May 5, 2020 a [video](https://www.youtube.com/watch?v=GA-tnZ8wlbY) was made by a user named "iLinked", basically the video was a "meme" video of them exploiting Discord's gateway to cause a server outage with how to do the exploit in the description.

I will be breaking down the exploit iLinked put in the descrption on how it works for the people reading that aren't very knowledgeable with programming.

<a href="/img/Crash-A-Party:-Discord-Outaging/w.png" target="_blank"><img class="centerImgMedium" src="/img/Crash-A-Party:-Discord-Outaging/w.png"></a> <br>

So first, you start a websocket on `wss://gateway.discord.gg/?v=9&encoding=json` which is the current gateway on their 9th generation. <br>

After creating a websocket connection, you send over identify packet (this tells the gateway what type of client we are and provides our authentication (token), without this, you cannot use the gateway & will be disconnected.) <br>

After sending the identify packet, you can send over a heartbeat packet (this tells the gateway that you are still alive and have not lost connection, if we don't send this we will be automatically disconnected from the gateway.) <br>

If you're confused, go ahead and read the Discord Developer Documentation on the [websocket gateway](https://discord.com/developers/docs/topics/gateway) which will explain it better on how to authenicate and connect to the gateway better than I can explain it. <br>

Now that it's authenicated and connected to the Discord's websocket, you can send the custom payload iLinked put in the description: <br>
```
{
   "op":14,
   "d":{
      "guild_id":"ID of the server here",
      "channels":{
         "ID of a text channel where u have permission to view it": [
            [
               0,
               -1
            ]
         ]
      }
   }
}
```
So what is the op14? well firstly, ponder the question; what is a op? A op is a operation code which tells the gateway what is the payload type, basically stating what type of the payload is being sent. <br>
Now, going on the [Discord Developer Documentation](https://discord.com/developers/docs/topics/opcodes-and-status-codes#gateway-opcodes) for operations code, I figured out it wasn't documented because it's a client based and the documentation is only for Discord bots, so I went to speak with multiple developers, I was able to gather that op14 basically gathers all online guild/servers members chunks and returns it. <br>
The "d" section is a description. In the description, it has a guild_id: which is the servers id with the channels{} which inside the bracket contains the channel ids, essentially fetching all the online guild members in the guild channel(s), but the very **key part** is the `[0,-1]` that is very important because then that means the gateway attempts to fetch -1 guild members and -1 guild members is not possible which is why this works, there is no error handling fo this. <br>
