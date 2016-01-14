---
layout: post
title: Installing Spamassassin
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Implementation:
1. Login on server as root
2. Install spamassassin through aptitude
3. In /etc/default/spamassassin:<pre>
    Set ENABLED=1
  </pre>
4. Add/uncomment in /etc/spamassassin/local.cf:<pre>
    rewrite_header Subject ``*****SPAM*****``
    report_safe 0
  </pre>
6. In /etc/postfix/master.cf:<pre>
    smtp      inet  n       -       -       -       -       smtpd
      -o content_filter=spamassassin

    spamassassin unix -     n       n       -       -       pipe
      user=debian-spamd argv=/usr/bin/spamc -f -e /usr/sbin/sendmail -oi -f ${sender} ${recipient}
  </pre>
5. Restart services<pre>
    `service spamassassin start`
    `service postfix reload`
  </pre>


## Verification:
Email sent to server with body
    ``XJS*C4JDBQADN1.NSBN3*2IDNEN*GTUBE-STANDARD-ANTI-UBE-TEST-EMAIL*C.34X``
    should have subject rewritten to ``****SPAM**** $header``

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
