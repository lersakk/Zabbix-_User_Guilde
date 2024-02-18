<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>------------------------------- External Alert ------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

<h1>ตัวอย่าง การตั้งค่าการแจ้งเตือนไปยัง Line Notify</h1>

## 1. การสร้าง File ที่ Directory ของ External Script

__Step 1 : ใช้คำสั่งเพื่อเข้าไปที่ Directory ที่เก็บไฟล์ Script ภายนอก__

__Command :__

~~~
cd zabbix-docker/zbx_env/usr/lib/zabbix/externalscripts/
~~~

__Step 2 : ใช้คำสั่งเพื่อสร้างไฟล์ Script__

__Command :__

~~~
sudo touch line-notify
~~~

__Step 3 : ทำการเพิ่มข้อมูลบนไฟล์โดยการใช้คำสั่ง nano [FILE-NAME] ในที่นี่ใช้ชื่อเป็น Line-notify__

__Command :__

~~~
sudo nano line-notify
~~~

__Script :__
~~~
#!/bin/bash

# Extract parameters from Zabbix alert
send_to="$1"
subject="$2"
message="$3"
 
# Construct the message for Line Notify
formatted_message="Subject: $subject\n\n$message"
 
# Send the message via Line Notify
curl -X POST -H "Authorization: Bearer $send_to" \
     -F "message=$formatted_message" https://notify-api.line.me/api/notify
~~~

__Step 4 : กดปุ่ม “Ctrl+x”  และ พิมพ์ “Y” เพื่อบันทึกไฟล์__

__Step 5 : ใช้คำสั่งเพื่อให้สิทธ์แก่ไฟล์ Script__

__Command :__

~~~
sudo chmod +X line-notify
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/58595472-438a-4007-9eee-0e2f9e2311b5" width="100%">

#

<br>

## 2. การสร้าง Line Notify

__Step 1 : ไปที่ [https://notify-bot.line.me/](https://notify-bot.line.me/) เพื่อทำการ Generate Token__

__Step 2 : ทำการแสกน QR Code จากหน้าเว็บไซต์บน Application Line เพื่อทำการเพิ่มเพื่อนกับ Line notify__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/86aa77d2-7c40-41bf-a936-632905637bde" width="100%">

#

__Step 3 : ทำการ Login และกดที่ Drop down จากนั้นเลือก My page__

#

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/b048188b-8681-48f4-a3ed-a609810f48d2" width="100%">

#

__Step 4 : ไปที่  Generate  token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/7113454f-d4c6-481d-9e56-c7ea381b97f1" width="100%">

#

__Step 5 : ใส่ชื่อของ Token และทำการใส่ชื่อของ Token__

_ชื่อดังกล่าวจะไปปรากฏหน้าข้อความโดยข้อความมีลักษณะเป็น Test Notify::ตามด้วย message จากระบบที่ส่งมา_

__Step 5 : (ต่อ) หลังจากนั้นทำการเลือก ห้อง Chat ที่ต้องการส่งการแจ้งเตือน และทำการกด Generate  Token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/44361b98-1f6d-464d-8f03-78ffcb2b6a1a" width="50%">

#

__Step 6 : ทำการ Copy Token ที่ได้มา__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/eab67553-e9e8-4fe5-9794-70085e6685ef" width="50%">

#

__Step 7 : ไปยัง Application Line เพื่อทำการเพิ่ม Line notify เข้าไปยังแชทกลุ่มที่ทำการเลือกไว้ในขั้นตอนการ Generate Token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/46b4228b-5e69-40fc-82fb-03382ac0816c" width="50%">

#

<br>

## 3. สร้าง Media Type
_ในการสร้าง Media เพื่อทำการแจ้งเตือนผ่าน Line Notify จำเป็นที่จะต้องเพิ่มก่อนที่จะสร้างการแจ้งเตือน สามารถทำได้ดังนี้_

__Step 1 : ไปที่ Alerts >Media types > Create media type__

__Step 2 : จากนั้นกรอกข้อมูลตามรูปด้านล่าง ใน Script parameters ทั้ง 3 ตัวที่จะต้องใส่ไปมีดังนี้__

~~~
Value :
   {ALERT.SENDTO}
   {ALERT.SUBJECT}
   {ALERT.MESSAGE}
~~~

__Step 3 : ทำการกด Add__ 

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/f7e30f01-af62-45df-a62f-7116d5d2bb2d" width="100%">

#

<br>

## 4. การเพิ่ม Media Line Notify เข้าไปที่ User

__Step 1 : ไปที่ Users > Users > Media และทำการกดปุ่ม Add เพื่อทำการเพิ่ม__

__Step 2 : ทำการกรอกข้อมูลที่จำเป็น ดังตัวอย่าง__

~~~
Type : Line-notify
Send to : กรอก Token ที่ทำการ Generate ไว้
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/b590a831-8c31-4525-8143-9090c09a2622" width="50%">

#

__Step 3 : กด Add__

#

<br>

## 5. ADD Actions Triger  

__Step 1 : ไปที่ Alerts > Actions > Trigger actions > Create action__

__Step 2 : จากนั้น ตั้งชื่อ กำหนด Type of calculation และ Conditions  ตามต้องการ__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/73cfa7e8-3599-4f1f-827a-9256134f48e1" width="100%">

#

__Step 3 : ไปที่ Operations กำหนด Operations โดยคลิกที่ Add__

ทำการเลือก Send to Users เป็น User ที่ต้องการใช้ส่งไปที่ Line 
Send only to เลือกเป็น Media types ที่เราส้างไว้เพื่อใช้ Line notify คลิกที่ Custom message
กำหนด Subject และ Message ตามต้องการ 

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/6849756e-c0f5-4f56-ba86-a9a518e84815" width="50%">

#

__Step 4 : จากนั้นไปที่ Recovery operations กำหนดค่าโดยคลิกที่ Add ให้ Operation เป็น Send message__
ทำการเลือก Send to Users เป็น User ที่ต้องการใช้ส่งไปที่ Line 
Send only to เลือกเป็น Media types ที่เราส้างไว้เพื่อใช้ Line notify คลิกที่ Custom message
กำหนด Subject และ Message ตามต้องการ 

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3a70515-76ae-4b70-b90e-8fe373cbd763" width="50%">

#

## ตัวอย่างการแจ้งเตือนจาก LINE notify 

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/b7f022a7-d891-4cf9-8b5f-a30277546cc6" width="50%">

