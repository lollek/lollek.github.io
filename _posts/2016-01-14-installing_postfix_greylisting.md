---
layout: post
title: Installing Postfix Greylisting
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Add greylisting to postfix through postgrey

## Implementation:
1. Login to server as root
2. Install postgrey through aptitude
3. Append to /etc/postgrey/whitelist-clients:<pre>
    d4.sysinst.ida.liu.se
  </pre>
4. Restart service: <pre>
    `service postgrey reload`
  </pre>
5. Add to/edit in /etc/postfix/main.cf:<pre>
    smtpd_recipient_restrictions =
        permit_mynetworks
        reject_unauth_destination
        check_policy_service inet:127.0.0.1:10023
  </pre>
6. Restart service:<pre>
    `service postfix reload`
  </pre>

## Verification:
* Email from any local host should not be greylisted
* Email from unknown address should be greylisted

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
