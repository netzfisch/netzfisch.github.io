---
layout:    default
tags:      developement 2cent application docker config
title:     Docker cheat sheet, links, tips and tricks
---
## docker cheat sheet

Here you find **Docker** specific and often used hints and **workflows**. As well as some configurations and links.

### clarify routing

tbd!

### clean things up

* remove all containers `docker rm -f $(docker ps -a -q)`
* remove all volumes `docker volume prune`
* remove all images `docker rmi -f $(docker images -a -q)

### Links

Install Docker on

* [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
* [Raspian](https://docs.docker.com/engine/install/raspberry-pi-os/)

... for testing/development check the [convenience script](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-convenience-script)!