---
layout: post
title:  "Dockerizing Go"
categories: docker go
date: 2020-11-10 14:18:00
---

When I was starting out with Go and Gin, I just wanted to see if I could get it working, so there wasn't any fancy trimmings, e.g. Docker. I've never Dockerized an application, only been given applications with Dockerfiles already built. Turns out that it's not that hard, here's my `Dockerfile`

```Docker
# Base it off of the golang image
FROM golang:1.15

# Set the `PORT` env var
ENV PORT 8080

# Create a working dir
WORKDIR /code

# Copy everything to workdir
COPY . .

# Get gin
RUN ["go", "get", "-u", "github.com/gin-gonic/gin"]

# Build the package
RUN ["go", "build", "-o", "main", "."]

# Expose port 8080 (so that we can hit it)
EXPOSE 8080

# Run the built main program
CMD ["./main"]
```

I'm not a Docker Guru, so this may not be the best practices, the terminology may not be entirely accurate, and there are probably better ways to do it, but hey, it works. And then to run it:

`docker run -it -p 8080:8080 go-gin`

I decided to re-run the performance tests to see how Docker impacts the speed.

![](/../assets/2020-11-10-15-28-35.png)

Speeds are a bit slower than without Docker, but still faster than Python. 