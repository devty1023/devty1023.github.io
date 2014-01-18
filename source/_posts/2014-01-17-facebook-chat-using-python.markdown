---
layout: post
title: "facebook chat using python"
date: 2014-01-17 16:30:41 -0500
comments: true
categories: [facebook, python]
---

I wanted to create a simple python script to connect to facebook chat. 

<!--more-->

It became pretty obvious, from reading [facebook chat api docs]( https://developers.facebook.com/docs/chat/) that I needed to get myself familar with [Jabber/XMPP protocol](http://en.wikipedia.org/wiki/XMPP), so I read [this](http://en.wikipedia.org/wiki/XMPP)

Fortunately, there are several python modules that supports xmpp protocol. I experimented with [xmpppy](http://xmpppy.sourceforge.net/) and [jabber.py](http://jabberpy.sourceforge.net/), but I eventually found [sleekXMPP](https://github.com/fritzy/SleekXMPP/wiki) to be the most suitable (i.e. I got what I needed to accomplish through sleekXMPP). If anything, it is the most actively maintained moduled of the three I tried.

sleekXMPP provides readily reable and understandable [examples](http://sleekxmpp.com/getting_started/echobot.html) to get us started. Here are few things I needed to do to use sleekXMPP to bring facebook chat to python.

1. `class sleekxmpp.ClientXMPP` accepts two parameters: `jid` and `password`. In facebook chat, `access_token` takes care of the authentication, so leaving those two parameter empty seems to be okay.

2. We do, however, need to provide an optional parameter: `sasl_mech`. Facebook chat authenticates user with 'X-FACEBOOK-PLATFORM' (or plain SASL). I thought that it would be okay to not provide this optional parameter, thinking that we can use the default plain SASL to authenticate xmpp connection, but it seems like we need to do this.

3. As I understand by reading the api document, we need to provide `api_key` and `access_token` during the [*challenge*](http://wiki.xmpp.org/web/SASLandDIGEST-MD5) phase of the xmpp protocol. Composing a correct response to server challenge requires encoding, decoding, hashing, and other hallaboongas - all of which sleekXMPP does automatically! We simply need to add `credentials` information to our client.

4. I needed a way to check if I've successfuly sent a message to the intended recipient. I did not implement this feature yet, but I found [this stackoverflow response](http://stackoverflow.com/questions/9815422/sleekxmpp-threaded-authentication) to use as my starting point. 


## Epilogue
### fbchatpy
Naive implementation of [Facebook Chat API](https://developers.facebook.com/docs/chat/) on python. View the source at [github](https://github.com/devty1023/fbchatpy)
