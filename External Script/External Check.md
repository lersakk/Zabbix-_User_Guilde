![User Mannal (14)](https://github.com/lersakk/ZabbixUserManual/assets/106941759/53c0ee98-f0c9-4e97-9fa7-0410b84ffee8)

<h1>Example of making an External Script to monitor the efficiency of querying domain name information on the DNS Server system.</h1>

## 1. Use the command to go to the directory where the external script files are stored.

__Step 1 Use the command to go to the directory where the external script files are stored.__

__Command :__

~~~
cd zabbix-docker/zbx_env/usr/lib/zabbix/externalscripts/
~~~

__Step 2 Use the command to create the Script file.__

__Command :__

~~~
nano dns_query_time.sh
~~~

__Step 3 Add information to the file as follows.__

__Script :__
~~~
#!/bin/bash

# Run dig command and capture the output
dig_output=$(dig google.com)

# Extract query time using grep and awk
query_time=$(echo "$dig_output" | grep "Query time:" | awk '{print $4}')

# Print the query time
echo "$query_time"
~~~

__Step 4 Press “Ctrl+x” and type “Y” to save the file.__

__Use commands to grant permissions to the Script file.__

__Command :__
~~~
sudo chmod 744 dns_query_time.sh
~~~

## 2. Installing the dnsutils package on the Container
_To monitor the performance of querying domain name information on the DNS Server in Docker containers, it is necessary to use the dig command and the dnsutils package._

__Step 1 Use this command to display a list of active containers.__  
  
__Command :__
~~~
sudo docker ps
~~~

__Step 2 Execute into the Container__

__Command :__

~~~
sudo docker exec -u root -ti [CONTAINER-ID] bash
~~~

__Step 3 After entering the Container, use the command to install dnsutils and wait for the installation to complete.__

__Command :__
~~~
apt-get update
~~~

__Command :__
~~~
apt install dnsutils 
~~~

## 3. Configuring the display on the Zabbix Web Interface

__Step 1 Go to the Zabbix Web Interface page to configure it.__

__Step 2 Go to the Data collection -> Templates -> Create Templates tab and enter the following information:__

~~~
Template name : External Check
Template groups : Template groups (Group ที่ต้องการ) 
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/70b949d6-61cb-4133-b435-d6ab2594a3cc" width="100%">

#
__Step 3 Go to the template you created -> go to Create Item and enter the following information:__
~~~
Name : DNS Query Time
Type : Zabbix agent
Key : dns_query_time.sh (Name of the file created)
Type of information : Numeric (float)
Update interval : 1m
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/45eaa6a0-21b0-46e7-878a-ae181f4c99a7" width="100%">

#

__Step 4 Press “Add”__

__Step 5 Add a template to the Host for monitoring.__ ([How to Add Template to Hosts](https://github.com/lersakk/ZabbixUserManual/blob/main/SNMP%20Traps.md#%E0%B8%AA%E0%B8%A3%E0%B9%89%E0%B8%B2%E0%B8%87%E0%B9%80%E0%B8%97%E0%B8%A1%E0%B9%80%E0%B8%9E%E0%B8%A5%E0%B8%95%E0%B8%AA%E0%B8%B3%E0%B8%AB%E0%B8%A3%E0%B8%B1%E0%B8%9A-snmp-traps))

