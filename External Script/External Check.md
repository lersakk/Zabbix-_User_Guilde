<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>----------------------------- DNS QUERY TIME ----------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>
<br>

__ตัวอย่างการทำ External Script เพื่อ Monitor ค่าประสิทธิภาพในการ Query ข้อมูลชื่อโดเมนบนระบบ DNS Server__

## 1. การสร้าง File ที่ Directory ของ External Script

__Step 1 ใช้คำสั่งเพื่อเข้าไปที่ Directory ที่เก็บไฟล์ Script ภายนอก__

__Command :__

~~~
cd zabbix-docker/zbx_env/usr/lib/zabbix/externalscripts/
~~~

__Step 2 ใช้คำสั่งเพื่อสร้างไฟล์ Script__

__Command :__

~~~
nano dns_query_time.sh
~~~

__Step 3 ทำการเพิ่มข้อมูลบนไฟล์ ดังนี้__

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

__Step 4 กดปุ่ม “Ctrl+x”  และ พิมพ์ “Y” เพื่อบันทึกไฟล์__

__ใช้คำสั่งเพื่อให้สิทธ์แก่ไฟล์ Script__

__Command :__
~~~
sudo chmod 755 dns_query_time.sh
~~~

## 2. การติดตั้งแพคเกจ dnsutils บน Container
_ในการ Monitor ค่าประสิทธิภาพในการ Query ข้อมูลชื่อโดเมนบนระบบ DNS Server บน Container ของ Docker จำเป็นที่จะต้องใช้คำสั่ง dig และจะเป็นต้องมีแพคเกจ dnsutils_

__Step 1 ใช้คำสั่งเพื่อแสดง List ของ Container ที่ทำงานอยู่__

__Command :__
~~~
sudo docker ps
~~~

__Step 2 ทำการ Execute เข้าไปใน Container__
__Command :__
~~~
sudo docker exec -u root -ti [CONTAINER-ID] bash
~~~

__Step 3 หลังจากเข้ามาใน Container ใช้คำสั่งเพื่อทำการติดตั้ง dnsutils และรอการติดตั้งจนเสร็จสมบูรณ์__

__Command :__
~~~
apt-get update
~~~

__Command :__
~~~
apt install dnsutils 
~~~

## 3. การกำหนดค่าการแสดงผลบน Zabbix Web Interface

__Step 1 ไปที่หน้า Zabbix Web Interface เพื่อกำหนดค่า__

__Step 2 ไปที่แท็บ Data collection -> Templates -> Create Templates และใส่ข้อมูลดังนี้__

~~~
Template name : External Check
Template groups : Template groups (Group ที่ต้องการ) 
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/70b949d6-61cb-4133-b435-d6ab2594a3cc" width="100%">

__.__

__Step 3 ไปที่ Template ที่สร้าง -> ไปที่ Create Item ใส่ข้อมูลดังนี้__
~~~
Name : DNS Query Time
Type : Zabbix agent
Key : dns_query_time.sh (ชื่อไฟล์ที่ทำการสร้าง)
Type of information : Numeric (float)
Update interval : 1m
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/45eaa6a0-21b0-46e7-878a-ae181f4c99a7" width="100%">

__.__

__Step 4 ทำการกด “Add”__

__Step 5 ทำการเพิ่ม Template เข้าไปที่ Host เพื่อทำการ Monitor__ ([How to Add Template to Hosts](https://www.w3schools.com/html/html_styles.asp))








