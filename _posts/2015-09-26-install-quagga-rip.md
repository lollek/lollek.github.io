---
layout: post
title: Configure Quagga with RIP
---

{% include panel_start.html header="Set up RIP routing" %}

{% capture data %}
## Description:
Install quagga on gw and configure it so traffic can be forwarded between internet and the virtual LAN

## Implementation:
### Enable ip-forwarding:
1. In /etc/sysctl.conf: Uncomment net.ipv4.ip_forward=1
2. Restart gw

### Installation:
1. Run aptitude
2. Locate and install metapackage quagga

### Configuration:
1. In /etc/quagga/daemons, set the following: <pre>
zebra=yes
ripd=yes
</pre>

2. Create /etc/quagga/zebra.conf (owned by quagga:quagga, mode 640): <pre>
hostname gw.d4.sysinst.ida.liu.se
password read_password
enable password write_password
log file /var/log/quagga/zebra.log
ip forwarding
ipv6 forwarding
</pre>

3. Create /etc/quagga/ripd.conf  (owned by quagga:quagga, mode 640): <pre>
  hostname gw.d4.sysinst.ida.liu.se
  password read_password
  enable password write_password
  log file /var/log/quagga/ripd.conf
  router rip
    version 2
    network eth1
    route 130.236.179.88/29
  !
</pre>

### Start:
`service quagga start`

## Verification:
Should be possible to ping google.com from gw, server, client-1 and client-2

## Backout:
Remove quagga through aptitude

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
