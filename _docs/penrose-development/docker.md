---
title: Deploying with Docker
category: Penrose Development
order: 2
---

If you are interested in building the Docker container, see the [Docker install]({{ site.baseurl }}/install/docker/)
pages. This guide here will describe the steps needed to have the container built (and automatically deployed)
to Docker Hub. This is useful for the following reasons:

 - each merge to master will be associated with a container build, meaning the software is available for each version.
 - using Penrose is much easier with a container because you don't need to install dependencies.

Specifically, when a user opens a pull request (PR), the PR will build the container on Circle CI, and when the pull request is merged, it will Deploy the container to Docker Hub. The container will be versioned with the first 10 alpha numeric characters of the commit, to always link back to the code base.

## Getting Started

We are assuming that you are starting with the [penrose](https://www.github.com/penrose/penrose) repository cloned, meaning it includes the hidden `.circleci` folder.  Today you will be doing the following:

  1.  Fork and clone the Penrose Github repository and confirm having the hidden `.circleci` folder.
  2.  creating an image repository on Docker Hub
  3.  connecting your repository to CircleCI
  4.  opening a pull request to run the build, which deploys to Docker Hub on merge to master

You don't need to install any dependencies on your host to build the
container, it will be done on a continuous integration server, and the
container built and available to you to pull from Docker Hub.

### Step 1. Clone the Repository

First, fork the [penrose](https://www.github.com/penrose/penrose)
Github repository to your account, and clone the fork:

```bash
git clone https://www.github.com/<username>/penrose
git clone git@github.com:<username>/penrose.git
```

### Step 2. Configuration

The hidden folder [.circleci/config.yml](.circleci/config.yml) has instructions for
[CircleCI](https://circleci.com/dashboard/) to build the Penrose container.
The build steps include the following:

 1.  clone of the repository with the penrose Github url that you specify
 2.  build
 3.  on merge, push to Docker Hub

If you look in the header of the file, you will notice some environment variables
that you can edit - the container name! This will be the Docker Hub repository
that is pushed to, which needs to exist. 

```yaml
    - PENROSE_CONTAINER: vanessa/penrose
```

Let's talk about creating this repository next.

### Step 3. Docker Hub

Go to [Docker Hub](https://hub.docker.com/), log in, and click the big
blue button that says "create repository" (not an automated build).
Choose an organization and name that you like (in the traditional format
`<ORG>/<NAME>`), and remember it! Add it to the build recipe
in the `environment` section we showed above.

### Step 4. Connect to CircleCI

If you do not already have a Circle CI account, head [here](https://circleci.com/signup/) and create one, and
add your project to your Circle CI account.  Here are [instructions](https://circleci.com/docs/getting-started/) if you've never done this before.

Once you have an account, if you navigate to the main [app page](https://circleci.com/dashboard/)
you should be able to click "Add Projects" and then select your
repository. If you don't see it on the list, then select a different
organization in the top left. Once you find the repository, you can
click the button to "Start Building" and accept the defaults.

Before you push or trigger a build, let's set up the following
environment variables. Also in the project interface on CirleCI, click
the gears icon next to the project name to get to your project settings.
Under settings, click on the "Environment Variables" tab. In this
section, you want to define the following:

1.  `DOCKER_USER` and `DOCKER_PASS` should be your credentials (to allowing pushing) **required**
2.  `DOCKER_TAG` is the tag you want to use. If not defined, will use first 10 characters of commit (optional)
3.  `CONTAINER_NAME` if you *don't* want to define it in the config, you can remove it from the config and define it here (optional)

If you don't define either of the variables for the Docker credentials, your image
will build but not be pushed to Docker Hub.

### Step 5. Push and Deploy!

Once the environment variables are set up, you can push or issue a pull
request to see circle build the workflow. Remember that you only need
the `.circleci/config.yml`and not any other files in the repository. 

**How do I use my container?**

Once the container is deployed, you should see it appear (along with tags)
on the associated Docker Hub page. The basic run usage is:

```bash
docker 

```bash
docker pull <ORG>/<NAME>:<TAG>
docker run <ORG>/<NAME>:<TAG>
```

## Support
If you want to get help please [post an issue!](https://www.github.com/penrose/penrose/issues/)
