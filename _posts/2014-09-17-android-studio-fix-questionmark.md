---
layout: post
title: Android Studio Questionmark Instead of Phone Name
---

{% include panel_start.html header=page.title %}

{% capture data %}
If you get questionmarks instead of phone name when trying to run your app, you
can fix it with the following code:

    #! /usr/bin/env bash
    path="/opt/android-studio"  # Path where Android Studio is installed
    set -e
    
    "$path/sdk/platform-tools/adb" kill-server
    sudo "$path/sdk/platform-tools/adb" start-server
    "$path/sdk/platform-tools/adb" devices


If it doesn't work the first time, try a few more times

{% endcapture %}{{ data | markdownify }}
{% include panel_end.html %}
