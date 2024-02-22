<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>------------------------------- SNMP Traps --------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

# SNMP Traps Settings in Zabbix

## Edit the .env file to use external files

To make adding SNMP traps users easier, edit the.env file as follows:

1. Open the.env file with the command:

```
sudo nano zabbix-docker/.env
```

2. Add the following text at # Locations

```
SNMPTRAP_DIRECTORY=./Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf
```

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/a13c7ae3-522e-429d-be82-b07780d863bf" width="100%">

## Add command in composition_zabbix_components.yaml

Go to add commands in the file. compose_zabbix_components.yaml as follows:

1. Open the file compose_zabbix_components.yaml with the command:

```
sudo nano zabbix-docker/compose_zabbix_components.yaml
```

2. Add the following text to the section of zabbix-snmptraps: > volumes:

```
- ${SNMPTRAP_DIRECTORY}:/etc/snmp/snmptrapd.conf:ro
```
<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/1324003c-54a9-4fff-90d4-e5d214d98005" width="100%">

## Add users for network devices

1. Access the snmptrapd.conf file with the command:

```
sudo nano zabbix-docker/Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf
```

2. Add user information as follows:

```
createUser -e 0x{ENGINE-ID} {USER-NAME} SHA1 {PASSWORD} AES {PASSWORD}
authUser log,execute,net {USER-NAME} priv
```
<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/9cc65eb7-805a-4815-8e02-dc242190d7a7" width="100%">

These data come from the creation [ผู้ใช้งาน SNMP](https://github.com/lersakk/ZabbixUserManual/blob/main/Add%20Hosts.md)

## Install and operate SNMP and Run snmpwalk

1. Install snmp with the command:

```
sudo apt install snmp
```

2. Run snmpwalk with the command:

```
snmpwalk -v3 -u {USER-NAME} -l authPriv -a sha -A {PASSWORD} -x aes -X {PASSWORD} {IP-ADDRESS-HOST}
```

## Create a template for SNMP traps

1. Create a template to Data collection > Template > Create template

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3fa01b6-d0d5-4769-a46b-78b225453950)

2. Create a Template name and select Template groups, then press Add

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/191dc9d4-7814-4d05-ad20-e1c1d2cf9c14)

3. Go to the template we just created, then add items to the templates by selecting the word item

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2f37e176-43de-450f-b33f-f8199960893a)

4. Select Create item to create item

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2b1f4db5-6133-4dea-afdf-7512f9e4667f)

5. Set a name, set a key, and set various settings as desired and press Add

~~~
Example:

- Name: Snmp traps
- type: SNMP trap
- Key: snmptrap[]
- Type of information: Log
- Log time format: yyyyMMdd.hhmmss
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/81249e4d-7636-494d-92be-d03d62035045)

6. Apply the template to the desired network device

## Check Log of SNMP traps

1. Check Log snmptraps at Monitoring > Latest data > SNMP TRAPS > History
![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/ec97096d-8b7d-47ef-9f77-5702f0ec94fe)

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5d5a7bb2-2ec7-4bb1-929b-717fc5b9f312)





# Next Step to Create Map
## [How to Create Map](https://github.com/lersakk/ZabbixUserManual/blob/main/Creating%20Map.md)
