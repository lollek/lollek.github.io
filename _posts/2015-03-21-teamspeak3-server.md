---
layout: post
title: Setup Teamspeak3 Server
---

{% include panel_start.html header=page.title %}

{% capture data %}
1. Download tarball from www.teamspeak.com
2. Untar to /home/teamspeak
3. Add user/group teamspeak
4. Run the init as teamspeak <pre>
sudo su - teamspeak
./ts3server_minimal_runscript.sh createinifile=1
</pre>
5. Configure ts3server.ini
6. Make sure you can start it (then ^C to kill it) <pre>./ts3server_startscript.sh start</pre>
7. Create /etc/systemd/system/teamspeak.service <pre>
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
</pre>
8. Test it thought systemctl and then enable it <pre>
sudo systemctl start teamspeak.service
sudo systemctl enable teamspeak.service
</pre>
{% endcapture %}{{ data | markdownify }}
{% include panel_end.html %}
