![User Mannal (13)](https://github.com/lersakk/ZabbixUserManual/assets/106941759/15b86dbf-d518-4c3b-a72e-c9bee34d8fa0)

<h1>For example, setting notifications to Line Notify.</h1>

# 1. Setting to send notifications to Line Notify

__Step 1 : Use the command to go to the directory where the external script files are stored.__ 

___Command :___

~~~
cd zabbix-docker/zbx_env/usr/lib/zabbix/externalscripts/
~~~

__Step 2 : Use the command to create the Script file.__

_For example_

~~~
sudo nano line-notify
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/1adb47c4-bf9d-49a3-95ea-5715b9a37aab" width="100%">

__Step 3 : Add information to the file using the nano [FILE-NAME] command. Here the name is Line-notify.__

___Command :___

~~~
sudo nano line-notify
~~~

__Script :__
~~~
#!/bin/bash

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

__Step 4 : Press “Ctrl+x” and type “Y” to save the file.__

__Step 5 : Use commands to grant permissions to the Script file.__

___Command :___

~~~
sudo chmod +X line-notify
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/58595472-438a-4007-9eee-0e2f9e2311b5" width="100%">

__***Recommend granting permissions as needed. chmod 744__


<br>


__Step 6 : Finally, perform the Install curl at CONTAINER zabbix-server__

___Command On Local:___
~~~
sudo docker exec -u root -ti {CONTAINER ID zabbix-server } bash
~~~

___Command On Container:___
~~~
apt-get update
apt install curl
~~~

## 2. Create Line Notify

__Step 1 : Go to [Line-Notify](https://notify-bot.line.me/)__

__Step 2 : press Scan QR code from the website page on Application Line to add Line notify to be our friend__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/86aa77d2-7c40-41bf-a936-632905637bde" width="100%">


__Step 3 : Then Login and press Drop down list from our Line name. Select My page__

#

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/b048188b-8681-48f4-a3ed-a609810f48d2" width="100%">

#

__Step 4 : Go to Generate token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/7113454f-d4c6-481d-9e56-c7ea381b97f1" width="100%">

#

__Step 5 : Enter the name of the Token.__

_This name will appear in front of the notification page, such as entering Test Notify: This name will appear in front of the message. The message looks like Test Notify::followed by the message from the system that was sent. 

__Step 5 (Cont.) : After that, select the chat you want to send. Notification and press Generate token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/44361b98-1f6d-464d-8f03-78ffcb2b6a1a" width="50%">

#

__Step 6 : Press Copy Token that you received__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/eab67553-e9e8-4fe5-9794-70085e6685ef" width="50%">

#

__Step 7 : Go to our Line application to add Line notify to the group chat selected during Generate token__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/46b4228b-5e69-40fc-82fb-03382ac0816c" width="50%">

#


<br>


## 3. Create Media

__Step 1 : To create Media, go to Alerts > Media types > Create media type__

__Step 2 : Then fill in the information as shown in the picture below in the 3 Script parameters that must be entered as follows__

~~~
Value :
   {ALERT.SENDTO}
   {ALERT.SUBJECT}
   {ALERT.MESSAGE}
~~~

__Step 3 : Then press Add__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/f7e30f01-af62-45df-a62f-7116d5d2bb2d" width="100%">

#


<br>


## 4. Adding Media Line Notify to User

_After that, it was scheduled to be sent to Line_

__Step 1 : Going to Users > Users > select User > Media > add
Select Type as Line-notify Send to. Put in the copied token and press Add to finish__

~~~
Type : Line-notify
Send to : กรอก Token ที่ทำการ Generate ไว้
~~~

<img src="https://github.com/lersakk/ZabbixUserManual/assets/106941759/b590a831-8c31-4525-8143-9090c09a2622" width="50%">

#

__Step 2 : กด Add__


<br>


## 5. ADD Actions Triger  

__Step 1 : Go to Alerts > Actions > Trigger actions > Create action__

__Step 2 : Then name define Type of calculation and Conditions as desired__

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/73cfa7e8-3599-4f1f-827a-9256134f48e1" width="100%">

#

__Step 3 : Go to Operations, set Operations by clicking on Add__

_Select Send to Users as the User you want to send to Line 
Send only to Select the Media types that we have left to use Line notify. Click on Custom message_

___Set Subject and Message as desired___

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/6849756e-c0f5-4f56-ba86-a9a518e84815" width="50%">

#

__Step 4 : Then go to Recovery operations. Configure it by clicking Add. Let Operation be Send message__

_Select Send to Users as the User you want to send to Line 
Send only to Select the Media types that we have left to use Line notify. Click on Custom message Set Subject and Message as desired_


<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/d3a70515-76ae-4b70-b90e-8fe373cbd763" width="50%">

#

## Example notification from LINE notify 

<img src="https://github.com/lersakk/ZabbixUserManual/assets/136166133/b7f022a7-d891-4cf9-8b5f-a30277546cc6" width="50%">

