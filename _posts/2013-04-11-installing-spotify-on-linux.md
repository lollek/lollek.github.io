---
layout: post
title: Installing Spotify on Linux
---
***NB - These instructions are old and probably don't work anymore***

Step 1: Add spotify-sources to apt-repo
------
{% highlight bash %}
echo "deb http://repository.spotify.com stable non-free" >> /etc/apt/sources.list
{% endhighlight %}

Step 2: Add key
------
{% highlight bash %}
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 94558F59
{% endhighlight %}

Step 3: Fetch updates
------
{% highlight bash %}
apt-get update
{% endhighlight %}

Step 4: Add this stupid old dependency
------
This might not be needed anymore / on other distros. Just do it if the next step doesn't work
{% highlight bash %}
wget snapshot.debian.org/archive/debian/20110406T213352Z/pool/main/o/openssl098/libssl0.9.8_0.9.8o-6_amd64.deb
dpkg -i libssl0.9.8_0.9.8o-6_amd64.deb
{% endhighlight %}

Step 5: Install spotify
------
{% highlight bash %}
apt-get install spotify-client
{% endhighlight %}

Step 6: Run spotify!
------
{% highlight bash %}
spotify
{% endhighlight %}
