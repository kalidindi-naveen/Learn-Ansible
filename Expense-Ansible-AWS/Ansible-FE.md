### Manual Step's
```
dnf install nginx -y 
systemctl enable nginx
systemctl start nginx
rm -rf /usr/share/nginx/html/*
curl -o /tmp/frontend.zip https://expense-builds.s3.us-east-1.amazonaws.com/expense-frontend-v2.zip
cd /usr/share/nginx/html
unzip /tmp/frontend.zip
```
### Create Nginx Reverse Proxy Configuration
```
vim /etc/nginx/default.d/expense.conf
```
```
proxy_http_version 1.1;

location /api/ { proxy_pass http://be.step-into-iot.cloud:8080/; }

location /health {
  stub_status on;
  access_log off;
}
```
```
systemctl restart nginx
```

### PlayBook Execution
```
[ ec2-user@ip-172-31-18-1 /expense ]$ sudo vi FE.yml

[ ec2-user@ip-172-31-18-1 /expense ]$ ansible-playbook -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 FE.yml

PLAY [Setup Nginx] *********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [step-into-iot.cloud]

TASK [Install Nginx] *******************************************************************************************************
changed: [step-into-iot.cloud]

TASK [Start Nginx & Enable] ************************************************************************************************
changed: [step-into-iot.cloud]

TASK [Download Frontend code] **********************************************************************************************
changed: [step-into-iot.cloud]

TASK [unzip Frontend code] *************************************************************************************************
changed: [step-into-iot.cloud]

TASK [copy backend conf] ***************************************************************************************************
changed: [step-into-iot.cloud]

TASK [Restart Nginx] *******************************************************************************************************
changed: [step-into-iot.cloud]

PLAY RECAP *****************************************************************************************************************
step-into-iot.cloud        : ok=7    changed=6    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Check weather FE running or not
```
[ ec2-user@ip-172-31-24-182 ~ ]$ netstat -lntp

Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      -
tcp6       0      0 :::22                   :::*                    LISTEN      -
tcp6       0      0 :::80                   :::*                    LISTEN      -
```