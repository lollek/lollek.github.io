---
layout: post
title: Installing Courier
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Install courier on server

## Implementation:
- Login as root
- Install fam and courier-imap-ssl through aptitude
    - Create directories for web-based administration? **No**
- In /etc/postfix/main.cf, add line:

~~~
home_mailbox = Maildir/
~~~

- Restart service

~~~
service postfix restart
~~~

- You may need to run *maildirmake Maildir* when in homedir?

## Verification:
1. Should be able to login as root on imap from outside of LAN
2. Should be able to login as root on imap with STARTTLS from outside of LAN

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
