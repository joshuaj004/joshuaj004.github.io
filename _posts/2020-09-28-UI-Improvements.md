---
layout: post
title:  "UI Improvements"
categories: sudoku vue 
date: 2020-09-28 12:12:00
---

In the same vein as yesterday, I'd planned on making some improvements to the UI. A couple things were bugging me:
* Lots of unused CSS Rules
    - No final effect on the look, but these were low hanging fruit.
* The board didn't fit nicely on mobile
    - It was like 20px too large which led to a lot of scrolling back and forth. A small thing, but highly annoying.
* Added a new mode to 'check as you go'
    - This makes any wrong numbers red.
* Used some [Vuetify](https://dev.vuetifyjs.com/en/) components instead of the default browser ones
    - This makes the difficulty slider and seed number look a bit nicer.

![](/../assets/2020-09-28-12-16-20.png)