<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>-------------------------- Trigger Bandwidth ------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

# Trigger Bandwidth

First add the Cisco IOS by SNMP Template to the desired Host

Then go to the Data collection > Host > Trigger of that host > Create trigger 

Set Trigger's name in the Name field :

Select the desired Severity

Then add Problem expression according to the command below ✨ This command will capture Bandwith at G0/1 Triger Bandwith ✨

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

and select OK event generation as Recovery expression 

Then add Recovery expressionv according to the command below ✨ This command will capture Bandwith at G0/1 ✨

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

And press Add to add Trigger 

## Viewing the key of the desired host's interface

Go to Monitoring > Latest data 
Filter by selecting the desired host and pressing Show details and pressing Apply

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d722785a-dc53-4f1c-8cb3-9d8d02e3e001)
#

Then select the desired interface to look at TAG VALUES > Interface 

✨ here from select G0/1 ✨

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/06542edc-f316-4640-a721-738232a1da3a)
#

We will be able to know what the key of the desired interface is. From the green letters as shown in the picture below  

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/80e179c3-48c0-404e-a69a-0fc26dc5172f)
#

Example of creating a trigger 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5c786ef8-f225-4c84-b0f9-0f181d2a6bc7)
#

Once the Trigger has been created, after that we will put the Trigger in the map that we have created

Go to Monitoring > Map > select the Map that we have created

You can see how to create it at  [Map](https://github.com/lersakk/ZabbixUserManual/blob/main/Creating%20Map.md) 

Then press Edit map > select the desired device > press Edit at Collumn Links ✨ here you will select Links Router ✨

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/2e0db9c1-e582-4276-b301-289fe75bb464)
#

See Collumn Link indicators > Add

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6f282086-0d55-4bc5-b09f-13cb2028ca15)
#

Select the Host we want and select the Trigger that we have created. Then press Select and press Apply and press Update on the Network maps page

 ![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6c3ef012-ad83-42d8-8b59-55fbb4bb77d3)
 
#

✨ enhances how to do Label tactical realtime ✨

Go to Monitoring > Map > Select the desired Map > Edit map > Select the desired device > Press Edit at Collumn Links ✨ Here you will select Links Router ✨

Go to Collumn Label then enter text into Label as follows

✨ here will capture send-receive in Switch1 Interface G0/1 ✨
~~~
In:{?last(/Swicth1/net.if.in[ifHCInOctets.10101])} /  Out:{?last(/Swicth1/net.if.out[ifHCOutOctets.10101])} [{?last(/Swicth1/net.if.speed[ifHighSpeed.10101])}]
Swicth1:Gi0/1 <> Fa0/1(Router)
~~~

The above script is only an example. You can change the desired interface by replacing the key. An example can be found at : [Key Interface](https://github.com/lersakk/ZabbixUserManual/blob/main/Trigger%20Bandwidth.md#%E0%B8%81%E0%B8%B2%E0%B8%A3%E0%B8%94%E0%B8%B9-key-%E0%B8%82%E0%B8%AD%E0%B8%87-interface-%E0%B8%82%E0%B8%AD%E0%B8%87-host-%E0%B8%97%E0%B8%B5%E0%B9%88%E0%B8%95%E0%B9%89%E0%B8%AD%E0%B8%87%E0%B8%81%E0%B8%B2%E0%B8%A3)

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5671100c-8b99-41b6-a7a1-4af2b555214d)

