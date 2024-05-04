
### Manual Step's
```
dnf module disable nodejs -y
dnf module enable nodejs:20 -y
dnf install nodejs -y
useradd expense
mkdir /app
curl -o /tmp/backend.zip https://expense-builds.s3.us-east-1.amazonaws.com/expense-backend-v2.zip
cd /app
unzip /tmp/backend.zip
npm install
```

### We need to setup a new service in systemd so systemctl can manage this service

Setup SystemD Expense Backend Service
```
vim /etc/systemd/system/backend.service
```
```
[Unit]
Description = Backend Service

[Service]
User=expense
Environment=DB_HOST="db.step-into-iot.cloud"
ExecStart=/bin/node /app/index.js
SyslogIdentifier=backend

[Install]
WantedBy=multi-user.target
```

```
systemctl daemon-reload
systemctl start backend
systemctl enable backend
```

We need to load the schema. To load schema we need to install mysql client.
```
dnf install mysql -y
```

Load Schema
```
mysql -h db.step-into-iot.cloud -uroot -pExpenseApp@1 < /app/schema/backend.sql
```
Restart the service.
```
systemctl restart backend
```

### PlayBook Execution
```
[ ec2-user@ip-172-31-18-1 /expense ]$ sudo vi BE.yml

[ ec2-user@ip-172-31-18-1 /expense ]$ ansible-playbook -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 BE.yml

PLAY [Setup MySql] *********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [be.step-into-iot.cloud]

TASK [Disable Default NodeJS] **********************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Enable NodeJS 20] ****************************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Install NodeJS 20 & MySql] *******************************************************************************************
ok: [be.step-into-iot.cloud] => (item=nodejs)
changed: [be.step-into-iot.cloud] => (item=mysql)

TASK [Create User] *********************************************************************************************************
ok: [be.step-into-iot.cloud]

TASK [Create App Directory] ************************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Download backend code] ***********************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [unzip backend code] **************************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Install npm dependencies] ********************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Copy backend service] ************************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [install python mysql dependencies] ***********************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Import data into mysql] **********************************************************************************************
changed: [be.step-into-iot.cloud]

TASK [Daemon reload] *******************************************************************************************************
ok: [be.step-into-iot.cloud]

TASK [Start and Enable Backend Service] ************************************************************************************
changed: [be.step-into-iot.cloud]

PLAY RECAP *****************************************************************************************************************
be.step-into-iot.cloud     : ok=14   changed=11   unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Check weather BE Started or not
```
[ ec2-user@ip-172-31-23-148 ~ ]$ netstat -lntp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::8080                 :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
```

### Check weather Schema Loaded or Not
```
[ ec2-user@ip-172-31-21-183 ~ ]$ mysql -u root -p
Enter password:

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
| transactions       |
+--------------------+
5 rows in set (0.02 sec)

mysql> exit
Bye
```