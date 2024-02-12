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
