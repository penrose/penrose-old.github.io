---
title: Penrose via Docker
category: Install
order: 2
---

We currently provide a container hosted at [vanessa/penrose](http://hub.docker.com/r/vanessa/penrose)
that you can use to quickly run Penrose without any installation of other dependencies
or compiling on your host. 

If you've never used Docker, you can [install Docker](https://www.docker.com/get-started) and learn
about containers. When you are ready, try running Penrose using it:

```bash
$ docker run {{ site.docker }}
Usage: ./Main prog1.sub prog2.sty prog3.dsl
```

That will show you the Penrose usage! That's it.
