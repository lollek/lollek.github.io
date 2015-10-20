---
layout: post
title: Expecting ssh login
---

{% include panel_start.html header=page.title %}

{% capture data %}
# Clean
```
#! /usr/bin/env expect
set host [lindex $argv 0];
set user [lindex $argv 1];
set passwd [lindex $argv 2];

set timeout 5
spawn ssh $user@$host

expect {
  "yes/no" {
    send "yes\r"
    exp_continue
  }

  "password:" {
    send "$passwd\r"
    exp_continue
  }

  "$user@$host:~? " {
    exit 0
  }

  timeout {
    exit 2
  }
}

exit 1
```

# Wrapped inside bash
```
#! /usr/bin/env bash

assert_ssh_login() {
  local host="${1}"
  local user="${2}"
  local passwd="${3}"

  local details="$(echo -e "\
    set timeout 5              \n\
    spawn ssh ${user}@${host}  \n\
    expect {                   \n\
      \"yes/no\" {             \n\
        send \"yes\r\"         \n\
        exp_continue           \n\
      }                        \n\
                               \n\
      \"password:\" {          \n\
        send \"${passwd}\r\"   \n\
        exp_continue           \n\
      }                        \n\
                               \n\
      \"${user}@${host}:~? \" {\n\
        exit 0                 \n\
      }                        \n\
                               \n\
      timeout {                \n\
        exit 2                 \n\
      }                        \n\
    }                          \n\
                               \n\
    exit 1                     \n\
    " | expect)"

  case "${?}" in
    "0") echo "Can login as ${user} on ${host}" ;;
    "1") echo "${user} can not login on ${host}. Details: ${details}" ;;
    "2") echo "${user} can not login on ${host} (Timeout). Details: ${details}" ;;
     * ) echo "${user} can not login on ${host} (Error ${?}): Details: ${details}" ;;
  esac
}
```
{% endcapture %}{{ data | markdownify}}
{% include panel_end.html %}
