ก่อนอื่นให้ Add Template Cisco IOS by SNMP ลงไปใน Host ที่ต้องการก่อน
จากนั้นไปที่ Data collection > Host > Traigger ของ  Host นั้นๆ > Create trigger 

ตั้งชื่อของ Trigger ที่ช่อง Name :
เลือก Severity ที่ต้องการ
จากนั้นเพิ่ม Problem expression ตามคำสั่งด้านล่าง  ** คำสั่งนี้จะเป็นการจับBandwith ที่ G0/1 **

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

## การดูว่า key ของ Interface ของ Host ที่ต้องการ

เข้าไปที่ Monitoring > Latest data 
ทำการ Filter โดย เลือก  Host ที่ต้องการและกด Show details และกด Apply

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d722785a-dc53-4f1c-8cb3-9d8d02e3e001)


ตัวอย่างการ Create trigger 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5c786ef8-f225-4c84-b0f9-0f181d2a6bc7)
