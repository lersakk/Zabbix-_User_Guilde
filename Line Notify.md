<strong> <h1>
<p>------------------------------------------------------------------------------</p>
<p>------------------------------- Line Notify ---------------------------------</p>
<p>------------------------------------------------------------------------------</p>
</h1> </strong>

# Setting to send notifications to Line Notify

Creating Script for sending notifications to Line Notify to 

~~~
cd  zabbix-docker/zbx_env/usr/lib/zabbix/alertscripts
~~~

Then create a Script file with the nano command followed by the desired name here used as Line-notify 
For example

~~~
sudo nano line-notify
~~~


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/1adb47c4-bf9d-49a3-95ea-5715b9a37aab)

Then type the script as below

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

Then enter the command below to grant permissions to the script file

~~~
sudo chmod +x line-notify
~~~

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/58595472-438a-4007-9eee-0e2f9e2311b5)

And finally, perform the Install curl at CONTAINER zabbix-server

~~~
sudo docker exec -u root -ti {CONTAINER ID zabbix-server } bash
~~~

~~~
apt-get update
apt install curl
~~~


## Create Media

To create Media, go to Alerts > Media types > Create media type 
Then fill in the information as shown in the picture below in the 3 Script parameters that must be entered as follows


{ALERT.SENDTO}

{ALERT.SUBJECT}

{ALERT.MESSAGE}

Then press Add 

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/f7e30f01-af62-45df-a62f-7116d5d2bb2d)

## Create Line Notify

Go to [Line-Notify](https://notify-bot.line.me/) press Scan QR code from the website page on Application Line to add Line notify to be our friend

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/138e3c93-13dd-4667-be87-dd044f596767)


Then Login and press Drop down list from our Line name. Select My page

![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/b048188b-8681-48f4-a3ed-a609810f48d2)


Go to Generate token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/36239c2f-0fad-442b-88d0-2bd3c746bd37)

Enter the name of the Token. This name will appear in front of the notification page, such as entering Test Notify: This name will appear in front of the message. The message looks like Test Notify::followed by the message from the system that was sent. After that, select the chat you want to send. Notification and press Generate token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/715979c4-4f46-4672-9ef0-eaf6f3b29162)

Press Copy Token that you received


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/4772117a-20d0-4faa-b80e-f83a2218358c)


and go to our Line application to add Line notify to the group chat selected during Generate token


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/46b4228b-5e69-40fc-82fb-03382ac0816c)

After that, it was scheduled to be sent to Line  
By going to Users > Users > select User > Media > add
Select Type as Line-notify Send to. Put in the copied token and press Add to finish


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/82f8f40e-88f7-421b-8d02-5a10e3a34e07)

## ADD Actions Triger  

Go to Alerts > Actions > Trigger actions > Create action

Then name define Type of calculation and Conditions as desired 


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/73cfa7e8-3599-4f1f-827a-9256134f48e1)


and go to Operations, set Operations by clicking on Add
Select Send to Users as the User you want to send to Line 
Send only to Select the Media types that we have left to use Line notify. Click on Custom message
Set Subject and Message as desired  



![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/6849756e-c0f5-4f56-ba86-a9a518e84815)


Then go to Recovery operations. Configure it by clicking Add. Let Operation be Send message
Select Send to Users as the User you want to send to Line 
Send only to Select the Media types that we have left to use Line notify. Click on Custom message
Set Subject and Message as desired  



![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3a70515-76ae-4b70-b90e-8fe373cbd763)


## Example notification from LINE notify 


![image](https://github.com/lersakk/ZabbixUserManual/assets/136166133/b7f022a7-d891-4cf9-8b5f-a30277546cc6)
