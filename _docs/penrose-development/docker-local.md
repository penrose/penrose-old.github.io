---
title: Deploying with Docker
category: Penrose Development
order: 3
---

If you are interested in building the Docker container, see the [Docker install]({{ site.baseurl }}/install/docker/)
pages. If you are interested in understanding how the container is built on CircleCI, see the [Docker Deployment]({{ site.baseurl }}/penrose-development/docker/) pages. This page will give you pointers on local Dockerized development of the container.

## Building Your Container

We start here assuming that you have forked penrose, cloned your fork, and are sitting at the root of the
repository. If you want to recompile the code, the best we can currently do is to build the Dockerfile from scratch. That looks like
this:

```bash
$ docker build -f docker/Dockerfile -t vanessa/penrose .
```

## Updating the Container

Now let's say that we want to update some of the static files (without recompiling). Since 
the Dockerfile has an `ADD` statement, this means that all following statements are re-run (and
you will be waiting at least another 30 minutes to use your container!). This isn't so great.
Instead, we've provided a Dockerfile that you can use (that bootstraps the original `vanessa/penrose`
that *only* updates these static files. Let' pretend that we've made a change to the docker/entrypoint.sh
and we want to build a new container. Let's call it `vanessa/penrose:dev`

```bash
$ docker build -f docker/Dockerfile.dev -t vanessa/penrose:dev .
```

## Shelling Into the Container

If you need to debug, you can start the container with a different entrypoint to shell inside:

```bash
$ docker run -it --entrypoint bash vanessa/penrose
root@1981f3478a60:/penrose#
```

## Support
If you want to get help please [post an issue!](https://www.github.com/penrose/penrose/issues/)
