---
layout: post
title: Installing Spotify on Linux
---

{% include panel_start.html header=page.title %}

{% capture data %}
These instructions are old and probably don't work anymore

    # Step 1: Add spotify-sources to apt-repo
    echo "deb http://repository.spotify.com stable non-free" >> /etc/apt/sources.list

    # Step 2: Add key
    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 94558F59

    # Step 3: Fetch updates
    apt-get update

    # Step 4: Add this stupid old dependency
    # This might not be needed anymore / on other distros. Just do it if the next step doesn't work
    wget snapshot.debian.org/archive/debian/20110406T213352Z/pool/main/o/openssl098/libssl0.9.8_0.9.8o-6_amd64.deb
    dpkg -i libssl0.9.8_0.9.8o-6_amd64.deb

    # Step 5: Install spotify
    apt-get install spotify-client

    # Step 6: Run spotify!
    spotify

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
