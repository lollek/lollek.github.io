---
layout: post
title: Attach to LDAP on server
---

{% include panel_start.html header="Connect clients to server LDAP" %}

{% capture data %}
## Description:
Clients should use server.d4.sysinst.ida.liu.se for LDAP authentication

## Implementation:
1. Login to computers as root
2. Install package libnss-ldapd
3. Set LDAP server URI: ldap://server.d4.sysinst.ida.liu.se
4. Set LDAP server search base: dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
5. Set LDAP server to use all services
6. In /etc/nsswitch.conf, replace compat with files
7. In /etc/pam.d/common-session, add line to end:

~~~
session required        pam_mkhomedir.so skel=/etc/skel umask=0022
~~~

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
