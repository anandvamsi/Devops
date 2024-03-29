# Ansible Day2

- Ansible installation
- what are inventories
- inventory files
- Configuring inventories
- Inventories var files
- Ansible connection types.

## Ansible installation

```bash
sudo yum update
sudo yum install epel-release
sudo yum install ansible
```

## Ansible installation in Amazon Linux

```bash
amazon-linux-extras install epel
yum install ansible
```

## Ansible Inventory
Ansible works against multiple systems in your infrastructure at the same time.
It does this by selecting portions of systems listed in Ansible’s inventory file, which defaults to being saved in the location /etc/ansible/hosts
can specify a different inventory file using the -i <path> option on the command line.

## Setting up the invetory--> In Master server
```
/etc/ansible/hosts
[webserver]
slaves_IP
```

Example:-
```bash
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```

Note:: Its ok to put more than one system in one or more groups.
for instance a server could be both a webserver and a dbserver


Security reason if ssh port is different we need to mention that along with hostname
```bash
one.example.com:678
```



## Other examples of inventory example
```
[webservers]
www[01:50].example.com

[databases]
db-[a:f].example.com
```
## Ansible host with connection variables
```bash
[targets]
localhost              ansible_connection=local
other1.example.com     ansible_connection=ssh        ansible_user=user1
other2.example.com     ansible_connection=ssh        ansible_user=user2
```

## Inventory with variables.
```bash
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=303 maxRequestsPerChild=909
```


## Group of groups and variables
```bash
[atlanta]
host1
host2

[raleigh]
host2
host3

[southeast:children]
atlanta
raleigh

[southeast:vars]
some_server=foo.southeast.example.com
halon_system_timeout=30
self_destruct_countdown=60
escape_pods=2
```


## Other examples for iterations
```bash
[webservers]
cobweb
webbing
weber

webservers[0]       # == cobweb
webservers[-1]      # == weber
webservers[0:2]     # == webservers[0],webservers[1]
                    # == cobweb,webbing
webservers[1:]      # == webbing,weber
webservers[:3]      # == cobweb,webbing,we
```


## Syntax for executing Ansible CLI
```bash
ansible target -i inventory -m module -a "module options"
```



## First CLI to check the connectivity
```bash
~$ ansible all -m ping
192.168.150.129 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}

```

## To list hosts of the group
```bash

ansible servers --list-hosts
  hosts (3):
    192.168.1.14
    192.168.1.15
    192.168.1.16
```

## To begin individual server
```bash
ansible -m ping 192.168.150.129
```
## Another Method
```bash
ansible newserver -m ping --limit "192.168.1.14"
```

## Using the wildcard starting with 192
```bash
ansible newserver -m ping --limit "192.*"
```

## if you want to login to login to different user
```bash 
ansible all -m ping -u <username>
```

## Using Ask password method
```bash
ansible all -m ping --ask-pass
SSH password:
192.168.150.129 | SUCCESS => {
    "ansible_facts": {
        "discovered_interpreter_python": "/usr/bin/python"
    },
    "changed": false,
    "ping": "pong"
}
```

## Shell module
```
ansible all -m shell -a "ls -l /tmp"
ansible all -m shell -a "hostname"
ansible all -m shell -a "uptime"
ansible all -m shell -a "date"
```

## File based commands
```bash
ansible all -m file "dest = /tmp/newfolder  mode = 777 owner = user1 group = user1 state = directory" 
ansible all -m file -a "dest=/tmp/test mode=755 owner=ansibleuser group=ansibleuser state=directory"
```
## copying a single file
```bash
 ansible all -m copy -a "src=/etc/hosts dest=/tmp/hosts
 ```
## copying with content
 ```bash
 ansible server2 -m copy -a "content='Hello, My name is deepak' dest=~/ctrl_file"
 ```

## To create a file
```bash
 ansible server2 -m file -a "path=/tmp/demo_2.txt state=touch mode=755"
```
## To delete the directory
```bash
ansible webservers -m file -a "dest=/path/to/c state=absent
```

## To Create a directory
```bash
ansible server2 -m file -a "path=/tmp/dir_1.txt state=directory"
```

## To remove a directory
```bash
ansible server2 -m file -a "path=/tmp/dir_1.txt state=absent"
```

## Commands with sudo privilages
```bash
ansible server2 -m command -a "fdisk -l"
server2 | CHANGED | rc=0 >>
fdisk: cannot open /dev/xvda: Permission denied
```

## Using sudo privilages
```bash
ansible server2 -m command -a "fdisk -l" --become --ask-become-pass
```

## To create a user
```bash
ansible newserver  -m user -a "name=raj1 state=present" -b -K 
- b stand for become
-K will as the sudo password
```

## Installing a package
```bash
ansible server2 -m yum -a "name=git state=latest"
```


## Package based <becoming root user>

Before this module make sure apt-get update
```bash
ansible --become -K all -m  apt -a "name=apache2 state=present"
```

## Install the latest version
```bash
ansible all -m apt -a "name=screen state=latest" --become -K
```

## TO remove a package
```bash
ansible all -m apt -a "name=screen state=absent" --become -K
```

## Deployment
```bash
ansible all -m git -a "repo=https://github.com/trekhleb/javascript-algorithms.git dest=/tmp/repo
```


## Services
```bash
ansible webservers -m service -a "name=apache2 state=started" --become -K
ansible webservers -m service -a "name=apache2 state=stopped" --become -K
ansible webservers -m service -a "name=apache2 state=restarted" --become -K
```

## custom inventory file
```bash
ansible all -m ping -i my_custom_inventory
```

## Connections Types of Ansible.
```Ansible Local```: Ansible Local refers to executing Ansible tasks on the local machine rather than remotely connecting to other hosts. This can be useful for tasks that need to be performed on the machine where Ansible is being run, such as setting up local configurations or managing the local environment.

```Ansible SSH```: Ansible SSH is the method Ansible uses to connect to remote hosts and execute tasks. Ansible relies on SSH (Secure Shell) to securely communicate with and manage remote servers. It uses SSH keys for authentication and typically requires SSH access to be configured on the remote hosts.

