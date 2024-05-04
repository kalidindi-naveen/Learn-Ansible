### Setup MySql
```
1. Install mysql-server.
2. Enable & Start it. 
3. Setup Root Password.
```

### Manual Step's
```
dnf install mysql-server -y
systemctl enable mysqld
systemctl start mysqld
mysql_secure_installation --set-root-pass ExpenseApp@1
```

### Execute PlayBook
```
[ ec2-user@ip-172-31-18-1 /expense ]$ sudo vi DB.yml

[ ec2-user@ip-172-31-18-1 /expense ]$ ansible-playbook -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 DB.yml
Enter PASSWORD:

PLAY [Setup MySql] *********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [db.step-into-iot.cloud]

TASK [Install MySql-Server] ************************************************************************************************
ok: [db.step-into-iot.cloud]

TASK [Enable & Start MySql-Server] *****************************************************************************************
ok: [db.step-into-iot.cloud]

TASK [Attempt to login to MySQL] *******************************************************************************************
changed: [db.step-into-iot.cloud]

TASK [Print Output] ********************************************************************************************************
ok: [db.step-into-iot.cloud] => {
    "msg": "Output: {'changed': True, ......, 'failed': False}"
}

TASK [setup root password] *************************************************************************************************
skipping: [db.step-into-iot.cloud]

PLAY RECAP *****************************************************************************************************************
db.step-into-iot.cloud     : ok=5    changed=1    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```

### Check MySql Running or Not
```
[ ec2-user@ip-172-31-21-183 ~ ]$ netstat -lntp

Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::33060                :::*                    LISTEN      -
tcp6       0      0 :::3306                 :::*                    LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
```