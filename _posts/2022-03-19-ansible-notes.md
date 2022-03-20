---
layout: post
title: my ansible notes
---


### Useful Commands

```
# oracle_cloud is a group of servers
ansible oracle_cloud --list-host
    # -a means --arg
    # -m means --module

# simple commands
ansible oracle_cloud -m setup # list infomation
ansible oracle_cloud -a "free -h"

# use module ping check server availability
ansible all -m ping

# run script
ansible ubuntu_utm -m script -a "fix_ripgrep_apt_error.sh"

# use module
# install chrony by yum 
ansible oracle_cloud -b -m yum -a "name=chrony state=present"
# ansible multi -b -m service -a "name=chronyd state=started enabled=yes"
```

### Role

```
# new role
ansible-galaxy role init roles/media_server

# import/include role

```



### Debug

```
{% raw %}
# show variables or message
- debug: var=ansible_user
- debug: msg={{ansible_user}}
{%endraw%}

# show ansible execution details
ansible-playbook provision.yml -vvvv


# inspect config
ansible-config view
ansible-config dump

```

### others

- Ask sudo password

```
ansible-playbook -i inventory my.yml \--extra-vars 'ansible_become_pass=YOUR-PASSWORD-HERE' 

ansible-playbook -i inventory my.yml -kK # -k ask for password# -K ask for become password
```

- faster by enable ssh pipeline

```
# ~/.ansible.cfg
[ssh_connection]
# may comment defaults requiretty option in /etc/sudoer
spipeling = True
```




references:
* https://github.com/ansible/ansible-examples
* https://github.com/calvinbui/ansible-monorepo
* https://www.redhat.com/sysadmin/faster-ansible-playbook-execution
* https://zwischenzugs.com/2021/08/27/five-ansible-techniques-i-wish-id-known-earlier