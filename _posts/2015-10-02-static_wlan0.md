---
layout: post
title: Configure static wlan0 in /etc/network/interfaces
---

{% include panel_start.html header=page.title %}

{% capture data %}
Get wpa-psk by running `wpa_passphrase MyNetwork MyPassphrase`.  
/etc/network/interfaces should be chmod 0600 if you put the wpa-psk there


    source /etc/network/interfaces.d/*
    
    # The loopback network interface
    auto lo wlan0
    iface lo inet loopback
    
    iface wlan0 inet dhcp
      wpa-ssid MyNetwork
      wpa-psk a2d024861ef90117c47083c9252d1e9c107c7cc6ab938cd08349c9192d444d2f

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
