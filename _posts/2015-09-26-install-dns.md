---
layout: post
title: Install and Configure DNS
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Need to install DNS on server with the following configuration:

1. It must respond authoritatively to all non-recursive queries for names in the zones it is authoritative for.
2. It must respond to all recursive queries from the hosts on its own network.
3. It must not respond to any recursive queries from any outside host (i.e.  host not on its own network).
4. Apart from the queries in (1), it should not respond to any queries from any outside host.
5. It must contain valid zone data for its zone(s).
6. The cache parameters must be chosen sensibly
7. It must not be susceptible to the standard cache poisoning attacks. See http://www.kb.cert.org/vuls/id/800113 for details. Test its DNS server using porttest.dns-oarc.net (see http://www.dns-oarc.net/oarc/services/porttest).

It should have the following normal zone:

- zone d4.systinst.ida.liu.se
- D4-router.lab-2 should have IP 130.236.179.89 and FQDN gw.d4.sysinst.ida.liu.se
- D4-server.lab-2 should have IP 130.236.179.90 and FQDN server.d4.sysinst.ida.liu.se
- D4-client-1.lab-2 should have IP 130.236.179.91 and FQDN client-1.d4.sysinst.ida.liu.se
- D4-client-2.lab-2 should have IP 130.236.179.92 and FQDN client-2.d4.sysinst.ida.liu.se


## Implementation:
1. Login with the root account
2. Run aptitude
3. Update package list
4. Locate and install bind9 and dnsutils

5. Copy /etc/bind/named.conf.local to /etc/bind/named.conf.local.change.0009
6. Add data to /etc/bind/named.conf.local: <pre>
    zone "d4.sysinst.ida.liu.se" {
      type master;
      file "/etc/bind/db.d4.sysinst.ida.liu.se";
    };

    zone "db.88-29.179.236.130.in-addr.arpa" {
      type master;
      file "/etc/bind/db.88-29.179.236.130.in-addr.arpa";
    };
</pre>

6. Create /etc/bind/db.d4.sysinst.ida.liu.se with the following data: <pre>
    $TTL    1H
    @        IN      SOA     server.d4.sysinst.ida.liu.se.
    hostmaster.d4.sysinst.ida.liu.se. (
                         2015092301
                                 1H
                                10M
                                 1D
                                 1H )
    ;
    @        IN      NS      server.d4.sysinst.ida.liu.se.

    gw       IN      A       130.236.179.89
    server   IN      A       130.236.179.90
    client-1 IN      A       130.236.179.91
    client-2 IN      A       130.236.179.92
</pre>
7. Create /etc/bind/db.88-29.179.236.130.in-addr.arpa with the following data: <pre>
    $TTL    1H
    @       IN      SOA     server.d4.sysinst.ida.liu.se.
    hostmaster.d4.sysinst.ida.liu.se. (
                         2015092301
                                 1H
                                10M
                                 1D
                                 1H)
    ;
    @       IN      NS      server.d4.sysinst.ida.liu.se.
    89      IN      PTR     gw.d4.sysinst.ida.liu.se.
    90      IN      PTR     server.d4.sysinst.ida.liu.se.
    91      IN      PTR     client-1.d4.sysinst.ida.liu.se.
    92      IN      PTR     client-2.d4.sysinst.ida.liu.se.
</pre>

8. Move /etc/bind/named.conf.options to /etc/bind/named.conf.options.change.0009
9. Create /etc/bind/named.conf.options with following data: <pre>
    acl lan { 127.0.0.1; 130.236.179.88/29; };

    options {
      directory "/var/cache/bind";
      auth-nxdomain no;    # conform to RFC1035
      forwarders { 130.236.178.1; };
      allow-transfer { none; };
      allow-query { any; };
      allow-recursion { lan; };
      dnssec-validation yes;
    };
    include "/etc/bind/bind.keys";
</pre>
10. Run `server bind reload`

## Verification:
- Run `named-checkzone d4.sysinst.ida.liu.se /etc/bind/db.d4.sysinst.ida.liu.se` and verify there is an OK

- Run `dig org. SOA +dnssec @localhost.` It should resolve and have an 'ad' flag.
- Run `dig google.com @localhost.` It should resolve

## Backout:
  Uninstall bind9 through aptitude
{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
