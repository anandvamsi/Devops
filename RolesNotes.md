# Ansible Roles :- More like refactor the playbooks

Ansible roles are a way to organize and structure your Ansible code in a reusable and modular fashion. 
They provide a means to break down complex automation tasks into smaller, more manageable components


Roles help in organizing your Ansible playbooks into a directory structure that is easy to understand and maintain. They encourage code reuse, modularity, and best practices in configuration management. 
They can be shared with others through Ansible Galaxy or other means, allowing for a thriving ecosystem of reusable automation components.

A role typically contains the following components:

Directory structure of the role
-------------------------------
- ```defaults``` –  Includes default values for variables of the role. Here we define some sane default variables, but they have the lowest priority and are usually overridden by other methods to customize the role.
- ```files```  – Contains static and custom files that the role uses to perform various tasks.
- ```handlers``` – A set of handlers that are triggered by tasks of the role. 
- ```meta``` – Includes metadata information for the role, its dependencies, the author, license, available platform, etc.
- ```tasks``` – A list of tasks to be executed by the role. This part could be considered similar to the task section of a playbook.
- ```templates``` – Contains Jinja2 template files used by tasks of the role.
- ```tests``` – Includes configuration files related to role testing.
- ```vars``` – Contains variables defined for the role. These have quite a high precedence in Ansible.



## Command to create a role
```bash
ansible-galaxy init <rolename>
```

## Main.yaml to invoke the role
```bash
----
- hosts: all
  roles:
    - role: "<name of the role>"

Example:

- hosts: all 
  roles:
    - role: webserver
```

## passing variables with roles

```bash
- hosts: testdroplets
  roles:
    - update
    - bootstrap_server
    - { role: create_new_user, username: 'foobar' }
    - vimserver
```  
    
 ## setting facts for all the roles
 ```bash
- hosts: testdroplets
  pre_tasks:
    - set_fact:
        username: my_username
  roles:
    - update
    - bootstrap_server
    - create_new_user
    - vimserver
```
