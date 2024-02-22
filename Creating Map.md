![User Mannal (6)](https://github.com/lersakk/ZabbixUserManual/assets/106941759/a743d27b-077e-4904-a392-8ddf0c8a8112)


<h1>Creating Map</h1>

Go to Monitoring → Maps

→ Go to the view with all maps

→ Click on Create map

The Map tab contains general map attributes:
![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/5a223fca-9607-488c-a24d-a830671fd772)

## Adding elements
You can go to Monitoring → Maps → Go to the view with all maps → Click on Constructor
To add elements  By clicking on the add at map elements and click on that element to determine


Type : Host

Label:
~~~
{HOSTNAME} ({HOST.IP})
Latency: {?last(/{HOST.HOST}/icmppingsec)}
~~~
Change Icon :

If everything is done, press apply

 ![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/e9342802-653b-4287-bc5a-b975f1d76a89)


## Linking elements

Once you have put some elements on the map, it is time to start linking them. To link two elements you must first select them. With the elements selected, click on Add next to Link.

With a link created, the single element form now contains an additional Links section. Click on Edit to edit link attributes.

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/62fda27c-342a-4839-9bdc-da8a6b0db3b9)

 And you can add trigger in Link indicator By clik on the Add in Column Linkindicator
 
 ![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/058be4d2-0ac5-48cd-9d79-2ec53a1d1df0)

And select the trigger that wants to be warned by the warning. The warning will come in the form of Changing the color of the Link indicator.

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/49b3ddda-0425-4de4-86b8-35f13b96a1e1)


and you can create Tigger Other designs can be used such as making a Tigger to tell the Bandwith [Triger Bandwith](https://github.com/lersakk/ZabbixUserManual/blob/main/Trigger%20Bandwidth.md)
 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/65456465-a83a-47f7-a5e1-1f71a2870c94)





# Option for Map
## [Setup for User Trigger Bandwidth](https://github.com/lersakk/ZabbixUserManual/blob/main/Trigger%20Bandwidth.md)

# Option Alert
## [Setup for User Line-Notify](https://github.com/lersakk/ZabbixUserManual/blob/main/Line%20Notify.md)
