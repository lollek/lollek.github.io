---
layout: post
title: Installing Postfix Greylisting
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Add greylisting to postfix through postgrey

## Implementation:
- Login to server as root
- Install postgrey through aptitude
- Append to /etc/postgrey/whitelist-clients:

~~~
d4.sysinst.ida.liu.se
~~~

- Restart service:

~~~
service postgrey reload
~~~

- Add to/edit in /etc/postfix/main.cf:

~~~
smtpd_recipient_restrictions =
    permit_mynetworks
    reject_unauth_destination
    check_policy_service inet:127.0.0.1:10023
~~~

- Restart service:

~~~
service postfix reload
~~~

## Verification:
* Email from any local host should not be greylisted
* Email from unknown address should be greylisted

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
