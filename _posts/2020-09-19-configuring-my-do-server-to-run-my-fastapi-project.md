---
layout: post
title:  "Configuring my Digital Ocean Server to Run my FastAPI Project"
categories: digitalocean fastapi
date: 2020-09-19 09:24:00
---

This'll be a quick post explaining what I had to do to get my server to run my project. 

The template comes with a gitlab-ci file so I decided to try out gitlab and signed up for an account there. There's a free tier so I'm just using that at the moment. I also had to generate and add a PGP key and gitlab walks you through the process.

Next, I cloned the repo onto my server and installed the dependencies.

Docker: [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04)

Docker Compose: [https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-compose-on-ubuntu-20-04)

Poetry: [https://python-poetry.org/docs/](https://python-poetry.org/docs/)

NodeJS: [https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-20-04)

And by running `df -h` we still have 22G left on this $5/month droplet.

By copying over `.env` and `cookiecutter-config-file.yml` files we can then run `docker-compose up -d` and it'll pull down all the relevant docker images. Don't forget to also copy over the `.env` file in the frontend directory.

Unfortunately, I don't think this droplet has enough memory (1GB) to run this (the front-end alone wants 1 worker with 2048mb), so let's add some swap memory and see if that works.

How To Add Swap Space on Ubuntu 20.04: [https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04](https://www.digitalocean.com/community/tutorials/how-to-add-swap-space-on-ubuntu-20-04)


I'm going to give it 4 GB to see if it works now.

`sudo fallocate -l 4G /swapfile`

I'm running into a CORS issue, so I'm adding my IP address to the `BACKEND_CORS_ORIGINS` array in the `.env` file and updated the value of `VUE_APP_DOMAIN_DEV` to be the ip of my server. I don't think this is the right way to communicate with my backend server on the same machine, but I will deal with that later!

It lives! And it's only using about 200MB of the swap file. Really a better thing to do would be to upgrade the server to have 2GB, but once again, that can be dealt with later!

![](/../assets/2020-09-19-10-31-15.png)