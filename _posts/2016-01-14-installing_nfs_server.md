---
layout: post
title: Installing NFS Server
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Install the NFS Kernel Server and allow mappings from the subnet 130.236.179.88/29

## Implementation (Server-side):
- Login as root
- Install package nfs-kernel-server
- Add the following line to /etc/hosts.allow:

~~~
portmap: 130.236.179.88/255.255.255.248
~~~
- Add the following line to /etc/exports: 

~~~
/usr/local         130.236.179.88/29(ro,root_squash,subtree_check)
~~~

- Restart the services

~~~
service portmap restart
service nfs-kernel-server restart
~~~

## Implementation (Client-side):
- Login as root
- Install package nfs-common
- Attempt a mount:

~~~
mount -o ro,vers=3 -t nfs server.d4.sysinst.ida.liu.se:/usr/local /usr/local
~~~

- If it works, add the following line to /etc/fstab:

~~~
server.d4.sysinst.ida.liu.se:/usr/local       /usr/local    nfs       ro,vers=3             0 0 
~~~

## Verification:
1. Create file /usr/local/bin/hello which echoes hello.
2. Run hello on client, should echo hello.
3. Make sure it's not possible to mount from other host

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
