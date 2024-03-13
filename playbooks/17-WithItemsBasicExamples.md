# Loops and with_items

In Ansible, both loops and with_items serve the same purpose of iterating over a list of items and executing tasks for each item. 
However, they are implemented differently and have different levels of support depending on the Ansible version.


## Examples of with items and loops
```bash
- name: Execute a command with different arguments
  command: "echo {{ item }}"
  with_items:
    - "Hello"
    - "World"

- name: Copy multiple files
  copy:
    src: "{{ item.src }}"
    dest: "{{ item.dest }}"
  with_items:
    - { src: '/path/to/file1.txt', dest: '/tmp/file1.txt' }
    - { src: '/path/to/file2.txt', dest: '/tmp/file2.txt' }    

- name: Start multiple services
  service:
    name: "{{ item }}"
    state: started
  with_items:
    - apache2
    - mysql
    - nginx    

- name: Create multiple directories
  file:
    path: "{{ item }}"
    state: directory
  with_items:
    - /var/www/html
    - /etc/nginx/sites-available
    - /etc/nginx/sites-enabled
```
