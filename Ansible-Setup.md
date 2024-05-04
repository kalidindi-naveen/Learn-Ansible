### GCP
```
We are having Ubuntu Systems
1. Install Ansible
2. Copy Private Key on to Ansible
3. Test Connection
```

### AWS
```
We are having Redhat Systems
1. Install Ansible
2. Copy Private Key on to Ansible 
    --OR--
    We can use username and password
3. Test Connection
```

### Login -- VM's
```
ssh -i naveen.pem naveen@35.208.249.227
naveen@ansible-master:~$

ssh -i naveen.pem naveen@35.194.33.71
naveen@ansible-node:~$
```

### Setup Ansible -- Ubuntu
```
naveen@ansible-master:~$ sudo apt update -y

naveen@ansible-master:~$ sudo apt install ansible -y

naveen@ansible-master:~$ ansible --version
ansible [core 2.14.3]
  config file = None
  configured module search path = ['/home/naveen/.ansible/plugins/modules', '/usr/share/ansible/plugins/modules']
  ansible python module location = /usr/lib/python3/dist-packages/ansible
  ansible collection location = /home/naveen/.ansible/collections:/usr/share/ansible/collections
  executable location = /usr/bin/ansible
  python version = 3.11.2 (main, Mar 13 2023, 12:18:29) [GCC 12.2.0] (/usr/bin/python3)
  jinja version = 3.1.2
  libyaml = True
```

### Copy Key & Set Permissions
```
naveen@ansible-master:~$ cd /opt

naveen@ansible-master:/opt$ sudo mv /home/naveen/naveen.pem .

naveen@ansible-master:/opt$ chmod 600 naveen.pem

naveen@ansible-master:/opt$ ls -al
total 12
drwxr-xr-x  2 root   root   4096 Apr 30 14:43 .
drwxr-xr-x 18 root   root   4096 Apr 30 14:32 ..
-rw-------  1 naveen naveen 1680 Apr 29 02:28 naveen.pem
```

### Adhoc commands
```
ansible -i 172.31.16.150, all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ping

ansible -i 10.128.15.213, all -u naveen --private-key=naveen.pem -m ping 

-m -- after -m we need to use name of module
-a -- arguments
-b -- become root
-u -- username
```

### Ping Ansible Node
```
naveen@ansible-master:/opt$ ansible -i 10.128.15.213, all -u naveen --private-key=naveen.pem -m ping
10.128.15.213 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "changed": false,
    "ping": "pong"
}
```

### Install Nginx in Ansible Node
```
naveen@ansible-master:/opt$ ansible -i 10.128.15.213, all -u naveen --private-key=naveen.pem -b -m apt -a "name=nginx state=present"
10.128.15.213 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python3"
    },
    "cache_update_time": 1713207325,
    "cache_updated": false,
    "changed": false
}
```
### Setup inventory.ini
```
naveen@ansible-master:/opt$ sudo vi inventory.ini

naveen@ansible-master:/opt$ cat inventory.ini
[web]
10.128.15.213
```

### Task1
#### Write Ansible Playbook to ping web group 
```
naveen@ansible-master:/opt$ sudo vi 00.ping.yml

naveen@ansible-master:/opt$ ls
00.ping.yml  inventory.ini  naveen.pem

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 00.ping.yml

PLAY [Ping Web Group] **************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [10.128.15.213]

TASK [ping web group] **************************************************************************************************
ok: [10.128.15.213]

PLAY RECAP *************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task2
#### Write Ansible Playbook to Install Nginx 
```
naveen@ansible-master:/opt$ sudo vi 01.nginx.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 01.nginx.yml

PLAY [Install Nginx] ***************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************
ok: [10.128.15.213]

TASK [Install Nginx] ***************************************************************************************************
ok: [10.128.15.213]

TASK [Start Nginx & Enable] ********************************************************************************************
ok: [10.128.15.213]

PLAY RECAP *************************************************************************************************************
10.128.15.213              : ok=3    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Checking nginx status in ansible-node
```
naveen@ansible-node:~$ systemctl status nginx
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; preset: enabled)
     Active: active (running) since Tue 2024-04-30 15:22:37 UTC; 12s ago
```

### Task3
#### Write ansible playbook to connect to localhost
```
naveen@ansible-master:/opt$ sudo vi 02.multi-play.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 02.multi-play.yml

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Hi From Web Group"
}

PLAY [Play2] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [localhost]

TASK [Say Hi From Localhost] ***********************************************************************************************
ok: [localhost] => {
    "msg": "Hi From localhost"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
localhost                  : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task4
#### Write ansible playbook to use Variables @ Play Level
```
naveen@ansible-master:/opt$ sudo vi 03.var.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 03.var.yml

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm Naveen with Age:23"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task5
#### Write ansible playbook to use Variables @ task level
```
naveen@ansible-master:/opt$ sudo vi 04.var-task.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 04.var-task.yml

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm Naveen with Age:18"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task6
#### Write ansible playbook to use Variables using file
```
naveen@ansible-master:/opt$ sudo vi var-file.yml

naveen@ansible-master:/opt$ cat var-file.yml
NAME: "Naveen"
AGE: 23

naveen@ansible-master:/opt$ sudo vi 05.var-with-file.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 05.var-with-file.yml

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm Naveen with Age:23"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task7
#### Write ansible playbook to ask user input
```
naveen@ansible-master:/opt$ sudo vi 06.var-prompt.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 06.var-prompt.yml
Enter NAME: NK
Enter AGE:

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm NK with Age:24"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task8
#### Write ansible playbook to use Variables @ inventory level
```
naveen@ansible-master:/opt$ sudo vi inventory.ini

naveen@ansible-master:/opt$ cat inventory.ini
[web]
10.128.15.213

[web:vars]
NAME="Naveen-2.0"
AGE=24

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 07.var-inventory.yml

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm Naveen-2.0 with Age:24"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Task9
#### Write ansible playbook to use Variables as arguments
```
naveen@ansible-master:/opt$ sudo vi 08.var-arg.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 08.var-arg.yml -e NAME="NK-3.0" -e AGE=21

PLAY [Play1] ***************************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Say Hi From Web] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "I'm NK-3.0 with Age:21"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Variables-Preference
```
# 1. arguments
# 2. Task level
# 3. variable files
# 4. Prompt
# 5. Play level
# 6. inventory
# 7. role level
```

### Data Types
```
naveen@ansible-master:/opt$ sudo vi 09.data-types.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 09.data-types.yml

PLAY [Data Types] **********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [print variables] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Course: DevOps with AWS, Tools covered: ['Linux', 'Shell', 'Ansible'], Experience i{'DevOps': 3, 'AWS': 2, 'Docker': 1}, is real project: True"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```