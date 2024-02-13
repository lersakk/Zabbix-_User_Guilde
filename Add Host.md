<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>--------------------------------- Add Host ---------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>
<br>

<h1>ตัวอย่างขั้นตอนการเพิ่มอุปกรณ์เครือข่ายเพื่อ Monitoring ค่าประสิทธิภาพในการทำงานของตัวอุปกรณ์</h1>

# ขั้นตอนการ Add Host

มีการดำเนินการ 2 ที่ คือ บนอุปกรณ์ Network และ บน Zabbix




## Part 1 Network Device Configuretion

__Step 1 : Start__
1) ต่อสาย Console เพื่อทำการกำหนดค่าบนตัวอุปกรณ์ Network
2)	ใช้ Putty เพื่อทำการ Console เข้าตัว อุปกรณ์ Network
3)	enable (ใช้คำสั่ง enable เพื่อเข้าสู่ Privilege Mode)
4)	configure terminal (ใช้คำสั่ง configure terminal เพื่อเข้าสู่ Configuration Mode)


__Step 2 : การสร้าง snmp group__
~~~
Switch#configure terminal
Switch(config)#snmp-server group {GROUP-NAME} v3 priv
~~~

__Step 3 : การตรวจสอบ Group__
~~~
Switch#show snmp group
~~~

__Step 4 : การสร้าง snmp user (ตัวอย่าง snmp version 3)__

_Command :_
~~~
Switch(config)#snmp-server user {USER-NAME} {GROUP-NAME} v3 auth sha {PASSWORD} priv aes 128 {PASSWORD}
~~~

_Parameter :_

~~~
USER-NAME : ชื่อUser ที่ต้องการตั้ง

GROUP-NAME : ชื่อ group snmp ที่ตั้งไว้ ที่ต้องการให้ user อยู่

v3 : snmp version 3

Password : รหัสผ่านที่ต้องการตั้ง
~~~


__Step 5 : ตรวจสอบ User__

~~~
Switch#show snmp user
~~~


__Step 6 : Config อุปกรณ์ Network เพื่อให้มีการเปิดใช้งาน SNMP__

~~~
Switch(config)#snmp-server enable traps
~~~


__Step 7 : Enable host__

_Command :_
~~~
Switch(config)#snmp-server host {IP-ADDRESS-SERVER} version 3 priv {USER-NAME}
~~~

_Parameter :_
~~~
IP-ADDRESS-SERVER : IP ของ Ubuntu

USER-NAME : ชื่อ User ที่ต้องการ
~~~

<br>
<br>

## Part 2 Zabbix Server Configuretion


**ไปที่หน้า zabbix**
![image](https://github.com/lersakk/ZabbixUserManual/assets/106941759/4ff020bc-fe4a-49c2-9352-718a86a3ba60)



**เลือก Data Collection กดไปที่ Hosts And กด create Host ที่ด้านขวาบน**
![image](https://github.com/lersakk/ZabbixUserManual/assets/106941759/d901fb4d-faec-4e93-ad4e-271ad43e1a99)



**กรอกข้อมูลที่จำเป็น**
![image](https://github.com/lersakk/ZabbixUserManual/assets/106941759/206450e4-2b09-469c-81a2-0e85dfbc0de2)

_Parameter :_

~~~   
Host Name : ชื่ออุปกรณ์ที่เราจะ monitor 

Template : เลือก Template ที่ต้องการ 

Host Groups : group ที่ต้องการให้ host อยู่

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
~~~

<br>
<br>
<br>

***-กด add ที่อยู่ด้านล่าง Host Groups***

**-เลือกเป็น SNMP**
<br>
![image](https://github.com/lersakk/ZabbixUserManual/assets/111458085/69e3f0c8-e198-41ed-9df8-320f0608c01a)

<br>
#######################################################################



#######################################################################

<br>
<br>
<br>

***-กด Add  ที่ด้านขวาล่าง เพื่อ สร้าง Host***
