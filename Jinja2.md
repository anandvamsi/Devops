# Jinja2 Templates
---------------------
Jinja2 is a powerful templating engine used in Ansible for creating dynamic content within playbooks, 
templates, and other Ansible files. Here are some examples of using Jinja2 in Ansible

Some of the important features of Jinja2 are:
• It is fast and compiled just in time with the Python byte code
• It has an optional sandboxed environment
• It is easy to debug
• It supports template inheritance

## some of the Accepeted pattern in jinja.

{{ }} embeds variables inside a template and prints its value in the 
resulting file. This is the most common use of a template.
For example:
 {{ nginx_port }}

• {%    %} embeds statements of code inside a template, for example, for a loop, it embeds the if-else statements, 
which are evaluated at runtime but are not printed.
