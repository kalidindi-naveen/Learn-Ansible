### Generate SSH Key & Upload to PROJECT METADATA
```
- Copy Public Key to PROJECT METADATA in compute engine
  ex: ssh-rsa ....... naveen
```
### Setup Ansible
```
We are Using Ubuntu for Ansible-Server
1. Install Ansible
2. Copy Private Key on to Ansible-Server
3. Test Connection
```
### Login -- VM
```
ssh -i naveen.pem naveen@35.208.249.227
naveen@ansible-master:~$
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
naveen@ansible-master:~$ cd /expense

naveen@ansible-master:/expense$ sudo mv /home/naveen/naveen.pem .

naveen@ansible-master:/expense$ chmod 600 naveen.pem

naveen@ansible-master:/expense$ ls -al
total 12
drwxr-xr-x  2 root   root   4096 Apr 30 14:43 .
drwxr-xr-x 18 root   root   4096 Apr 30 14:32 ..
-rw-------  1 naveen naveen 1680 Apr 29 02:28 naveen.pem
```
### Purchase Domain in Hostlinger
```
step-into-iot.space
```
### Create 3 VM's with RockyLinux9

### Cloud DNS
```
- Create Hosted Zone
- Update Namespace in Hostlinger with CloudDNS
- Create few 'A' Records based on choice 
    10.128.15.218 -- be.step-into-iot.space
    10.128.15.217 -- db.step-into-iot.space
    10.128.15.219 -- fe.step-into-iot.space
    34.31.98.194 -- step-into-iot.space
```
### Add 3 VM's to inventory.ini
```
naveen@ansible-master:/expense$ sudo vi inventory.ini

naveen@ansible-master:/expense$ cat inventory.ini
[db]
db.step-into-iot.space
[be]
be.step-into-iot.space
[fe]
fe.step-into-iot.space
```
### Ping 3 VM's with PlayBook
```
naveen@ansible-master:/expense$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 00.ping.yml

PLAY [Ping Group's] ********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [fe.step-into-iot.space]
ok: [be.step-into-iot.space]
ok: [db.step-into-iot.space]

TASK [ping group's] ********************************************************************************************************
ok: [fe.step-into-iot.space]
ok: [be.step-into-iot.space]
ok: [db.step-into-iot.space]

PLAY RECAP *****************************************************************************************************************
be.step-into-iot.space     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
db.step-into-iot.space     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
fe.step-into-iot.space     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```