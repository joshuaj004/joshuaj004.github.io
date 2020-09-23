---
layout: post
title:  "SSL Certifications and Pagination"
categories: daily lets-encrypt pagination
date: 2020-09-23 15:34:00
---

I finally have valid SSL certs on my page! I'm actually not 100% sure why, but I'm pretty sure it's because I added address records for the `flower`, `pgadmin`, and `www` subdomains. What led me to this conclusion was grepping my traefik service logs and seeing a whole bunch of handshake failures on those urls. I am _thrilled_ this is resolved because it was driving me nuts. Now I can actually work on the the project itself rather than the devops portion. There are still a few things I would like to have working (including building and uploading docker images, setting up dev and staging environments, actually poking around some of the services I'm running (e.g. Traefik, Swarmpit, Swarmprom, Portainer)). It really depends on what seems most interesting.

Also: blog layout change. It turns out writing at least 1 blog post a day adds up and the home page was getting quite long. I didn't want people to have to download such a large page, so I looked up how pagination with Jekyll works. I found [this page for pagination](https://jekyllrb.com/docs/pagination/) and [this page for theming](https://jekyllrb.com/docs/themes/#overriding-theme-defaults).

Lastly, I ran some perf testing hitting the actual site. It's a bit slower and less powerful than running it locally which is kind of a funny thought because this isn't a super powerful computer. Of course, I could put this on a faster (read: more expensive) and it would probably blow my computer out of the water, but a $5-10/month machine is plenty for now.

(First run is generating real sudoku boards and second is just fetching an already made one)

![](/../assets/2020-09-23-15-47-01.png)