---
layout:    post
category:  linux
tags:      docker network routing howto
title:     Office host unreachable - blame Docker's default subnet
---
`ping build.corp` answered **Destination Host Unreachable**, and the answer came
from **my own laptop**. VPN, Wi-Fi and the office gateway were fine, the host
was up for everyone else. The culprit was a **Docker bridge network** that a
tool I had uninstalled a month ago left behind - sitting on the very /16 the
office uses and swallowing every packet for it. Addresses below are changed,
the mechanism is not.

### The symptom

    $ ping build.corp
    PING build.corp (172.19.40.20) 56(84) bytes of data.
    From laptop (172.19.0.1) icmp_seq=1 Destination Host Unreachable
    $ traceroute build.corp
     1  laptop (172.19.0.1)  3049.063 ms !H  3049.049 ms !H  3049.046 ms !H

The **`!H`** on hop one means the kernel never handed the packet to a gateway.
It decided the destination was **directly attached** and waited three seconds
for an ARP reply that never came.

### Read the routing table, not the VPN status

    $ ip -4 route
    default via 172.16.0.1 dev eth0
    172.16.0.0/16 dev eth0 proto kernel scope link
    172.17.0.0/16 dev docker0 proto kernel scope link
    172.18.0.0/16 dev br-1a2b3c4d5e6f proto kernel scope link
    172.19.0.0/16 dev br-9f8e7d6c5b4a proto kernel scope link

Office hosts live in 172.19.x.x behind the gateway, and there is a **local
route for 172.19.0.0/16** on a bridge. Longest prefix wins over the default
route. One command settles it:

    $ ip route get 172.19.40.20
    172.19.40.20 dev br-9f8e7d6c5b4a src 172.19.0.1
    $ ip -br addr show br-9f8e7d6c5b4a
    br-9f8e7d6c5b4a  DOWN  172.19.0.1/16

The bridge is even **DOWN** - nothing plugged in. The hex after `br-` is a
Docker network id, and the network's labels name the owner:

{% raw %}
    $ docker network ls
    NETWORK ID     NAME               DRIVER
    9f8e7d6c5b4a   openshell-docker   bridge
    $ docker network inspect openshell-docker \
        --format '{{json .Labels}} {{len .Containers}}'
    {"openshell.ai/managed-by":"openshell"} 0
{% endraw %}

[OpenShell][2], an agent sandbox I had tried for an afternoon. Binary, config
and images were long gone, the network survived - `docker network` objects
live in the daemon, out of reach of any `rm -rf ~/.tool`. **Zero containers**,
so nothing would miss it.

Docker hands every user-defined network a **whole /16** from a built-in pool:
172.17 for `docker0`, then 172.18, 172.19, ... in creation order. The third
network on a box already collides with any company that numbers its offices
out of 172.16.0.0/12. Nobody checks, and at home nobody ever notices.

### The fix: pick your own pool

Deleting the orphan cures today's symptom:

    $ docker network rm openshell-docker
    $ ip route get 172.19.40.20
    172.19.40.20 via 172.16.0.1 dev eth0 src 172.16.5.7

The next `docker compose up` in a fresh project would grab that /16 again, so
tell the daemon where new networks may live, in `/etc/docker/daemon.json`:

    {
      "bip": "10.200.0.1/24",
      "default-address-pools": [
        { "base": "10.201.0.0/16", "size": 24 }
      ]
    }

    $ sudo systemctl restart docker

`default-address-pools` is the range for **new user-defined networks**, `size`
the prefix each one gets: a /16 cut into /24 slices is **256 networks** of 253
containers, one per compose project. `bip` moves `docker0` alone and is
optional, but taking it along keeps every Docker range out of 172.16.0.0/12.
It must not overlap the pool, or `dockerd` refuses to start.

**Revert**: remove the two keys and restart the daemon. **Check** any
suspicious route later with:

{% raw %}
    $ ip route get <ip>             # "dev br-..." instead of "via" = collision
    $ docker network inspect <name> \
        --format '{{range .IPAM.Config}}{{.Subnet}}{{end}}'
{% endraw %}

### What did *not* help

* `default-address-pools` alone - it applies to networks created **after** the
  restart. The colliding one still had to be removed.
* `bip` alone - it moves `docker0` only. Compose projects keep drawing from
  the 172.x pool.
* `ip route add <ip>/32 via <gateway>` - instant relief, but one address and
  gone after a reboot.

Both options are described in the [dockerd reference][1], the leftover came
from [OpenShell][2], and [RFC 1918][3] explains why 172.16.0.0/12 is so
crowded in the first place.

[1]: https://docs.docker.com/reference/cli/dockerd/
[2]: https://github.com/NVIDIA/OpenShell
[3]: https://datatracker.ietf.org/doc/html/rfc1918
