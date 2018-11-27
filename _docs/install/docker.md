---
title: Penrose via Docker
category: Installation
order: 2
---

This is a bit of a trick, because you don't need to install a container persay. you
just need to ensure that you have [installed Docker](https://www.docker.com/get-started)
on your system.

For the container we will use, we currently provide a container hosted 
at [vanessa/penrose](http://hub.docker.com/r/vanessa/penrose) that you can use to 
quickly run Penrose without any installation of other dependencies
or compiling on your host. 

When you are ready, try running Penrose using it. This first command will
access the penrose executable, without any sort of webserver:

```bash
$ docker run {{ site.docker }}
Usage: ./Main prog1.sub prog2.sty prog3.dsl
```

That will show you the Penrose usage! That's it. For more commands and
how to get started, see the [user documentation]({{ site.baseurl }}/user-docs/)
pages.
