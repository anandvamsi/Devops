# YAML
- YAML (YAML Ain't Markup Language) is a human-readable data serialization format. 
- It's commonly used for configuration files, data exchange between programming languages,and in various other applications
where data needs to be easily readable by humans as well as machines.

## Example of YAML
```bash
# Example YAML document
name: John Doe
age: 30
job: Developer
skills:
  - Python
  - JavaScript
  - SQL
```
Notes : In this example Name,age,job and skills represent the key value pair.
skills is a list containing multiple items, each indicated by a hyphen followed by a space.

## YAML over JSON,XAMl
YAML is often preferred for its readability compared to other data serialization formats like JSON or XML, especially when dealing with complex data structures. 
It's commonly used in configuration files for applications and services, including tools like Kubernetes, Docker, and Ansible.


## Few Pointers of YAML.
- YAML should always start with ```---```
- ```comments``` in YAML starts with #
- YAML uses key-value pairs, where keys and values are separated by a colon (:)
- To represent a string we use ```"``` or single quotes ```'```
- Boolean values are represented as ``true`` or ``false``
- To represent a multi-line string using either the | or > characters
```bash
multi_line_string: |
  This is a multi-line
  string in YAML.
  It preserves newlines
  and keeps the formatting.
```
```bash
multi_line_string: >
  This is a multi-line
  string in YAML.
  It folds newlines into
  spaces and removes
  leading/trailing whitespace.
```


## Understading the Ansible tasks.
```
---
- name: Create a directory
  file:
    path: /testserver
    state: directory
```
Here the file is the module , path and state is the child attributes of the module ```file```




