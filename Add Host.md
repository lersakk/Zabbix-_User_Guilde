**ขั้นตอนการ Add Host**

มีการดำเนินการ 2 ที่ คือ บนอุปกรณ์ Network และ บน Zabbix

เข้าไปที่หน้า enable ของอุปกณ์ Network (ยกตัวอย่าง switch)

***-สร้าง snmp group***
~~~
Switch#configure terminal
Switch(config)#snmp-server group {GROUP-NAME} v3 priv
~~~

***ตรวจสอบ Group***
~~~
Switch#show snmp group
~~~
 ***-สร้าง snmp user (ตัวอย่าง snmp version 3)***
 ~~~
Switch(config)#snmp-server user {USER-NAME} {GROUP-NAME} v3 auth sha {PASSWORD} priv aes 128 {PASSWORD}
 ~~~

#######################################################################

USER-NAME : ชื่อUser ที่ต้องการตั้ง

GROUP-NAME : ชื่อ group snmp ที่ตั้งไว้ ที่ต้องการให้ user อยู่

v3 : snmp version 3

Password : รหัสผ่านที่ต้องการตั้ง

#######################################################################

<br>
<br>
<br>

***-Config อุปกรณ์ Network เพื่อให้มีการเปิดใช้งาน SNMP***
~~~
Switch(config)#snmp-server enable traps
~~~


***-enable host***

~~~
Switch(config)#snmp-server host {IP-ADDRESS-VM} version 3 priv {USER-NAME}
~~~
#######################################################################

IP-ADDRESS-VM : IP ของ Ubuntu

USER-NAME : ชื่อ User ที่ต้องการ

#######################################################################
<br>
<br>
<br>



**ไปที่หน้า zabbix**

เลือก Data Collection

***-กดไปที่ Hosts***

***-กด create Host ที่ด้านขวาบน***

#######################################################################
   
Host Name : ชื่ออุปกรณ์ที่เราจะ monitor 

Template : เลือก Template ที่ต้องการ 

Host Groups : group ที่ต้องการให้ host อยู่

#######################################################################
<br>
<br>
<br>

***-กด add ที่อยู่ด้านล่าง Host Groups***

***-เลือกเป็น SNMP***

#######################################################################

IP address : IP อุปการที่จะ monitor 

DNS Name : ไม่มีก็ไม่ต้องใส่

port : 161

SNMP version : ตามที่ได้สร้างในอุปกรณ์ Network

Security name : {USER-NAME}

Security level : authPriv

Authentication protocol : MD5,SHA1,SHA224,SHA256,SHA384,SHA521

Authentication passphrase : {PASSWORD}

Privacy protocol : DES,AES128,AES192,AES256,AES192C,AES256C

Privacy passphrase : {PASSWORD}

#######################################################################

<br>
<br>
<br>

***-กด Add  ที่ด้านขวาล่าง เพื่อ สร้าง Host***
