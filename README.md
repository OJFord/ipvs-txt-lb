# IPVS-TXT-LB

## What?

Create an [IPVS](https://en.wikipedia.org/wiki/IP_Virtual_Server) Load Balancer from a list of 'real servers' in a TXT record.

## Why?

My initial motivation is for a Kubernetes control plane - and not use an external load balancer for the API servers, which means they can stay behind a firewall and not on the public internet, and no separate load balancing server is required; they just need to be able to make a DNS query to obtain a list of peers (to whom they already have access over a private network).

In general, this could be useful in any case where:
  * you have a set of machines on a common network
  * each machine needs to talk to one of the others, but it doesn't matter which
  * you don't want a separate load balancer
  * you can query a DNS record from the machines, and keep it up to date with the machine list from outside (e.g. CI/CD)

## How?

1. Install `ipvsadm`
1. Put `ipvs-txt-lb` somewhere that's in `$PATH`
1. Put `ipvs-txt-lb.service` in `/etc/systemd/system/` and run `systemctl enable ipvs-txt-lb`
1. Put `ipvs-txt-lb.conf` in `/etc/` and populate it
1. Populate your `TXT` record with a `;`-delimited list of servers, e.g. `alpha;beta`
1. Run `systemctl start ipvs-txt-lb`
