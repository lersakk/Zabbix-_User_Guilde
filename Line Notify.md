<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>------------------------------- Line Notify ---------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

# การตั้งค่าส่งแจ้งเตือนไปยัง Line Notify

การสร้าง Script สำหรับส่งการแจ้งเตือนไปยัง Line Notify  ไปที่ 

~~~
cd  zabbix-docker/zbx_env/usr/lib/zabbix/alertscripts
~~~

จากนั้นทำการสร้างไฟล์ Script ขึ้นมาด้วยคำสั่ง nano ตามด้วยชื่อที่ต้องการในที่นี่ใช้เป็น Line-notify 
ตัวอย่างเช่น

~~~
sudo nano line-notify
~~~


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/1adb47c4-bf9d-49a3-95ea-5715b9a37aab)

จากนั้นให้พิมพ์ script ตามด้านล่างนี้

~~~
#!/bin/bash
 
# curl -X POST -H "Authorization: Bearer $1" \
#      -F "message=$2" https://notify-api.line.me/api/notify
 
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

จากนั้นให้ใส่คำสั่ง ด้านล่าง เพื่อให้ สิทธิ์กับไฟล์ script

~~~
sudo chmod +x line-notify
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/58595472-438a-4007-9eee-0e2f9e2311b5)


## สร้าง Media 

การสร้าง Media ขึ้นมาให้ไปที่ Alerts >Media types > Create media type 
จากนั้นกรอกข้อมูลตามรูปด้านล่าง ใน Script parameters ทั้ง 3 ตัวที่จะต้องใส่ไปมีดังนี้

{ALERT.SENDTO}

{ALERT.SUBJECT}

{ALERT.MESSAGE}

จากนั้นก็กด Add 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/f7e30f01-af62-45df-a62f-7116d5d2bb2d)

## สร้าง Line Notify

ไปที่  [Line-Notify](https://notify-bot.line.me/)  กด Scan QR code จากหน้าเว็บไซต์บน application Line เพื่อเพิ่ม Line notify มาเป็นเพื่อนเรา

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/138e3c93-13dd-4667-be87-dd044f596767)


จากนั้น  Login และกด Drop down list ลงมาจากชื่อ Line ของเรา เลือก My page

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/b048188b-8681-48f4-a3ed-a609810f48d2)


ไปที่  Generate  token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/36239c2f-0fad-442b-88d0-2bd3c746bd37)

ใส่ชื่อของ Token โดยชื่อนี้จะไปปรากฏอยู่หน้าการแจ้งเตือนเช่นใส่ Test Notify: ชื่อนี้จะไปปรากฏหน้าข้อความโดยข้อความมีลักษณะเป็น Test Notify::ตามด้วย message จากระบบที่ส่งมา หลังจากนั้น ทำการเลือก chat ที่ต้องการส่งการแจ้งเตือนไป และ กด Generate  token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/715979c4-4f46-4672-9ef0-eaf6f3b29162)

กด Copy token ที่ได้มา


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/4772117a-20d0-4faa-b80e-f83a2218358c)


และไปยัง application Line ของเราเพื่อทำการเพิ่ม Line notify เข้าไปยังแชทกลุ่มที่ทำการเลือกไว้ตอน Generate  token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/46b4228b-5e69-40fc-82fb-03382ac0816c)

หลังจากนั้นกำหนดให้ส่งไปที่ Line  
โดยไปที่ Users > Users > Media > add
เลือก Type  เป็น Line-notify    Send to ให้นำ token ที่คัดลอกไว้มาใส่แล้วกด Add เป็นอันเสร็จ


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/82f8f40e-88f7-421b-8d02-5a10e3a34e07)

## ADD Actions Triger  

ไปที่ Alerts > Actions > Trigger actions > Create action

จากนั้น ตั้งชื่อ กำหนด Type of calculation และ Conditions  ตามต้องการ 


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/73cfa7e8-3599-4f1f-827a-9256134f48e1)


และไปที่ Operations กำหนด Operations  โดยคลิกที่ Add
เลือก Send to Users เป็น User ที่ต้องการใช้ส่งไปที่ Line 
Send only to เลือกเป็น Media types ที่เราส้างไว้เพื่อใช้ Line notify คลิกที่ Custom message
กำหนด Subject และ Message ตามต้องการ 



![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6849756e-c0f5-4f56-ba86-a9a518e84815)


จากนั้นไปที่ Recovery operations กำหนดค่าโดยคลิกที่ Add ให้ Operation เป็น Send message
เลือก Send to Users เป็น User ที่ต้องการใช้ส่งไปที่ Line 
Send only to เลือกเป็น Media types ที่เราส้างไว้เพื่อใช้ Line notify คลิกที่ Custom message
กำหนด Subject และ Message ตามต้องการ 



![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3a70515-76ae-4b70-b90e-8fe373cbd763)


## ตัวอย่างการแจ้งเตือนจาก LINE notify 


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/b7f022a7-d891-4cf9-8b5f-a30277546cc6)
