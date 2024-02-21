<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>------------------------------- SNMP Traps --------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

# การตั้งค่า SNMP Traps ใน Zabbix

## แก้ไขไฟล์ .env เพื่อใช้งานไฟล์ภายนอก

เพื่อทำให้การเพิ่มผู้ใช้ SNMP traps ง่ายขึ้น ให้ทำการแก้ไขไฟล์ .env ดังนี้:

1. เปิดไฟล์ .env ด้วยคำสั่ง:

```
sudo nano zabbix-docker/.env
```

2. เพิ่มข้อความต่อไปนี้ ที่ # Locations

```
SNMPTRAP_DIRECTORY=./Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf
```

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/a13c7ae3-522e-429d-be82-b07780d863bf" width="100%">

## เพิ่มคำสั่งใน compose_zabbix_components.yaml

เข้าไปเพิ่มคำสั่งในไฟล์ compose_zabbix_components.yaml ดังนี้:

1. เปิดไฟล์ compose_zabbix_components.yaml ด้วยคำสั่ง:

```
sudo nano zabbix-docker/compose_zabbix_components.yaml
```

2. เพิ่มข้อความต่อไปนี้ที่ส่วนของ zabbix-snmptraps: > volumes:

```
- ${SNMPTRAP_DIRECTORY}:/etc/snmp/snmptrapd.conf:ro
```
<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/1324003c-54a9-4fff-90d4-e5d214d98005" width="100%">

## เพิ่มผู้ใช้งานสำหรับอุปกรณ์เครือข่าย

1. เข้าไปที่ไฟล์ snmptrapd.conf ด้วยคำสั่ง:

```
sudo nano zabbix-docker/Dockerfiles/snmptraps/ubuntu/conf/etc/snmp/snmptrapd.conf
```

2. เพิ่มข้อมูลผู้ใช้งานดังนี้:

```
createUser -e 0x{ENGINE-ID} {USER-NAME} SHA1 {PASSWORD} AES {PASSWORD}
authUser log,execute,net {USER-NAME} priv
```
<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/9cc65eb7-805a-4815-8e02-dc242190d7a7" width="100%">

ข้อมูลเหล่านี้มาจากการสร้าง [ผู้ใช้งาน SNMP](https://github.com/lersakk/ZabbixUserManual/blob/main/Add%20Host.md)

## ติดตั้งและใช้งาน SNMP และ Run snmpwalk

1. ติดตั้ง snmp ด้วยคำสั่ง:

```
sudo apt install snmp
```

2. Run snmpwalk ด้วยคำสั่ง:

```
snmpwalk -v3 -u {USER-NAME} -l authPriv -a sha -A {PASSWORD} -x aes -X {PASSWORD} {IP-ADDRESS-HOST}
```

## สร้างเทมเพลตสำหรับ SNMP traps

1. สร้างเทมเพลตไปที่ Data collection > Template > Create template

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3fa01b6-d0d5-4769-a46b-78b225453950)

2. ตั้งชื่อ Template และเลือก Template groups จากนั้น กด Add

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/191dc9d4-7814-4d05-ad20-e1c1d2cf9c14)

3. เข้าไปที่ Template ที่เราพึ่งสร้าง จากนั้น เพิ่ม Item ใน Templates โดยเลือกคำว่า item

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2f37e176-43de-450f-b33f-f8199960893a)

4. เลือก Create item เพื่อสร้าง item

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2b1f4db5-6133-4dea-afdf-7512f9e4667f)

5. ตั้งชื่อ กำหนด Key และตั้งค่าต่างๆ ตามต้องการ และกด Add

~~~
ตัวอย่าง:

- Name: Snmp traps
- type: SNMP trap
- Key: snmptrap[]
- Log time format: yyyyMMdd.hhmmss
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/81249e4d-7636-494d-92be-d03d62035045)

6. นำเทมเพลตมาใช้กับอุปกรณ์เครือข่ายที่ต้องการ

## ตรวจสอบ Log ของ SNMP traps

1. ตรวจสอบ Log snmptraps ได้ที่ Monitoring > Latest data > SNMP TRAPS > History

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/ec97096d-8b7d-47ef-9f77-5702f0ec94fe)

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5d5a7bb2-2ec7-4bb1-929b-717fc5b9f312)

