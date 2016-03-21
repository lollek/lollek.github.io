---
layout: post
title: Building Debian files
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Links
- [Official Manual](https://www.debian.org/doc/manuals/maint-guide/build.en.html)

## Example building sudo-1.8.11p1-2

    # Download the source files
    curl -O http://ftp.de.debian.org/debian/pool/main/s/sudo/sudo_1.8.11p1-2.dsc
    curl -O http://ftp.de.debian.org/debian/pool/main/s/sudo/sudo_1.8.11p1.orig.tar.gz
    curl -O http://ftp.de.debian.org/debian/pool/main/s/sudo/sudo_1.8.11p1-2.debian.tar.xz
    
    # Extract the tarballs:
    tar xf sudo_1.8.11p1-2.debian.tar.xz   # Creates folder sudo-1.8.11p1
    tar xf sudo_1.8.11p1.orig.tar.gz       # Creates folder debian
    
    # Move the debian file to the source
    mv debian sudo-1.8.11p1
    
    # Cd to the folder
    cd sudo-1.8.11p1
    
    # Build
    dpkg-buildpackage -us -uc

{% endcapture %}{{ data | markdownify }}
{% include panel_end.html %}
