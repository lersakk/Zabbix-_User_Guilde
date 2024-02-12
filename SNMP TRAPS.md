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


โดยนำข้อมูลมาจากการที่เรานั้นได้สร้าง [snmp user](https://github.com/lersakk/ZabbixUserManual/blob/main/Add%20Host.md) ไว้   
 

###  การติดตั้ง snmp และ Run snmpwalk

เริ่มต้นด้วยการติดตั้ง snmp 

~~~
sudo apt install snmp
~~~

จากนั้น Run  snmpwalk 

~~~
snmpwalk -v3 -u {USER-NAME} -l authPriv -a sha -A {PASSWORD} -x aes -X {PASSWORD} {IP-ADDRESS-HOST}
~~~


### Create Template for snmp traps

สร้าง Templates เข้าที่ Data collection > Template > Create template

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3fa01b6-d0d5-4769-a46b-78b225453950)

ตั้งชื่อ Template และเลือก Template groups

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/191dc9d4-7814-4d05-ad20-e1c1d2cf9c14)


เพิ่ม Item ใน Templates ให้เลือกคำว่า item 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2f37e176-43de-450f-b33f-f8199960893a)

เลือก Create item เพื่อสร้าง item

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2b1f4db5-6133-4dea-afdf-7512f9e4667f)

ตั้งชื่อ กำหนด Key และตั้งค่าต่างๆตามต้องการ และ กด Add

-------------------ตัวอย่าง-----------------------

Name: Snmp traps

Key : snmptrap[]

log time format : yyyyMMdd.hhmmss

-----------------------------------------------


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/81249e4d-7636-494d-92be-d03d62035045)








