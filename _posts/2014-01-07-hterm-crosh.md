---
layout: post
title: Crosh / HTerm
---

{% include panel_start.html header="Bad charmap encoding (sending, not reading)" %}
{% capture data %}
1. Go to **nassh_preferences_editor.html** under chrome extensions
2. Change **send-encoding** from **utf-8** to **raw**
{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
