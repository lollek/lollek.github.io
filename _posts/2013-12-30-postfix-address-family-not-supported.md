---
layout: post
title: Postfix - Address Family Not Supported
---

Issue during postfix startup
-------
{% highlight bash %}
Stopping Postfix Mail Transport Agent: postfixpostmulti: warning:
inet_protocols: disabling IPv6 name/address support: Address family not
supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postfix: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
.
Starting Postfix Mail Transport Agent: postfixpostmulti: warning:
inet_protocols: disabling IPv6 name/address support: Address family not
supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postmulti: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
postfix: warning: inet_protocols: disabling IPv6 name/address support: Address
family not supported by protocol
.
{% endhighlight %}

Solution
-------
{% highlight bash %}
sudo postconf -e 'inet_protocols = ipv4'
sudo /etc/init.d/postfix restart
{% endhighlight %}
