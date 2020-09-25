---
layout: post
title:  "Adding a Timer and Pulling From DockerHub"
categories: sudoku
date: 2020-09-25 12:43:00
---

I've added a timer to the sudoku page so that you know how long you've been working on a puzzle! It resets anytime you get a new board and it stops when you've successfully solved + verified a board. It was easy enough to add, just a SetInterval call. 

![](/../assets/2020-09-25-15-06-34.png)

I've also gotten better at the workflow to update the program. Before, I was making changes to the project, committing them, uploading to gitlab, pulling them onto my server, building the docker images on my server, and starting the new docker images. Which is fine, don't get me wrong, it works. But it's not optimal. For one, these servers are pretty cheap which means limited CPU + memory. On the $5/month server, I can only build the image with the addition of swap memory (which isn't recommended because it has too many read/writes for an SSD). On the $10/month server, I can build the docker images if and only if the only other running docker stack is Traefik. So the preferred way to handle this is to build the docker images (rather quickly) on my desktop, push them to dockerhub, and pull them onto my server. Then I can easily start the service without killing my server. It's a definite improvement. 

An update on the non server things. I upgraded my desktop's Ubuntu version to 20.04 which is fine and dandy, but it messed with ruby or bundle or something and now I can't run `bundle exec jekyll serve` without running into errors. I spent like an hour trying to figure out what the issue is, but I think it's a few things. Anyways, I'll probably check it out tomorrow. One cool thing is that since being able to get Magic The Gathering Arena working on Ubuntu (through Lutris), I haven't booted into Windows! For a couple weeks, I would boot into Windows once every couple of days and it would always have 7 million updates to download and install which was really annoying. Anyways, I shouldn't have to deal with that for a while at least.