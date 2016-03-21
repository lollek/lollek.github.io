---
layout: post
title: Installing Spamassassin
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Implementation:
- Login on server as root
- Install spamassassin through aptitude
- In /etc/default/spamassassin:

~~~
Set ENABLED=1
~~~

- Add/uncomment in /etc/spamassassin/local.cf:

~~~
rewrite_header Subject *****SPAM*****
report_safe 0
~~~

- In /etc/postfix/master.cf:

~~~
smtp      inet  n       -       -       -       -       smtpd
  -o content_filter=spamassassin

spamassassin unix -     n       n       -       -       pipe
  user=debian-spamd argv=/usr/bin/spamc -f -e /usr/sbin/sendmail -oi -f ${sender} ${recipient}
~~~

- Restart services

~~~
service spamassassin start
service postfix reload
~~~

## Verification:
Email sent to server with body
    ``XJS*C4JDBQADN1.NSBN3*2IDNEN*GTUBE-STANDARD-ANTI-UBE-TEST-EMAIL*C.34X``
    should have subject rewritten to ``****SPAM**** $header``

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
