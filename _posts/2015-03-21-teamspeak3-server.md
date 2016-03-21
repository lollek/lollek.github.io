---
layout: post
title: Setup Teamspeak3 Server
---

{% include panel_start.html header=page.title %}

{% capture data %}

- Download tarball from www.teamspeak.com
- Untar to /home/teamspeak
- Add user/group teamspeak
- Run the init as teamspeak

~~~
sudo su - teamspeak
./ts3server_minimal_runscript.sh createinifile=1
~~~

- Configure ts3server.ini
- Make sure you can start it (then ^C to kill it)

~~~
./ts3server_startscript.sh start
~~~

- Create /etc/systemd/system/teamspeak.service

~~~
[Unit]
Description=Teamspeak server
After=network.target

[Service]
User=teamspeak
Group=teamspeak
Type=forking
PIDFile=/home/teamspeak/ts3server.pid
ExecStart=/home/teamspeak/ts3server_startscript.sh start

[Install]
WantedBy=multi-user.target
~~~

- Test it thought systemctl and then enable it

~~~
sudo systemctl start teamspeak.service
sudo systemctl enable teamspeak.service
~~~

{% endcapture %}{{ data | markdownify }}
{% include panel_end.html %}
