---
layout: post
title: Installing NFS Server
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Install the NFS Kernel Server and allow mappings from the subnet 130.236.179.88/29

## Implementation (Server-side):
1. Login as root
2. Install package nfs-kernel-server
3. Add the following line to /etc/hosts.allow:<pre>
    portmap: 130.236.179.88/255.255.255.248
   </pre>
4. Add the following line to /etc/exports: <pre>
    /usr/local         130.236.179.88/29(ro,root_squash,subtree_check)
  </pre>

5. Restart the services<pre>
    `service portmap restart`
    `service nfs-kernel-server restart`
  </pre>

## Implementation (Client-side):
1. Login as root
2. Install package nfs-common
3. Attempt a mount: <pre>
    `mount -o ro,vers=3 -t nfs server.d4.sysinst.ida.liu.se:/usr/local /usr/local`
  </pre>
4. If it works, add the following line to /etc/fstab:<pre>
    server.d4.sysinst.ida.liu.se:/usr/local       /usr/local    nfs       ro,vers=3             0 0 
  </pre>

## Verification:
1. Create file /usr/local/bin/hello which echoes hello.
2. Run hello on client, should echo hello.
3. Make sure it's not possible to mount from other host

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
