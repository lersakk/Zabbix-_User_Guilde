# Triger Bandwith

ก่อนอื่นให้ Add Template Cisco IOS by SNMP ลงไปใน Host ที่ต้องการก่อน

จากนั้นไปที่ Data collection > Host > Traigger ของ  Host นั้นๆ > Create trigger 

ตั้งชื่อของ Trigger ที่ช่อง Name :

เลือก Severity ที่ต้องการ

จากนั้นเพิ่ม Problem expression ตามคำสั่งด้านล่าง  ✨ คำสั่งนี้จะเป็นการจับBandwith ที่ G0/1 Triger Bandwith

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

จากนั้นเพิ่ม Recovery expressionv ตามคำสั่งด้านล่าง  ✨ คำสั่งนี้จะเป็นการจับBandwith ที่ G0/1 ✨

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

จากนั้น กด Edit map > เลือกอุปกรณ์ที่ต้องการ > กด Edit ที่ Collumn Links  ✨ ในที่นี้จะเลือก Links Router ✨

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2e0db9c1-e582-4276-b301-289fe75bb464)


ดูที่ Collumn Link indicators > Add

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6f282086-0d55-4bc5-b09f-13cb2028ca15)


เลือก Host ที่เราต้องการ และเลือก Trigger ที่เราได้สร้างไว้ จากนั้น  กด Select และกด Apply และกด Update ที่หน้า Network maps

 ![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6c3ef012-ad83-42d8-8b59-55fbb4bb77d3)





✨ เสริมวิธีทำ Label taffic realtime ✨

เข้าไปที่ Monitoring > Map > Edit map >  เลือกอุปกรณ์ที่ต้องการ > กด Edit ที่ Collumn Links  ✨ ในที่นี้จะเลือก Links Router ✨

ไปที่ Collumn Label  จากนั้น ใส่ข้อความลงใน Label ดังนี้

✨ ในที่นี้จะเป็นการ จับ การส่ง-รับ ใน Interface G0/1 ของ Switch1 ✨
~~~

In:{?last(/Swicth1/net.if.in[ifHCInOctets.10101])} /  Out:{?last(/Swicth1/net.if.out[ifHCOutOctets.10101])} [{?last(/Swicth1/net.if.speed[ifHighSpeed.10101])}]
Swicth1:Gi0/1 <> Fa0/1(Router)

~~~

ข้อความข้างต้นเป็นเพียงตัวอย่างเท่านั้น คุณสามารถเปลี่ยน Interface ที่ต้องการได้จากการนำ Key มาใส่ โดยดูจาก [Key](https://github.com/lersakk/ZabbixUserManual/blob/main/Triger%20Bandwith.md#%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%94%E0%B8%B9-key-%E0%B8%82%E0%B8%AD%E0%B8%87-interface-%E0%B8%82%E0%B8%AD%E0%B8%87-host-%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%95%E0%B9%89%E0%B8%AD%E0%B8%87%E0%B8%81%E0%B8%B2%E0%B8%A3)



