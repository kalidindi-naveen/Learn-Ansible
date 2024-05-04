### Launch 4 VM's
```
- Select us-east-1
- Select Custom Image -- RHEL-9-DevOps-Practice
- Select AMI -- ami-090252cbe067a9e58
- username: ec2_user
- Password: DevOps321
```

### Create Route53 HostedZone
```
step-into-iot.cloud

- Create Hosted Zone
- Update Namespace in Hostlinger with Route53
- Create few 'A' Records based on choice
```

### Add 3 VM's to inventory.ini
```
[ ec2-user@ip-172-31-18-1 /expense ]$ sudo vi inventory.ini

[ ec2-user@ip-172-31-18-1 /expense ]$ cat inventory.ini
[db]
db.step-into-iot.cloud
[be]
be.step-into-iot.cloud
[fe]
step-into-iot.cloud
```

### Ping 3 VM's with Ansible-Server
```
[ ec2-user@ip-172-31-18-1 /expense ]$ ansible all -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping 
db.step-into-iot.cloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
be.step-into-iot.cloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
step-into-iot.cloud | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```
### Ping VM's with Ansible-Playbook
```
[ ec2-user@ip-172-31-18-1 /expense ]$ ansible-playbook -i inventory.ini -e ansible_user=ec2-user -e ansible_password=DevOps321 ping.yml

PLAY [Ping VM's] ***********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [step-into-iot.cloud]
ok: [be.step-into-iot.cloud]
ok: [db.step-into-iot.cloud]

TASK [ping VM's] ***********************************************************************************************************
ok: [step-into-iot.cloud]
ok: [be.step-into-iot.cloud]
ok: [db.step-into-iot.cloud]

PLAY RECAP *****************************************************************************************************************
be.step-into-iot.cloud     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
db.step-into-iot.cloud     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
step-into-iot.cloud        : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```