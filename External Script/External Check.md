# External Check

# DNS_QUERY_TIME
ตัวอย่างการทำ External Script เพื่อ Monitor ค่าประสิทธิภาพในการ Query ข้อมูลชื่อโดเมนบนระบบ DNS Server

การสร้าง File ของ Directory Script

ใช้คำสั่งเพื่อเข้าไปที่ Directory ที่เก็บไฟล์ Script ภายนอก
##### Command : 
~~~
cd zabbix-docker/zbx_env/usr/lib/zabbix/externalscripts/
~~~

ใช้คำสั่งเพื่อสร้างไฟล์ Script
##### Command : 
~~~
nano dns_query_time.sh
~~~

3)	ทำการเพิ่มข้อมูลบนไฟล์ ดังภาพ
~~~
#!/bin/bash

# Run dig command and capture the output
dig_output=$(dig google.com)

# Extract query time using grep and awk
query_time=$(echo "$dig_output" | grep "Query time:" | awk '{print $4}')

# Print the query time
echo "$query_time"
~~~
