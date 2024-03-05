# Ansible:- Use Cases

## Enforce security guidelines: 
Rules are rules. It’s best to automate in an effort to achieve strict security standards.
Patching :-
  - shell shock
  - Heart bleed 

## Monitor configuration drift:
 Use check mode with Ansible tasks to enforce desired settings and see if your configuration has drifted.

## Database binary patching: Several databases use outdated binary sets.
  Patch the binaries in accordance with the release of the latest patch.

## Instance provisioning:
Use modules for several cloud providers to create new instances and tailor their configuration

## Weekly system reboot: There’s nothing worse than doing the same thing for 8 hours a day!
 Eliminate repetitive, manual processes with automation

## Command blaster: Remarkably easy to write, you can run commands
 across your environment for any number of server

 Cisco Routers: Works with Cisco routers

## Ansible has several modules for managing Docker;
 a few of these are docker_image, docker_container, and docker_service

 Example:
 ### Basic provisioning example aws ec2 instance
 ```
- name: Ansible test
hosts: localhost
tasks:
- name: launching AWS instance using Ansible
ec2:
key_name: aws_instance_Ansible
instance_type: t2.micro
image: ami-XXXX
region: us-east-2
wait: yes
group: Ansible
count: 1
vpc_subnet_id: default
assign_public_ip: yes
aws_access_key: ***********xxxxxxxx
Aws_secret_key: ***********xxxxxxxx
```
