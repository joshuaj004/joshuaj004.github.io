---
layout: post
title:  "Contributing to Open Source"
categories: open-source
date: 2020-11-03 16:16:00
---

I made what was possibly my first Open Source PR today! It wasn't a major change, in fact it was very small -- 6 lines. And it was for documentation, but it still counts! I noticed that the docs for [K6](https://k6.io/) had an extra '$' prepended to the command (see screenshot below).

![](/../assets/2020-11-03-16-19-04.png)

This means that if you copy and paste it into your terminal, it will fail and complain about the dollar sign. I opened up a PR to remove this. The PR is [here](https://github.com/loadimpact/k6-docs/pull/146) if you're curious. It looks like the team is on Swedish time, so I probably won't get any feedback until tomorrow, but that's okay there's no rush. I've written about using k6 for performance testing previously and if you haven't used it, I highly recommend it! I also played around with Golang and man is it fast! I was seeing nanoseconds and microseconds instead of the milliseconds I'm used to.