---
title: Hello World
category: User Documentation
order: 2
---

## Getting started

### Docker

We have a container provided on [Docker Hub](https://hub.docker.com/r/vanessa/penrose) for you to use! 
We assume you have [Docker installed](https://www.docker.com/products/docker-desktop).


#### Penrose Binary
Let's run the container without any special binds or commands, and this will hit the Penrose
binary:


```bash
$ docker run -it vanessa/penrose
Usage: ./Main prog1.sub prog2.sty prog3.dsl
```

What else can we do? Let's ask for help.

```bash
$ docker run -it vanessa/penrose --help
Usage:

         docker run <container> [help|web]
         docker run -p 8000:8000 -p 9060:9060 <container> web

         Options:
            --port -p :    change the penrose port (default 8000)
            --template -t: choose a penrose template to run (blank to see options)
            --custom:  -c: provide your own determinants.sub, *.sty, and *.dsl
                           as input to the penrose executable

         Commands:

                help: show help and exit
                web: start penrose frontend (requires mapping of ports)         

         Examples:
             docker run -p 8000:8000 -p 9060:9060 <container> web --custom math.sub math.sty math.det
             docker run -p 8000:8000 -p 9060:9060 <container> web linear-algebra
```


Cool! Let's try mapping a port with web, and see what happens...

```bash
$ docker run -it -p 8000:8000 -p 9160:9160 vanessa/penrose web
Please specify a template to run! Choices are:

linear-algebra
real-analysis
```

Oh right, I need to specify a domain template!

```bash
$ docker run -it -p 8000:8000 -p 9160:9160 vanessa/penrose web --template linear-algebra
Please specify a template to run! Choices are:

linear-algebra
real-analysis
```

A bunch of text will be output to the console (I'm not sure yet what this is) but then 
you can open your browser to [http://localhost:8000/client.html](http://localhost:8000/client.html). 
Note that @vsoch is currently still struggling to get websockets working. More to come.
