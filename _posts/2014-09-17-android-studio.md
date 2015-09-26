---
layout: post
title: Android Studio
---

{% include panel_start.html header="Installation" %}

{% capture data %}
1. Download [android studio](https://developer.android.com/sdk/installing/studio.html)
2. Untar it to /opt/android-studio
3. Symlink /opt/android-studio/bin/studio.sh to /usr/local/bin/android-studio
4. Download [oracle's java](http://www.oracle.com/technetwork/java/javase/downloads/index.html)
5. Untar it to /opt/oracle-java
6. (Ubuntu) Symlink /opt/oracle-java to /usr/lib/jvm/default-java
7. (Ubuntu) Symlink /opt/oracle-java/bin/java to /usr/bin/java

## If you have 64-bit
{% endcapture %}{{ data | markdownify }}
{% highlight bash %}
# On ubuntu
sudo apt-get install lib32z1 lib32ncurses5 lib32bz2-1.0 lib32stdc++6
{% endhighlight %}
{% include panel_end.html %}

{% include panel_start.html header="Questionmarks instead of phone name" %}
If you get questionmarks instead of phone name when trying to run your app, you
can fix it with the following code:
{% highlight bash %}
#! /usr/bin/env bash
path="/opt/android-studio"  # Path where Android Studio is installed
set -e

"$path/sdk/platform-tools/adb" kill-server
sudo "$path/sdk/platform-tools/adb" start-server
"$path/sdk/platform-tools/adb" devices
{% endhighlight %}
If it doesn't work the first time, try a few more times

{% include panel_end.html %}
