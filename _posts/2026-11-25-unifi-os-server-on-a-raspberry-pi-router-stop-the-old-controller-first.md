---
layout:    post
category:  linux
tags:      raspberrypi unifi podman docker howto
title:     UniFi OS Server on a Raspberry Pi router - stop the old controller first
---
The Docker image **`ryansch/unifi-rpi`** that ran my UniFi controller for
years is **deprecated**, and Ubiquiti now ships its own **UniFi OS Server**
for Linux. My Pi is also the **VLAN router, DNS server and NAS** of the house,
so I planned the careful way: install the new one next to the old one, then
switch. That plan died on the first `ss`. The old container sits on the
**same four ports** the new one binds at install time, so the order is stop,
install, restore - and it took **15 minutes** with nobody noticing. Addresses
below are placeholders.

### The symptom

Nothing was broken. The Docker Hub page simply said "No further version
updates will be published here", and my container showed how long I had not
looked:

    $ docker images | grep unifi
    ryansch/unifi-rpi:latest    060a263178d3    13 months ago

Network **9.5.21**, while `latest` on the hub was already 10.4.

### Why "install first" does not work

The old container runs with `network_mode: host`. UniFi OS Server is a
**rootless podman** container that publishes its ports on the host:

    $ sudo ss -tulpn | grep -E ':(3478|6789|8080|8880)\s'
    udp  *:3478  users:(("java",pid=2100,fd=215))
    tcp  *:6789  users:(("java",pid=2100,fd=198))
    tcp  *:8880  users:(("java",pid=2100,fd=189))
    tcp  *:8080  users:(("java",pid=2100,fd=190))

Port **8080 is the inform port** the access points talk to, so there is no
"run both and compare". What can run in advance is everything else: packages,
download, snapshots. The access points keep serving WiFi without a controller.

### Prepare while the old one runs

    $ df -h /home                 # installer wants 15 GB there
    $ docker tag "$(docker inspect unifi --format '{{.Image}}')" ryansch/unifi-rpi:rollback
    $ sudo iptables -S > iptables.before && sudo iptables -t nat -S >> iptables.before
    $ sudo ss -tulpnH | awk '{print $1, $5}' | sort -u > ports.before
    $ apt-get install -s podman slirp4netns | grep upgraded
    0 upgraded, 28 newly installed, 0 to remove
    $ sudo apt-get install podman slirp4netns

The **rollback tag** matters: the compose file says `latest`, and I did not
want a revert to depend on what `latest` means that day. The dry run matters
on a router: **0 upgraded** means no library changes under dnsmasq or Docker.
The installer itself is a **735 MB** binary from the [download page][1].

### Cutover

    $ sudo systemctl disable --now unifi.service
    $ docker volume ls | grep -c unifi
    6
    $ sudo ./unifiosinstaller --non-interactive
    ...
    WARN UniFi OS Server startup timed out after 60 seconds.
    WARN On low-powered devices, the web interface can take a few minutes

The **six volumes stay**. They are the whole rollback, so never
`docker compose down -v` at this point. The timeout warning is harmless on a
Pi; the UI answered ten seconds later on **port 11443**. That is the new
default: the container's 443 is mapped to an unprivileged host port.

### Restore

The setup wizard offered me **no "Restore from backup"** with a local account.
The restore lives one level deeper: finish the wizard, open the **Network**
application, then Settings → System → Backups → Restore. The 9.5.21 backup
went straight into the bundled **10.5.67**:

    WARN system - Backup restored successfully. Restarting the server to apply changes.

Same IP, same inform port, so all four access points came back **without
`set-inform`**. The restore keeps the fresh admin account and skips the ones
from the backup.

### Did it touch the router?

That was my real worry, with a `FORWARD` policy of `DROP` and hand-written
NAT rules on the same box. Rootless podman with **pasta** does its networking
in userspace:

    $ sudo iptables -S > iptables.after && sudo iptables -t nat -S >> iptables.after
    $ diff iptables.before iptables.after && echo unchanged
    unchanged

The port diff showed only the new UniFi ports. DNS, DHCP and Samba listeners
were **identical**, and the same held after a reboot.

### Revert and check

**Revert**, as long as the old volumes exist:

    $ sudo systemctl disable --now uosserver uosserver-updater
    $ sudo systemctl enable --now /opt/unifi/unifi.service

**Check** after a reboot. Give the access points five minutes; mine backed
off while the controller was away, and only **one of four** was back at
first:

    $ systemctl is-active uosserver dnsmasq smbd docker
    $ curl -k -s -o /dev/null -w '%{http_code}\n' https://localhost:11443/
    200

### What did *not* work

* `./unifiosinstaller install` from a guide for version 4.3 -
  **`error: unexpected argument 'install' found`**. Version 5.1 takes
  options only; `--help` lists `--non-interactive`, `--web-port` and
  `--uninstall`.
* Looking for the restore in the wizard or on the UniFi OS dashboard - it is
  inside the Network application.
* Reading **"SSH adopt failed ... unreachable"** in the log as a network
  problem - arrgh! The controller tries SSH for 25 seconds after every
  restart, the access points have SSH closed, and the errors stop the moment
  they inform on their own.
* Resizing the **zram** swap I added afterwards (the installer warns without
  2 GiB of swap) by restarting its unit - "Device or resource busy" until
  `zramctl --reset /dev/zram0`.

Ubiquiti documents the supported setup in [Self-Hosting UniFi][2], the old
image says goodbye in its [README][3], and my cloud-init template for the
whole Pi, now with UniFi OS Server, lives in [rpi-cloud-server][4].

[1]: https://ui.com/download
[2]: https://help.ui.com/hc/en-us/articles/34210126298775-Self-Hosting-UniFi
[3]: https://github.com/ryansch/docker-unifi-rpi
[4]: https://github.com/netzfisch/rpi-cloud-server
