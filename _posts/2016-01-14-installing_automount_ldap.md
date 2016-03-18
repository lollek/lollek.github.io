---
layout: post
title: Installing Automount on System (using LDAP)
---

{% include panel_start.html header=page.title %}

{% capture data %}
## Description:
Setup autofs on machines client-1 and client-2, which both uses ldap (how to install is described in a previous note)

## Implementation:
1. Install package autofs5-ldap
2. Create autofs.ldif with the following data:<pre>
    dn: cn=autofs,cn=schema,cn=config
    objectClass: olcSchemaConfig
    cn: autofs
    olcAttributeTypes: {0}( 1.3.6.1.1.1.1.25 NAME 'automountInformation' DESC 'Inf
     ormation used by the autofs automounter' EQUALITY caseExactIA5Match SYNTAX 1.
     3.6.1.4.1.1466.115.121.1.26 SINGLE-VALUE )
    olcObjectClasses: {0}( 1.3.6.1.1.1.1.13 NAME 'automount' DESC 'An entry in an 
     automounter map' SUP top STRUCTURAL MUST ( cn $ automountInformation $ object
     class ) MAY description )
    olcObjectClasses: {1}( 1.3.6.1.4.1.2312.4.2.2 NAME 'automountMap' DESC 'An gro
     up of related automount objects' SUP top STRUCTURAL MUST ou )
  </pre>
3. Add the ldif to ldap:<pre>
    ldapadd -Y EXTERNAL -D cn=admin,(..) -W -f autofs.ldif
  </pre>
4. Create ldif with automount information and a test-user:<pre>
    n: ou=auto.master,ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
    ou: auto.master
    objectClass: top
    objectClass: automountMap

    dn:
    cn=/home,ou=auto.master,ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
    cn: /home
    objectClass: top
    objectClass: automount
    automountInformation:
    ldap:ou=auto.home,ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se --timeout=60 --ghost

    dn: ou=auto.home,ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
    ou: auto.home
    objectClass: top
    objectClass: automountMap

    dn: cn=ollehome1,ou=auto.home,ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se
    cn: ollehome1
    objectClass: top
    objectClass: automount
    automountInformation: -fstype=nfs,nfsvers=3,rw,soft,intr,exec server.d4.sysinst.ida.liu.se:/export/home1/&
  </pre>
5. Add ldif to ldap:<pre>
    ldapadd -D cn=admin,(..) -W -f autodata.ldif
  </pre>
6. Edit/enable the following lines in /etc/default/autofs:<pre>
    LOGGING="verbose"
    LDAP_URI="ldap://server.d4.sysinst.ida.liu.se"
    SEARCH_BASE="ou=automount,dc=d4,dc=sysinst,dc=ida,dc=liu,dc=se"
    MAP_OBJECT_CLASS="automountMap"
    ENTRY_OBJECT_CLASS="automount"
    MAP_ATTRIBUTE="ou"
    ENTRY_ATTRIBUTE="cn"
    VALUE_ATTRIBUTE="automountInformation"
  </pre>
7. Add automount to /etc/nsswitch.conf:<pre>
    automount:      files ldap
  </pre>
8. Restart autofs:<pre>
    `service autofs restart`
  </pre>

## Verification:
Should be able to login on both hosts with ollehome1

{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}