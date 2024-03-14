## Pointers of Ansible 


## 1.To display the time takeby each task there is a setting availble which needs to configure at ansible.cfg 
```bash
// Ansible.cfg
[defaults]
inventory = hosts
callback_whitelist = profile_tasks
deprecation_warnings = false
```
## 2.To skip the fingerprint 
 ```bash
 Method1 :- Either mention it /etc/ansible/ansible.cfg
 host_key_checking = False
 ```
```bash
 Method2 :-Mention the same inventory
  [newserver:vars]
ansible_ssh_common_args='-o StrictHostKeyChecking=no'
```
```bash
Method3:- Save it as the enviroenment variable
export ANSIBLE_HOST_KEY_CHECKING=False
```

## 3.If we need hosts in both the inventory groups.
```bash
if you want to have hosts in multiple groups in ansible 
 name: This sets up an httpd webserver
  hosts: web:db
```

## Pointers and Good Practices of Ansible.
- Use roles for code reuse: Organize your playbooks into reusable roles.
This encourages code reuse and makes your automation code more modular and maintainable
- Always use descriptive names for your ```roles, tasks, and variables```
- Never place secrets and sensitive data in your roles YAML files.Instead use Ansible vault.
- Parameterize your playbooks:
- Use tags: Tags allow you to selectively run specific tasks within your playbooks.
- Parallelize tasks: Use the --forks option to parallelize tasks across multiple hosts
- Enable logging for ansible




