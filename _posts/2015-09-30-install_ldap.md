---
layout: post
title: Install and configure LDAP on server
---

{% include panel_start.html header="Install and configure LDAP on server" %}

{% capture data %}
## Description:
Configure LDAP to the domain d4.sysinst.ida.liu.se

## Implementation:
- Login to server as root
- Install the packages slapd, ldap-utils and migrationtools
- Set /etc/ldap/ldap.conf so contain the following: 

~~~
#
# LDAP Defaults
#

# See ldap.conf(5) for details
# This file should be world readable but not world writable.

BASE    dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
URI     ldapi:///
~~~

- In /etc/migrationtools/migrate_common.ph, locate and set the following: 

~~~
$DEFAULT_MAIL_DOMAIN = "d4.sysinst.ida.liu.se";
$DEFAULT_BASE = "dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se";
~~~

- cd /usr/share/migrationtools
- ./migrate_all_online.sh
- Install package libnss-ldapd through aptitude, use all services
- Edit /etc/nsswitch.conf, change all "compat" to "files"
- In /etc/pam.d/common-session, add line to end:

~~~
session required        pam_mkhomedir.so skel=/etc/skel umask=0022
~~~

## Verification:
- Create /root/olle.ldif

~~~
root@server:~# cat user.ldif
dn: uid=olle,ou=People,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
objectClass: top
objectClass: account
objectClass: posixAccount
objectClass: shadowAccount
cn: olle
uid: olle
uidNumber: 10000
gidNumber: 100
homeDirectory: /home/olle
loginShell: /bin/bash
gecos: olle
userPassword: {crypt}x
shadowLastChange: 0
shadowMax: 99999
shadowWarning: 0
~~~

- `ldapadd -W -D "cn=admin,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se" -f olle.ldif`
- `ldappasswd -s mypassword -W -D
  "cn=admin,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se"
  "uid=olle,ou=People,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se"`

- Running `getent passwd` should now return olle as a user with uid 10000
{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
