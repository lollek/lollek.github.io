---
layout: post
title: Building Debian files
---


<div class="panel panel-default">
  <div class="panel-heading">
    <h1 class="panel-title">Links</h1>
  </div>
  <div class="panel-body">
    <div class="list-group">
      <a class="list-group-item"
      href="https://www.debian.org/doc/manuals/maint-guide/build.en.html">Debian
      Link</a>
    </div>
  </div>
</div>

<div class="panel panel-default">
  <div class="panel-heading">
    <h1 class="panel-title">Example building <a
    href="https://packages.debian.org/sid/sudo">sudo-1.8.11p1-2</a></h1>
  </div
  <div class="panel-body">
    <pre>
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
    </pre>
  </div>
</div>
