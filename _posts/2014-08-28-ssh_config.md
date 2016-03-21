---
layout: post
title: .ssh/config
---

{% include panel_start.html header=page.title %}

{% capture data %}
To make it easier to ssh (and enable autocomplete), create a ~/.ssh/config like
this:

    Host *
      SendEnv LANG LC_*
      HashKnownHosts yes
      GSSAPIAuthentication yes
      PreferredAuthentications publickey,password
      IdentityFile ~/.ssh/id_rsa
      ServerAliveInterval 60
      Compression      yes
      CompressionLevel 4
    Host github
      Hostname github.com
      User git


and you will always use user git when you ssh to github
{% endcapture %}{{ data | markdownify }}
{% include panel_end.html %}


