---
layout: post
title:  "Visualizing Login Attempts"
categories: digital-ocean login cron
date: 2020-09-12 14:28:00
---

In my [previous post]({% post_url 2020-09-12-who's-knocking %}), I created a simple script to figure out which countries had people trying to login to my server. I'd have to manually ssh in to my server, copy + paste this script, and then run it to see what I wanted to see. I decided I wanted to be able to check up on this from a web browser, so I went ahead and made that happen.

First thing's first, I installed Apache and made sure I could hit my server.

![](/assets/2020-09-12-14-31-45.png)

Then I experimented with cron jobs, because I hadn't ever set one up before. There were a couple gotchas:

* You won't see the output unless you route it somewhere else. I had `date` set to run every minute, but nothing happened until I redirected output to a log file. 
* You need to give a path to the script if you're running it. For some reason I thought I could just say `firstCron.sh` instead of `~/firstCron.sh`, so this one was a very simple mistake.


```
* * * * * date >> ~/simple.log
* * * * * ~/firstCron.sh
```

Some useful commands are:
* `crontab -l` to list your crontab file
* `crontab -e` to edit your crontab file
* Prepending `sudo` to the above commands to have root run your cron jobs

I set up a cron job to run every hour that gets the IPs, looks their geography up, sorts the output, counts the unique countries, writes this out to a file, and puts that file on the server at /logins (location subject to change). The result looks like this:

![](/assets/2020-09-12-14-54-04.png)

Nothing crazy, but it is far more convenient that it was before to see who is trying to log in.