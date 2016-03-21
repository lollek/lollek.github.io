---
layout: post
title: Installing Postfix on System
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Criterias:

1. Accept mail from any SMTP server in the world.
2. Be able to send mail to any SMTP server in the world.
3. Not accept mail for any other destinations than your domain.
4. Meet the requirements of RFC 2821, section 4.5.1 concerning the postmaster address.
5. Should rewrite local usernames to real names through LDAP lookups
6. Forwarded email from satellite systems should have their FQDN rewritten to
   this server's

## Implementation (Main server):
- Login on server as root:
- Install postfix through aptitude
    - Select type **Internet Site**
    - Set System Mail Name to **d4.sysinst.ida.liu.se**
- In /etc/postfix/main.cf, add the following lines:

~~~
mynetworks = 127.0.0.0/8 130.236.179.88/29 [::ffff:127.0.0.0]/104 [::1]/128
masquerade_domains = $mydomain
local_header_rewrite_clients = permit_mynetworks
sender_canonical_maps = ldap:/etc/postfix/canonical_sender
recipient_canonical_maps = ldap:/etc/postfix/canonical_recipent
~~~

- Create /etc/postfix/canonical_sender:

~~~
search_base = ou=People,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
server_host = server.d4.sysinst.ida.liu.se
bind = no
version = 3
domain = d4.sysinst.ida.liu.se
query_filter = uid=%u
result_attribute = mail
~~~

- Create /etc/postfix/canonical_recipent:

~~~
search_base = ou=People,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
server_host = server.d4.sysinst.ida.liu.se
bind = no
version = 3
domain = d4.sysinst.ida.liu.se
query_filter = mail=%s
result_attribute = uid
~~~

- Set MX record. Add line to /etc/bind/db.d4.sysinst.ida.liu.se:

~~~
@        IN     MX      10 server.d4.sysinst.ida.liu.se.
~~~

- Restart the services:

~~~
service postfix restart
service bind9 restart
~~~

## Implementation (Satellite Systems/Clients):
1. Install postfix through aptitude
    1. Select type **Satellite System**
    2. Set System Mail Name to **d4.sysinst.ida.liu.se**
    3. Set SMTP Relay Host to **server.d4.sysinst.ida.liu.se**

## Verification:
1. Test that the server can receive email from generic server outside of LAN
2. Test that the server can send email to generic server outside of LAN
3. Test that the server does not accept email for other domains than
    d4.sysinst.ida.liu.se
4. Test that all clients can send email to
    *@d4.sysinst.ida.liu.se and they should end up on server
5. Test that all clients do not have smtp open to LAN/WAN

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
