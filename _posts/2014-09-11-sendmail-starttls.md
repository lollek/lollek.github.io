---
layout: post
title: Sendmail @ FreeBSD - Activating SMTP AUTH (PLAIN) through STARTLS
---

{% include panel_start.html header=page.title %}

{% capture data %}
N.B. This expects a working sendmail installation with STARTTLS  

## Install cyrus-sasl
{% endcapture %}{{ data | markdownify }}
{% highlight bash %}
# install cyrus-sasl2
cd /usr/ports/security/cyrus-sasl2
make install clean
echo "pwcheck_method: saslauthd" > /usr/local/lib/sasl2/Sendmail.conf

# install cyrus-sasl2-saslauthd
cd /usr/ports/security/cyrus-sasl2-saslauthd
make install clean
echo 'saslauthd_enable="YES"' >> /etc/rc.conf
service saslauthd start
{% endhighlight %}
{% capture data %}

## Set sendmail make flags
Set the following flags in /etc/make.conf (create if it doesn't exist)
{% endcapture %}{{ data | markdownify }}
{% highlight make %}
SENDMAIL_CFLAGS=-I/usr/local/include/sasl -DSASL
SENDMAIL_LDFLAGS=-L/usr/local/lib
SENDMAIL_LDADD=-lsasl2
{% endhighlight %}
{% capture data %}

## Recompile sendmail
Did you have the source in /usr/src? Otherwise you will need to run
the following command. If you don't use RELEASE-10, you should change that.
{% endcapture %}{{ data | markdownify }}
{% highlight bash %}
svnlite checkout http://svn.freebsd.org/base/release/10.0.0/ /usr/src
{% endhighlight %}
Now recompile sendmail
{% highlight bash %}
cd /usr/src/lib/libsmutil
make cleandir && make obj && make
cd /usr/src/lib/libsm
make cleandir && make obj && make
cd /usr/src/usr.sbin/sendmail
make cleandir && make obj && make && make install
{% endhighlight %}
{% capture data %}

## Do some config switcheroo
Fetch a sendmail config if you don't already have one
{% endcapture %}{{ data | markdownify }}
{% highlight bash %}
cd /etc/mail
make
{% endhighlight %}
Now you should have a your-hostname.mc file. If you DON'T want AUTH PLAIN,
you can simply add the following lines
{% highlight bash %}
dnl set SASL options
TRUST_AUTH_MECH(`GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN')dnl
define(`confAUTH_MECHANISMS', `GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN')dnl
{% endhighlight %}
If you DO want AUTH PLAIN, add these instead
{% highlight bash %}
dnl Set SASL options
dnl Only allow AUTH LOGIN PLAIN through STARTTLS and disallow anons
dnl
define(`confAUTH_OPTIONS', `A p y')dnl
TRUST_AUTH_MECH(`GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
define(`confAUTH_MECHANISMS', `GSSAPI DIGEST-MD5 CRAM-MD5 LOGIN PLAIN')dnl
{% endhighlight %}

Now install them and restart sendmail
{% highlight bash %}
make install restart
{% endhighlight %}
{% include panel_end.html %}
