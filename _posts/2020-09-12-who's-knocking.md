---
layout: post
title:  "Who's Knocking at my Door?"
categories: digital-ocean login 
---

As part of the Linux Up Skill Challenge, you set up your own server and expose it to the world. This comes with some risks because people (or bots, whatever...) will immediately start trying to log in and take control of your server. I was curious as to how many people would be trying to login and from where, so I wrote a simple and inelegant bash script to show me.

{% gist c036fe136e42c2c3fdab5d4792d639c4 %}

In the mere 16 hours, I've had over 10,000 login attempts! 

![image](/assets/2020-09-12-05-43-14.png)

You can check out the country codes with this [website](https://www.iban.com/country-codes).

Also with this post I figured out how to embed gists for easy code viewing - [Jekyll Gist](https://github.com/jekyll/jekyll-gist).