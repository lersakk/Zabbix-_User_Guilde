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

###### จากนั้นให้ใส่คำสั่ง ด้านล่าง เพื่อให้ zabbix สามารถใช้ script เพื่อส่งการแจ้งเตือนได้

~~~
sudo chmod +x line-notify
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/58595472-438a-4007-9eee-0e2f9e2311b5)


## สร้าง Media 

###### การสร้าง Media ขึ้นมาให้ไปที่ Alerts >Media types > Create media type 
จากนั้นกรอกข้อมูลตามรูปด้านล่าง Script parameters ทั้ง3 ตัวที่ใส่ไปมีดังนี้
{ALERT.SENDTO},{ALERT.SUBJECT},{ALERT.MESSAGE}
จากนั้นก็กด Add ได้เลย




