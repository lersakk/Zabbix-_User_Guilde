# ZabbixUserManual
![zabbix](https://github.com/lersakk/ZabbixUserManual/assets/136166133/73df118a-760a-41a8-b155-0ead08eb73a0)


## Software Requirements
*  [Oracle VM VirtualBox](https://download.virtualbox.org/virtualbox/7.0.12/VirtualBox-7.0.12-159484-Win.exe)
*  [Ubuntu Server](https://ubuntu.com/download/server)
*  [Zabbix Appliance](https://cdn.zabbix.com/zabbix/appliances/stable/6.4/6.4.10/zabbix_appliance-6.4.10-ovf.tar.gz)

  
  
# How To Install 
## Step 1: Install SNMP packages
First, we need to install a collection of packages associated with the SNMP protocol and other:

~~~
yum install net-tools
~~~
~~~
ifconfig eth1 up
~~~
~~~
dhclient eth1
~~~
~~~
yum install nano
~~~
~~~
yum install -y net-snmp-utils net-snmp
~~~

## Step 2: Configure Snmptrapd Service
Edit the snmptrapd.conf configuration file using the following command:

~~~
nano  /etc/snmp/snmptrapd.conf
~~~

And make the necessary changes so that the final configuration resembles this (you can delete everything else):

~~~
# Disables authorization for SNMP v1 and v2 traps, allowing all incoming traps without authentication.
disableAuthorization yes

# Creates two SNMPv3 user with authentication and encryption for testing purposes
createUser -e 0x80000009030038FD myuser MD5 authpass AES mypassword
authUser log,execute,net myuser priv

createUser -e 0x8000000001020304 traptest SHA mypassword AES
authUser log,execute traptest
~~~
Save and exit file (**ctrl+x**, followed by **y** and enter).

## Step 3: Setup Script for SNMP Trap Processing
### Configure a Perl Script for Handling SNMP Traps
Install a Perl package on Linux:

~~~
yum install -y net-snmp-perl 
~~~

Download the ‘zabbix_trap_receiver.pl’ script from github, save it to ‘/usr/bin’ and grant it executable permissions:

~~~
curl -o /usr/bin/zabbix_trap_receiver.pl https://raw.githubusercontent.com/TheAldin/scripts/main/zabbix_trap_receiver.pl
chmod +x /usr/bin/zabbix_trap_receiver.pl
~~~

Disable the current traphandles with this command:

~~~
sed -i '/^traphandle/ s/^/# /' /etc/snmp/snmptrapd.conf
~~~

Add this line to the SNMP configuration file so that Snmptrapd knows it needs to send each trap to Perl script for processing:

~~~
echo 'perl do "/usr/bin/zabbix_trap_receiver.pl"' >> /etc/snmp/snmptrapd.conf
~~~

## Step 4: Restart and Enable Snmptrapd to Start on Boot
Use the following command to restart the Snmptrapd service:

~~~
systemctl restart snmptrapd
~~~

And make sure to enable it to start on boot so that the Snmpd service starts automatically when the system reboots:

~~~
systemctl enable snmptrapd
~~~

Check the status of the snmptrapd process with command:

~~~
systemctl status snmptrapd
~~~

## Step 5: Configure Firewall to permit SNMP traps
If you have firewall enabled then you need to permin UDP port 162 to receive SNMP traps.

Use these commands to open UDP port 162 on **RHEL** / **CentOS** / **Oracle Linux** / **Alma Linux** / **Rocky Linux OS**:

~~~
firewall-cmd --permanent --add-port=162/udp
~~~
~~~
firewall-cmd --reload
~~~

## Step 6: Testing SNMP v3, v2 and v1 Traps

~~~
# Testing SNMP v1 traps
snmptrap -v1  -c public 127.0.0.1 '.1.3.6.1.6.3.1.1.5.4' '0.0.0.0' 6 33 '55' .1.3.6.1.6.3.1.1.5.4 s "testSNMPv1"

# Testing SNMP v2 traps
snmptrap -v 2c -c secret 127.0.0.1 0 linkUp.0  .1.3.6.1.6.3.1.1.5.4 s "testSNMPv2"

# Testing SNMP v3 traps with MD5
snmptrap -e 0x80000009030038FD -v 3 -l authPriv -u myuser -a MD5 -A  authpass -x AES -X mypassword 127.0.0.1 0 linkUp.0  .1.3.6.1.6.3.1.1.5.4 s "testSNMPv3_MD5"

# Testing SNMP v3 traps with AES
snmptrap -v 3 -n "" -a SHA -A mypassword -x AES -X mypassword -l authPriv -u traptest -e 0x8000000001020304 localhost 0 linkUp.0  .1.3.6.1.6.3.1.1.5.4 s "testSNMPv3_AES"
~~~

Now, we need to check the ‘zabbix_traps.tmp‘ file, to see if the script has processed those traps, using this command:

~~~
cat /var/log/snmptrap/zabbix_traps.tmp
~~~

### If everything was done correctly, you should have four SNMP traps that look like this

~~~
[root@localhost ~]# cat /var/log/snmptrap/zabbix_traps.tmp

10:07:10 2023/12/19 ZBXTRAP 127.0.0.1
PDU INFO:
  messageid                      0
  community                      public
  requestid                      0
  version                        0
  receivedfrom                   UDP: [127.0.0.1]:45615->[127.0.0.1]:162
  transactionid                  1
  errorstatus                    0
  notificationtype               TRAP
  errorindex                     0
VARBINDS:
  DISMAN-EVENT-MIB::sysUpTimeInstance type=67 value=Timeticks: (55) 0:00:00.55
  SNMPv2-MIB::snmpTrapOID.0      type=6  value=OID: IF-MIB::linkUp.0.33
  IF-MIB::linkUp                 type=4  value=STRING: "testSNMPv1"
  SNMP-COMMUNITY-MIB::snmpTrapCommunity.0 type=4  value=STRING: "public"
  SNMPv2-MIB::snmpTrapEnterprise.0 type=6  value=OID: IF-MIB::linkUp
10:07:10 2023/12/19 ZBXTRAP 127.0.0.1
PDU INFO:
  requestid                      1329914491
  community                      secret
  messageid                      0
  transactionid                  2
  version                        1
  receivedfrom                   UDP: [127.0.0.1]:42739->[127.0.0.1]:162
  errorstatus                    0
  notificationtype               TRAP
  errorindex                     0
VARBINDS:
  DISMAN-EVENT-MIB::sysUpTimeInstance type=67 value=Timeticks: (0) 0:00:00.00
  SNMPv2-MIB::snmpTrapOID.0      type=6  value=OID: IF-MIB::linkUp.0
  IF-MIB::linkUp                 type=4  value=STRING: "testSNMPv2"
10:07:10 2023/12/19 ZBXTRAP 127.0.0.1
PDU INFO:
  securityEngineID               0x80000009030038fd
  errorstatus                    0
  securitymodel                  3
  contextName
  errorindex                     0
  notificationtype               TRAP
  requestid                      548565555
  contextEngineID                0x80001f888044f1eb6ed8f17b6500000000
  messageid                      639394077
  securitylevel                  3
  securityName                   myuser
  transactionid                  3
  receivedfrom                   UDP: [127.0.0.1]:51365->[127.0.0.1]:162
  version                        3
VARBINDS:
  DISMAN-EVENT-MIB::sysUpTimeInstance type=67 value=Timeticks: (0) 0:00:00.00
  SNMPv2-MIB::snmpTrapOID.0      type=6  value=OID: IF-MIB::linkUp.0
  IF-MIB::linkUp                 type=4  value=STRING: "testSNMPv3_MD5"
10:07:12 2023/12/19 ZBXTRAP 127.0.0.1
PDU INFO:
  errorstatus                    0
  securityEngineID               0x8000000001020304
  contextName
  securitymodel                  3
  notificationtype               TRAP
  errorindex                     0
  contextEngineID                0x80001f888044f1eb6ed8f17b6500000000
  requestid                      215025274
  securitylevel                  3
  messageid                      1876781901
  securityName                   traptest
  transactionid                  4
  receivedfrom                   UDP: [127.0.0.1]:43463->[127.0.0.1]:162
  version                        3
VARBINDS:
  DISMAN-EVENT-MIB::sysUpTimeInstance type=67 value=Timeticks: (0) 0:00:00.00
  SNMPv2-MIB::snmpTrapOID.0      type=6  value=OID: IF-MIB::linkUp.0
  IF-MIB::linkUp                 type=4  value=STRING: "testSNMPv3_AES"

~~~

## Step 7: Enable SNMP Traps on Zabbix Server
All the setup we’ve done so far has nothing to do with Zabbix. That was the standard procedure for enabling traps on Linux. Now, we need to configure Zabbix to start the SNMP trapper process and point it to the trap file that the scripts generates. In our case, trap file would be ‘/var/log/snmptrap/zabbix_traps.tmp’.

Open ‘zabbix_server.conf’ file with command:

~~~
nano /etc/zabbix/zabbix_server.conf
~~~

and add this configuration anywhere in file:

~~~
StartSNMPTrapper=1 
SNMPTrapperFile=/var/log/snmptrap/zabbix_traps.tmp
~~~

Save and exit file (**ctrl+x**, followed by **y** and **enter**). And restart Zabbix server afterwards:

~~~
systemctl restart zabbix-server
~~~

## Example creating a Host

### ทำการสร้าง Snmp บน อุปกรณ์ทีต้องการใช้ 
 SNMPv3 Config Command to Create
~~~
snmp-server group {GROUP-NAME} v3 priv
~~~
~~~
snmp-server user {USER-NAME} {GROUP-NAME} v3 auth sha {PASSWORD} priv aes 128 {PASSWORD}
~~~
### Example:
~~~
snmp-server group test v3 priv
~~~
~~~
snmp-server user sw1 test v3 auth sha 123456789 priv aes 128 123456789
~~~
![Screenshot 2023-12-25 103530](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5a48ebf0-8806-4b6c-bb07-6fab01e05008)

### Add device  to the file below

~~~
nano  /etc/snmp/snmptrapd.conf
~~~

### Example:

~~~
createUser -e 0x80000009030000CAE55BB881 sw1 SHA1 123456789 AES 123456789
authUser log,execute,net sw1 priv
~~~

![Screenshot 2023-12-25 103107](https://github.com/lersakk/ZabbixUserManual/assets/136166133/ed2c9650-07e1-434f-8887-abc38ffc63eb)
Save and exit file (**ctrl+x**, followed by **y** and enter).


### Run snmpwalk

~~~
snmpwalk -e 0x80000009030000CAE55BB881 -v 3 -l authPriv -u sw1 -a SHA -A  123456789 -x AES -X 123456789 192.168.56.122
~~~

### เข้าไปสร้าง Host ที่เว็บของ Zabbix 

![Screenshot 2023-12-25 102315](https://github.com/lersakk/ZabbixUserManual/assets/136166133/91c407db-aa76-4fcb-9bf2-8db416f4dfc9)


### ทำการ Run Service ต่างๆอีกรอบ

~~~
systemctl restart snmptrapd
~~~
~~~
systemctl enable snmptrap
~~~
~~~
systemctl restart zabbix-server
~~~


### ตัวอย่างการ ADD Templates ลงใน Host

![Screenshot 2023-12-25 105315](https://github.com/lersakk/ZabbixUserManual/assets/136166133/7a6b1a48-6e38-435d-8013-3a962b3ef565)
![Screenshot 2023-12-25 105351](https://github.com/lersakk/ZabbixUserManual/assets/136166133/651165fc-c88c-4006-9288-fd05d9efc516)








