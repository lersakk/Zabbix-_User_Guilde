<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>--------------------------------- Add Host ---------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

<h1>Example of steps for adding network equipment to monitor the performance of the equipment</h1>
  
## Add Host steps
_There are two operations: on Network devices and on Zabbix_

#

## Part 1 Network Device Configuretion

__Step 1 : Start__
1) Connect the console cable to configure the network device
2) Use Putty to integrate the network device
3) enable (use the enable command to enter Privilege Mode)
4) configure terminal (use the configure terminal command to enter Configuration Mode)

__Step 2 : Creating a snmp group__
~~~
configure terminal
snmp-server group {GROUP-NAME} v3 priv
~~~

__Step 3 : Group inspection__
~~~
show snmp group
~~~

__Step 4 : Creating a snmp user (snmp version 3 example)__

_Command :_
~~~
snmp-server user {USER-NAME} {GROUP-NAME} v3 auth sha {PASSWORD} priv aes 128 {PASSWORD}
~~~

_Parameter :_

~~~
USER-NAME  : Name of User you want to set
GROUP-NAME : Set group snmp name that requires the user to stay
v3         : snmp version 3
Password   : Password you want to set
~~~

__Step 5 : Check User__

~~~
show snmp user
~~~


__Step 6 : Config Network device to have SNMP enabled__

~~~
snmp-server enable traps
~~~

__Step 7 : Enable host__

_Command :_
~~~
snmp-server host {IP-ADDRESS-SERVER} version 3 priv {USER-NAME}
~~~

_Parameter :_

~~~
IP-ADDRESS-SERVER   : Ubuntu IP
USER-NAME           : {USER-NAME} desired
~~~

<br>

## Part 2 Zabbix Server Configuretion


**Go to zabbix page**

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/4ff020bc-fe4a-49c2-9352-718a86a3ba60" width="100%">

#

**Select Data Collection Press on Hosts And Press create Host on the top right**

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/d901fb4d-faec-4e93-ad4e-271ad43e1a99" width="100%">

#

**Fill in the required information**

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/206450e4-2b09-469c-81a2-0e85dfbc0de2" width="100%">

#

_Parameter :_

~~~   
Host Name       : The name of the device we will monitor 
Template        : Select the desired template 
Host Groups     : group that wants the host to be
Interfaces      : SNMP
  IP address           : IP อุปการที่จะ monitor 
  DNS Name             : (If you don't have it, you don't have to wear it)
  port                 : 161
  SNMP version         : As created on Network devices
  Security name        : {USER-NAME}
  Security level       : authPriv
  Auth protocol        : MD5,SHA1,SHA224,SHA256,SHA384,SHA521
  Auth passphrase      : {PASSWORD}
  Privacy protocol     : DES,AES128,AES192,AES256,AES192C,AES256C
  Privacy passphrase   : {PASSWORD}
~~~

#

***-Press Add at the bottom right to create a Host***

# Next Step to Setup SNMP Traps
## [Setup SNMP Traps](https://github.com/lersakk/ZabbixUserManual/blob/main/SNMP%20Traps.md)
