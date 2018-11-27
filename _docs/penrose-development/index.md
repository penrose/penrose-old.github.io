---
title: Penrose Development
category: Penrose Development
permalink: /penrose-development/index.html
order: 1
---

If you are here, you are interested in developing the core Penrose software.
Likely, you want to set up a development environment first. We recommend that
you look at the [install options]({{ site.baseurl }}/install/) for penrose. You
can either build Penrose locally, or using a Docker container. Once you have
a set up that you like, you can continue reading the following topics.

## Docker

 - [Deployment]({{ site.baseurl }}/penrose-development/docker/): happens with continuous integration via Github. Once set up, when you issue a pull request to the penrose repository, it will build a container, and deploy on merge.
 - [Local]({{ site.baseurl }}/penrose-development/docker-local/): are some quick start steps for building Docker containers locally.

## Local

 - [Building and Running]({{ site.baseurl }}/penrose-development/building-running/): are some quick start steps for building from source, locally.
 - [Debugging]({{ site.baseurl }}/penrose-development/debugging/): some helpful hints when things go wrong. Which never happens, of course :)
 - [Testing]({{ site.baseurl }}/penrose-development/testing/): good practices for writing and running tests.
 - [Parameters]({{ site.baseurl }}/penrose-development/parameters/): that might be useful for snap.
 - [Profiling]({{ site.baseurl }}/penrose-development/profiling/): good practices for writing and running tests.
 - [Docstrings]({{ site.baseurl }}/penrose-development/documentation/): Haskell documentation
 - [React Frontend]({{ site.baseurl }}//penrose-development/react-rendering-frontend/)

Since the documentation is under development, for now we recommend using the Docker container (the first link above.)

**Note** none of the pages are edited or tested yet, this section is under development!
