# Ansible commandline 
-----------

## Running Adhoc commands structure 
Syntax
```bash
ansible host_pattern -m module_name -a "module_options"
```
- Modules are reusable,standalone script that can be used by Ansible API or playbooks.


## CommandLine flags
```bash
-i :- inventory
-u :- remote_user
-b :- become
-become_user  :- become_uesr
-K, --ask-become-pass :- become_ask_pass
-f :- forks 
Ansible abc -a "/sbin/reboot" -f 12
```

```Usecases```::
Ansible ad-hoc commands are ideal to perform tasks that are not executed fre-quently such us getting servers
uptime, retrieving system information, etc.

## To get the Module list
```bash
ansible-doc -l
ansible-doc ping
ansible-doc file
```

#To list the hosts of the Ansible inventory
ansible development -i myhosts --list-hosts
ansible production -i myhosts --list-hosts

## Hello world of Ansible:- To test the connectivity
```bash
ansible all -m ping
```

## To execute a command 
```bash
ansible node1 -m command -a "uptime"
```

## Shell Module
command module do have an issues with pipes and symbols
You can’t use piping or redirection with the command module, To resolve it we need to use shell module.
```bash
ansible node2 -m shell -a "lscpu | head -n 5"
```

## Raw Module
Raw module which can handles 
- Simple commands
- Run commands with pipes or redirections
  
```bash
ansible node4 -m raw -a "cat /etc/os-release"
```

## File Module
```bash
ansible all -m file -a "path=/tmp/foo.conf mode=0664 owner=anand state=touch"
```



## To install a Package
```bash
ansible -i hosts all -m yum -a "name=kpartx state=latest"
ansible -i hosts all -a "uptime"
```
