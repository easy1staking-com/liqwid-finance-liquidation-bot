# Index

- [Installing Docker](#installing-docker)
    * [Ubuntu](#ubuntu)
    * [Installing docker on ubuntu](#installing-docker-on-ubuntu)

# Installing Docker

Docker is the container runtime that will be used in this project to manage the components required to successfully run
one or multiple Liqwid Finance Liquidation BOTs.

Out there, there are several linux distributions and different ways to install docker. It's up to the user to choose what's the
best way to install docker based on their setup.

If you're a beginner or have little experience with docker, linux and containers, we will provide the simplest way to install docker on ubuntu,
one of the most popular linux distribution that can be installed on several cloud or VPS provider.

## Ubuntu

Whether you own a spare computer or rent a Virtual Private Server (VPS) to run your bot, I would recommend ubuntu as OS as it's
very popular, works well, and there are several article and website on how to solve majority of the issues you might encounter.

As ubuntu version I would recommend anything after 18.

## Installing docker on ubuntu

If you google how to install docker on ubuntu you'll see there are dozen of articles, some using `snap` otehr using `apt`.

I would recommend `snap`.

The first result on google is [Install Docker on Ubuntu w/ Snap](https://linux.how2shout.com/how-to-install-docker-using-snap-on-ubuntu-linux/) does the job,
and for handiness I'll summarise the steps here:

```bash 
sudo apt update && sudo apt upgrade # Ensure your OS is updated to latest packages
sudo snap install docker # Install Docker 
```

Yeah it is that simple BUT, in order to use docker with an ordinary non-root user like the `ubuntu` user that should have been
created by default on your OS, it is necessary to grant `ubuntu` with the permissions to use Docker.

These are the simple steps to follow:

```bash
sudo snap install docker
sudo adduser ubuntu docker # Replace ubuntu with the user you want to use
sudo adduser $USER docker
```

And you're done.

You can test the `ubuntu` user (or the user you've decided to use) can access docker by issuing `docker ps`

You should see something like

```bash
> docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
```

If instead of seeing this result you get an error, exit the current session and open a new one. This means that if you're on SSH, 
exit and ssh back into the system.

Congrats! You just installed Docker! You're a step closer to run a Liqwid Finance Liquidation BOT!
