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

### UniFi OS Server (rootless podman)

* status `systemctl status uosserver uosserver-updater`
* restart `sudo systemctl restart uosserver`
* cli help `uosserver help`
* list the container `sudo -u uosserver -H bash -c 'cd /tmp && podman ps'`
* follow the Network log `sudo -u uosserver -H bash -c 'cd /tmp && podman exec uosserver tail -f /data/unifi/logs/server.log'`
* install or update `sudo ./unifiosinstaller --non-interactive`, options via `./unifiosinstaller --help`
* web UI on port `11443`, device inform on `8080`

### Links

Self-host the UniFi controller with [UniFi OS Server](https://help.ui.com/hc/en-us/articles/34210126298775-Self-Hosting-UniFi).

Install Docker on

* [Ubuntu](https://docs.docker.com/engine/install/ubuntu/)
* [Raspian](https://docs.docker.com/engine/install/raspberry-pi-os/)

... for testing/development check the [convenience script](https://docs.docker.com/engine/install/raspberry-pi-os/#install-using-the-convenience-script)!