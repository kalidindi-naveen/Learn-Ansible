### Task10
#### Conditions
```
naveen@ansible-master:/opt$ sudo vi 10.condition.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 10.condition.yml

PLAY [Conditions] **********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Check user exists or not] ********************************************************************************************
fatal: [10.128.15.213]: FAILED! => {"changed": true, "cmd": ["id", "nk"], "delta": "0:00:00.004340", "end": "2024-04-30 16:44:09.188261", "msg": "non-zero return code", "rc": 1, "start": "2024-04-30 16:44:09.183921", "stderr": "id: ‘nk’: no such user", "stderr_lines": ["id: ‘nk’: no such user"], "stdout": "", "stdout_lines": []}
...ignoring

TASK [Print user info] *****************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "User info: {'changed': True, 'stdout': '', 'stderr': 'id: ‘nk’: no such user', 'rc': 1, 'cmd': ['id', 'nk'], 'start': '2024-04-30 16:44:09.183921', 'end': '2024-04-30 16:44:09.188261', 'delta': '0:00:00.004340', 'failed': True, 'msg': 'non-zero return code', 'stdout_lines': [], 'stderr_lines': ['id: ‘nk’: no such user']}"
}

TASK [create user] *********************************************************************************************************
changed: [10.128.15.213]

TASK [say Hello] ***********************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Hello"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=5    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=1
```

### Ansible we can't create functions, we can use existing filters
```
Prompt -- take it as string
Ansible is strict, it can't automatically ,convert text to string ?? use ansible filter (consider string as 0)

naveen@ansible-master:/opt$ sudo vi 11.filters.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 11.filters.yml
please enter number: dev

PLAY [Check Number] ********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [Number is less than 10] **********************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Given number dev is less than 10"
}

TASK [Number is greater than or equal to 10] *******************************************************************************
skipping: [10.128.15.213]

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=1    rescued=0    ignored=0
```
### More Filters
```
naveen@ansible-master:/opt$ sudo vi 12.filters-1.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 12.filters-1.yml

PLAY [default value] *******************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [print default value] *************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Hello Ansible"
}

PLAY [upper case] **********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [convert into uppercase] **********************************************************************************************
ok: [10.128.15.213] => {
    "msg": "HELLO, GOOD MORNING"
}

PLAY [lower case] **********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [convert into lowercase] **********************************************************************************************
ok: [10.128.15.213] => {
    "msg": "hello, good morning"
}

PLAY [remove duplicates] ***************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [remove duplicates] ***************************************************************************************************
ok: [10.128.15.213] => {
    "msg": [
        1,
        2,
        3,
        4,
        5
    ]
}

PLAY [print min and max] ***************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [print min and max] ***************************************************************************************************
ok: [10.128.15.213] => {
    "msg": " min age: 25, max age: 89"
}

PLAY [convert dictionary into items/list] **********************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [before convert] ******************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Before convert: {'Course': 'Ansbile', 'Trainer': 'naveen', 'Duration': '120hr'}"
}

TASK [after convert] *******************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "After convert: [{'key': 'Course', 'value': 'Ansbile'}, {'key': 'Trainer', 'value': 'naveen'}, {'key': 'Duration', 'value': '120hr'}]"
}

PLAY [convert items to dictionary] *****************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [before convert] ******************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "Before convert: [{'key': 'Course', 'value': 'Ansbile'}, {'key': 'Trainer', 'value': 'naveen'}, {'key': 'Duration', 'value': '120hr'}]"
}

TASK [after convert] *******************************************************************************************************
ok: [10.128.15.213] => {
    "msg": "After convert: {'Course': 'Ansbile', 'Trainer': 'naveen', 'Duration': '120hr'}"
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=16   changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Loops
```
naveen@ansible-master:/opt$ sudo vi 13.loops.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 13.loops.yml

PLAY [demo on loops] *******************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [print names] *********************************************************************************************************
ok: [10.128.15.213] => (item=linux) => {
    "msg": "Hello linux "
}
ok: [10.128.15.213] => (item=shell) => {
    "msg": "Hello shell "
}
ok: [10.128.15.213] => (item=ansible) => {
    "msg": "Hello ansible "
}

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Install Packages using loop
```
naveen@ansible-master:/opt$ sudo vi 14.install-pkg-loop.yml
naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 14.install-pkg-loop.yml

PLAY [install packages] ****************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [install packages] ****************************************************************************************************
ok: [10.128.15.213] => (item=nginx)
ok: [10.128.15.213] => (item=postfix)

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```
### ### Install Packages with state using loop
```
naveen@ansible-master:/opt$ sudo vi 15.install-pkg.state.yml

naveen@ansible-master:/opt$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 15.install-pkg.state.yml

PLAY [install packages] ****************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [10.128.15.213]

TASK [install packages] ****************************************************************************************************
changed: [10.128.15.213] => (item={'name': 'nginx', 'state': 'absent'})
changed: [10.128.15.213] => (item={'name': 'postfix', 'state': 'absent'})

PLAY RECAP *****************************************************************************************************************
10.128.15.213              : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

### Ping Multiple Hosts
```
naveen@ansible-master:/expense$ cat inventory.ini
[db]
db.step-into-iot.space

[be]
be.step-into-iot.space

[fe]
step-into-iot.space

naveen@ansible-master:/expense$ sudo vi 00.ping.yml

naveen@ansible-master:/expense$ cat 00.ping.yml
- name: Ping Group's
  hosts:
    - db
    - be
  tasks:
  - name: ping group's
    ansible.builtin.ping:

naveen@ansible-master:/expense$ ansible-playbook -i inventory.ini -u naveen --private-key=naveen.pem 00.ping.yml

PLAY [Ping Group's] ********************************************************************************************************

TASK [Gathering Facts] *****************************************************************************************************
ok: [be.step-into-iot.space]
ok: [db.step-into-iot.space]

TASK [ping group's] ********************************************************************************************************
ok: [be.step-into-iot.space]
ok: [db.step-into-iot.space]

PLAY RECAP *****************************************************************************************************************
be.step-into-iot.space     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
db.step-into-iot.space     : ok=2    changed=0    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```