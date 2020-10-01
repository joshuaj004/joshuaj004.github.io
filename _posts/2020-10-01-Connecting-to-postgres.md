---
layout: post
title:  "Connecting to PostgreSQL"
categories: opensource gitter postgres
date: 2020-10-01 11:31:00
---

After having skimmed the manual for pgAdmin, I felt ready to actually connect to the postgres instance I was running. I fired up the stack and opened the pgAdmin site on my localhost. The 'Add New Server' button beckoned to me and I could not resist. The name field was easy enough, `testConnection` or `db` is a good default. Host address was `localhost` and we're ready to rock and roll. Badda bing badda boom. Except localhost didn't work. 

![](/../assets/2020-10-01-11-35-24.png)

That's weird, it's definitely running on localhost. Running `docker-compose top` shows that it's running. After many searches for the solution, I found a link to [Gitter](https://gitter.im/) something I'd never heard of. Basically it's like slack for Gitlab and Github. I posted a message about this problem and they were very helpful.

![](/../assets/2020-10-01-11-38-29.png)

One person explained that the Postgres instance was running in a docker container and that the ports weren't exposed/declared as published in the `docker-compose.yml` file. They also explained that if I were to create a DB with psql, I'd need to do it from a container with access to the DB or from pgadmin.

Another explained that I could use the service name (`db`) as the address instead of `localhost` and let the docker internal DNS resolve the ip of the container. That worked like an absolute charm and I was able to connect to the databases from pgAdmin.

Very grateful to the two people who helped out a total n00b! Now I'll be able to play around with it and see what I can do!