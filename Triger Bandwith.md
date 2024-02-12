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
