<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>-------------------------- Import MIB to Zabbix --------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>
<br>

_Optional to Import MIB to Zabbix ( External Mibs )_
_Path to Local Mibs Folder : zabbix-docker/zbx_env/var/lib/zabbix/mibs_


## 1. Add Automatically Download MIB Files with Python Script

__Step 1 Find correct name of the MIB file__
Go to the MIB database, search for your MIB, and note down its correct name without any extension. In my example, that would be F5-BIGIP-SYSTEM-MIB.
MIB database : https://mibbrowser.online/mibdb_search.php

__Step 2 Download the python script__
Download the ‘ZabbixDownloadMib.py’ script from Github use this 

__Command:__

~~~
curl -O https://raw.githubusercontent.com/TheAldin/scripts/main/ZabbixDownloadMib.py
~~~

__Step 3 Run the script__

Run the script like this: "sudo python3 ZabbixDownloadMib.py [MIB name] [MIB DIR PATH]"

__Command:__

~~~
sudo python3 ZabbixDownloadMib.py F5-BIGIP-SYSTEM-MIB /zabbix-docker/zbx_env/var/lib/zabbix/mibs
~~~

__( Output )__
~~~
Downloaded F5-BIGIP-SYSTEM-MIB.txt successfully.
…
~~~

__Step 4 Restart the Zabbix server__

~~~
systemctl restart zabbix-server
~~~

__OR__ 

~~~
docker compose –profile full -f ./file.yaml restart
~~~


## 2. Enable MIB Support for SNMP Traps ( Snmptrapd )

_You may have correct MIB files in the MIB directory ("zabbix-docker/zbx_env/var/lib/zabbix/mibs") but Snmptrapd will not load them by default!_

To enable this, you need to edit the Snmptrapd service with the option "-m ALL" which means it will load and make available all SNMP MIB modules configured on the system:

__Step 1 Execute to container Use command :__

~~~
docker exec -u root -ti <CONTAINER-ID> bash
~~~

__Step 2 Use command to edit file__

__: Note : If not have nano use this command first__

~~~
apt-get update
~~~

__And__

~~~
apt install nano
~~~

__Command To edit file__

~~~
nano /usr/lib/systemd/system/snmptrapd.service
~~~


__( In this File : )__
~~~
Add the option ‘-m ALL’ so that the ‘ExecStart’ line looks like this (note that this line differs on RHEL from Debian-based OS):
# If You use Debian / Ubuntu / Raspberry Pi OS
ExecStart=/usr/sbin/snmptrapd -LOw -f -m ALL -p /run/snmptrapd.pid
~~~

__Step 3 Save and exit file (ctrl+x, followed by y and enter)__
_then reload the daemon and restart the Snmptrapd service afterward _

__Command :__

~~~
systemctl daemon-reload
~~~

__And__

~~~
systemctl restart snmptrapd
~~~


