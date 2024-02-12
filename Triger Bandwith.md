ก่อนอื่นให้ Add Template Cisco IOS by SNMP ลงไปใน Host ที่ต้องการก่อน

จากนั้นไปที่ Data collection > Host > Traigger ของ  Host นั้นๆ > Create trigger 

ตั้งชื่อของ Trigger ที่ช่อง Name :

เลือก Severity ที่ต้องการ

จากนั้นเพิ่ม Problem expression ตามคำสั่งด้านล่าง  ✨ คำสั่งนี้จะเป็นการจับBandwith ที่ G0/1 ✨

~~~

(
    (
        avg(/Swicth1/net.if.in[ifHCInOctets.10101], 1m) > last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 0 / 100 
        and avg(/Swicth1/net.if.in[ifHCInOctets.10101], 1m) <= last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 5 / 100
        and last(/Swicth1/net.if.speed[ifHighSpeed.10101]) > 0
    ) 
    or 
    (
        avg(/Swicth1/net.if.out[ifHCOutOctets.10101], 1m) > last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 0 / 100 
        and avg(/Swicth1/net.if.out[ifHCOutOctets.10101], 1m) <= last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 5 / 100
        and last(/Swicth1/net.if.speed[ifHighSpeed.10101]) > 0
    )
)

~~~

และเลือก OK event generation  เป็น Recovery expression 

จากนั้นเพิ่ม Recovery expressionv ตามคำสั่งด้านล่าง  ** คำสั่งนี้จะเป็นการจับBandwith ที่ G0/1 **

~~~

(
    (
        avg(/Swicth1/net.if.in[ifHCInOctets.10101], 1m) < last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 0 / 100 
        or avg(/Swicth1/net.if.in[ifHCInOctets.10101], 1m) > last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 5 / 100
        and last(/Swicth1/net.if.speed[ifHighSpeed.10101]) > 0
    ) 
    or 
    (
        avg(/Swicth1/net.if.out[ifHCOutOctets.10101], 1m) < last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 0 / 100 
        or avg(/Swicth1/net.if.out[ifHCOutOctets.10101], 1m) > last(/Swicth1/net.if.speed[ifHighSpeed.10101]) * 5 / 100
        and last(/Swicth1/net.if.speed[ifHighSpeed.10101]) > 0
    )
)

~~~

และกด Add เพื่อเพิ่ม Trigger 

## การดู key ของ Interface ของ Host ที่ต้องการ

เข้าไปที่ Monitoring > Latest data 
ทำการ Filter โดย เลือก  Host ที่ต้องการและกด Show details และกด Apply

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d722785a-dc53-4f1c-8cb3-9d8d02e3e001)

จากนั้นเลือก Interface ที่ตองการจะดูที่ TAG VALUES > Interface 

✨ ในที่นี้ จากเลือก G0/1 ✨

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/06542edc-f316-4640-a721-738232a1da3a)


เราจะสามารถรู้ได้ว่า Key ของ Interface ที่ต้องการคืออะไร จากตัวหนังสือสีเขียวดังรูปด้านล่าง 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/80e179c3-48c0-404e-a69a-0fc26dc5172f)



ตัวอย่างการ Create trigger 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5c786ef8-f225-4c84-b0f9-0f181d2a6bc7)




เมื่อสร้าง Trigger สำเร็จแล้ว หลังจากนั้นเราจะนำ Trigger ไปใส่ใน Map ที่เราได้สร้างไว้

เข้าไปที่ Monitoring > Map > เลือก Map ที่เราได้สร้างไว้ 

สามารถดูวิธีการสร้างได้ที่  [Map](https://github.com/lersakk/ZabbixUserManual/blob/main/Map.md) 
