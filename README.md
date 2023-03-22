# DynV6 update script 

Based on dynv6 update script from Julian Kornberger (https://gist.github.com/corny/7a07f5ac901844bd20c9)

### Instalation Instructions

**Clonning directory**

~~~bash
git clone https://github.com/py1tcm/dynv6.git
cd dynv6

~~~

**Files configuration**

If you are not running on "pi" user, you need to modify these files:

On dynv6.service modify username on <code>WorkingDirectory</code>, <code>ExecStart</code> and <code>User</code> fields to your username

~~~bash
[Unit] 
Description=Dynv6 update service 
After=network.target 

[Service] 
Type=oneshot 
WorkingDirectory=/home/pi/dynv6 
ExecStart=/home/pi/dynv6/dynv6.command 
SyslogIdentifier=Dynv6 
User=pi 

[Install]
WantedBy=multi-user.target

~~~

On dynv6.command modify directory path <code>/home/pi/dynv6/dynv6.sh</code> to your username

<code>your-authentication-token</code> to your token id provided by DynV6

<code>your-host.dynv6.net</code> to your host provided by DynV6

~~~bash
#!/bin/sh

token=<your-authentication-token> /home/pi/dynv6/dynv6.sh <your-host.dynv6.net>

~~~

On dynv6 directory has three dynv6.sh files, dynv6_dual.sh is for dual stack (ipv4 and ipv6) networks and update ip on both networks, dynv6_ipv4.sh update ipv4 netowrk only and dynv6_ipv6.sh update ipv6 network only

After define your network type, copy seleted file to dynv6.sh

**Example:**

~~~bash
cp dynv6_dual.sh dynv6.sh

~~~

### Setting files to be executable

~~~bash
chmod +x dynv6.command
chmod +x dynv6.sh

~~~

### Automatic start and update host

Copy dynv6.service and dynv6.timer to systemd directory

~~~bash
sudo cp dynv6.service /etc/systemd/system
sudo cp dynv6.timer /etc/systemd/system

sudo systemctl enable dynv6.service
sudo systemctl enable dynv6.timer
sudo systemctl start dynv6.service
sudo systemctl start dynv6.timer

~~~


### View output log

~~~bash
sudo journalctl -u dynv6.service -f -n

~~~
