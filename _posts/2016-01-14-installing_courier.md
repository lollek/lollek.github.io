---
layout: post
title: Installing Courier
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Install courier on server

## Implementation:
1. Login as root
2. Install fam and courier-imap-ssl through aptitude
    1. Create directories for web-based administration? **No**
3. In /etc/postfix/main.cf, add line:<pre>
    home_mailbox = Maildir/
  </pre>
4. Restart service<pre>
    `service postfix restart`
  </pre>
5. You may need to run *maildirmake Maildir* when in homedir?

## Verification:
1. Should be able to login as root on imap from outside of LAN
2. Should be able to login as root on imap with STARTTLS from outside of LAN

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
