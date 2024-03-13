# Ansible Day 3
- Setting up passwordless ssh from Ansible_server to clients
- Ansible facts
- Introduction to YAML file
- Creating first playbooks
- Setting log_path in ansible
- Module reference 
- Dry Run of playbooks
- Playbooks with variables
- Tags
- Extra variables
- variables precedence


#Setting up passwordless ssh
>>Execute below command from Ansible server
>>Replace all with client IP
ssh-copy-id anand@<ClientIP>
/usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/anand/.ssh/id_rsa.pub"
/usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
/usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
anand@192.168.1.15's password:



## Ansible facts
Ansible facts are nothing but system properties or pieces of information about remote nodes that you have connected to. This information includes 
- System architecture, 
- OS version
- BIOS information
- system time
- date
- system uptime
- IP address
- Hardware information.

Ansible facts are handy in helping the system administrators which operations to carry out, for instance, depending on the operating system, they are able to know which software packages need to be installed, and how they are to be configured, etc

```bash
 ansible all  -m setup

 ansible all -m setup
192.168.150.129 | SUCCESS => {
    "ansible_facts": {
        "ansible_all_ipv4_addresses": [
            "192.168.150.129"
        ],
        "ansible_all_ipv6_addresses": [
            "fe80::20c:29ff:fee2:3d02"
        ],
        "ansible_apparmor": {
            "status": "enabled"
        },
        "ansible_architecture": "x86_64",
        "ansible_bios_date": "07/31/2013",
        "ansible_bios_version": "6.00",
        "ansible_cmdline": {
            "BOOT_IMAGE": "/boot/vmlinuz-3.13.0-24-generic",
            "find_preseed": "/preseed.cfg",
            "noprompt": true,
            "quiet": true,
            "ro": true,
            "root": "UUID=715c9efe-0f8f-42fc-9515-4b0dc0e64854"
        },

.................................
..................................
```

Use case:
While deploying a NTP application/hosts file

<html>
<center>
   <h1> The hostname of this webserver is {{ ansible_hostname }}</h1>
   <h3> It is running on {{ ansible_os_family}}system </h3>
</center>
</html>


## Facts section data:
```bash
ansible_architecture
ansible_bios_date
ansible_bios_version
ansible_date_time
ansible_machine
ansible_memefree_mb
ansible_os_family
ansible_selinux
```

## using filters
```bash
ansible localhost  -m setup -a "filter=ansible_distribution*"
ansible localhost -m setup -a "filter=*ip*"
```
```bash
ansibleuser@ubuntu-master:~/playbooks$ ansible localhost  -m setup -a "filter=ansible_distribution*"
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_distribution": "Ubuntu",
        "ansible_distribution_file_parsed": true,
        "ansible_distribution_file_path": "/etc/os-release",
        "ansible_distribution_file_variety": "Debian",
        "ansible_distribution_major_version": "14",
        "ansible_distribution_release": "trusty",
        "ansible_distribution_version": "14.04"
    },
    "changed": false
}
ansibleuser@ubuntu-master:~/playbooks$ ansible localhost  -m setup -a "filter=ansible_selinux*"
localhost | SUCCESS => {
    "ansible_facts": {
        "ansible_selinux": {
            "status": "Missing selinux Python library"
        },
        "ansible_selinux_python_present": false
    },
    "changed": false
}
```

## Example of Including ansible facts in a playbook
```bash
- name: Example Playbook
  hosts: all
  tasks:
    - name: Display OS type
      debug:
        msg: "The operating system is {{ ansible_facts['os_family'] }}"
```



## Creating custom facts
```bash
mkdir -p /etc/ansible/facts.d
vim /etc/ansible/facts.d/date_time.fact

#!/bin/bash
DATE=`date`
echo "{\"date\" : \"${DATE}\"}"

chmod +x /etc/ansible/facts.d/date_time.fact

ansible localhost -m setup | grep date
        "ansible_bios_date": "07/31/2013",
        "ansible_date_time": {
            "date": "2021-01-12",
```




##Setting log_path for ansible
------------------------------------
logpath is more like saving the ansible execution logs to log file , by default logs are not saved in any of the 

set the log_path variable in ansible configration file 
books$ grep log_path /etc/ansible/ansible.cfg
log_path = /var/log/ansible.log

#Make sure it has right permission to the file

if the above method didt work follow this:-
# sudo touch /var/log/ansible.log
#sudo groupadd ansible
# sudo chown root:ansible /var/log/ansible.log
# sudo chmod 775 /var/log/ansible.log
# sudo usermod sudeep -aG ansible
You need to signout and sign in before executing the playbook.


## Ansible Playbook
An Ansible playbook is a configuration file written in YAML (YAML Ain't Markup Language) format used to define a set of tasks to be executed by Ansible, an open-source automation tool. Playbooks are at the heart of Ansible's configuration management system and are used to automate tasks such as provisioning servers, deploying applications, configuring software, and more.

## Example of Ansible playbook
```bash
---
- name: Example Playbook
  hosts: web_servers
  become: true
  tasks:
    - name: Ensure Apache is installed
      yum:
        name: httpd
        state: present

    - name: Ensure Apache is running
      service:
        name: httpd
        state: started
```
## Dissect the playbook
```bash
```name``` field specifies the name of the playbook.
```hosts``` field specifies the target hosts where the playbook tasks will be executed. These hosts can be defined in an Ansible inventory file.
```become``` field specifies whether to execute tasks with root privileges (using sudo).
```Tasks``` section contains a list of tasks to be executed.
Each task consists of a name describing the task and a module (such as yum or service) with its corresponding parameters.
```


### Syntax Check and Dry Run of playbooks
----------------------------
```bash
ansible-playbook playbooks/PLAYBOOK_NAME.yml --syntax-check
```

## DryRun
Check mode is just a simulation, and if you have steps that use conditionals that depend on the results of prior commands, it may be less useful for you. However it is great for one-node-at-time basic configuration management use case
```bash
ansible-playbook  WithItemMultiPathFileCopy.yaml  --check
```



## Tags

In Ansible, tags allow you to selectively ```run specific tasks or plays within a playbook```. 
Tags are defined within tasks or plays and can be applied to include or exclude those tasks or plays during playbook execution. Here are some examples of using tags in Ansible
```bash
- name: Main playbook
  hosts: all
  tags: main

  tasks:
    - name: Task 1
      debug:
        msg: "This is Task 1"
      tags:
        - task1
    - name: Task 2
      debug:
        msg: "This is Task 2"
      tags:
        - task2      
```



```bash
ansible-playbook playbooks/PLAYBOOK_NAME.yml --tags 'task1'
ansible-playbook playbooks/PLAYBOOK_NAME.yml --skip-tags 'sudoers'
```
