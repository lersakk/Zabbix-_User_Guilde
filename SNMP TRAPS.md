# SNMP TRAPS

### Edit File .env  For External file
เพื่อให้การเพิ่ม User snmp traps นั้น เพิ่มได้ง่ายขึ้น โดยใช้คำสั่ง

~~~
sudo nano zabbix-docker/.env
~~~

และเพิ่มข้อความดังต่อไปนี้

~~~
SNMPTRAP_DIRECTORY=./Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/a13c7ae3-522e-429d-be82-b07780d863bf)


จากนั้น เข้าไปเพิ่มคำสั่งใน compose_zabbix_components.yaml

โดยใช้คำสั่ง 

~~~
sudo nano zabbix-docker/compose_zabbix_components.yaml
~~~

และเพิ่มข้อความดังต่อไปนี้ที่ zabbix-snmptraps:  


~~~

- ${SNMPTRAP_DIRECTORY}:/etc/snmp/snmptrapd.conf:ro

~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/1324003c-54a9-4fff-90d4-e5d214d98005)



### การเพิ่ม User ของอุปกรณ์ Network

โดยเข้าไปใน File snmptrapd.conf  ด้วยคำสั่ง

~~~

sudo nano zabbix-docker/Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf

~~~

โดยมีการเขียนดังนี้

~~~

createUser -e 0x{ENGINE-ID} {USER-NAME} SHA1 {PASSWORD} AES {PASSWORD}
authUser log,execute,net {USER-NAME} priv

~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/9cc65eb7-805a-4815-8e02-dc242190d7a7)


###  การติดตั้ง snmp และ Run snmpwalk

เริ่มต้นด้วยการติดตั้ง snmp 

~~~
sudo apt install snmp
~~~

จากนั้น Run  snmpwalk 

~~~
snmpwalk -v3 -u {USER-NAME} -l authPriv -a sha -A {PASSWORD} -x aes -X 
{PASSWORD} {IP-ADDRESS-HOST}

~~~
