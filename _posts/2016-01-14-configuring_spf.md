---
layout: post
title: Configuring SPF
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
  Configure your DNS server to include SPF records for your domain.
  The only acceptable as a source of e-mail from your domain is your server.
  The SPF record should reflect this.

## Implementation:
1. Add line to /etc/bind/db.d4.sysinst.ida.liu.se:  
    ```@        IN     TXT     "v=spf1 mx -all"```  
2. Restart service:  
    ```service bind9 reload```  

## Verification:
* `dig d4.sysinst.ida.liu.se TXT` should return the above line
* Sending an email from server.d4.sysinst.ida.liu.se to e.g. a gmail should
    have a 'Received-SPF: pass' in the header.
* Sending an email from another client should have a 'Received-SPF: fail' in the
    header

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
