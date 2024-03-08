```bash
---
- name: Example Ansible playbook for Handlers
  hosts: localhost
  become: yes
  remote_user: anand

  vars:
    # 1. String Variable
    main_playbook_variable: "foo"

    # 2. Boolean Variable
    is_enabled: false

    # 3. List Variable
    fruits:
      - apple
      - banana
      - orange

    # 4. Dictionary Variable
    fruit_prices:
      apple: 0.5
      banana: 0.25
      orange: 0.75


  # 6. Variable from the my-vars.yml
  vars_files:
    - my-vars.yml

  tasks:
    - name: String Variable from - main_playbook_variable
      debug:
        var: main_playbook_variable

    - name: Boolean variable
      debug:
        msg: "Variable is true"
      when: is_enabled

    - name: List variable - Print list of fruits
      debug:
        var: fruits

    - name: List variable - Reference individual item in list
      debug:
        var: fruits[0]

    - name: Dictionary Variable - Accessing all dictionary variable
      debug:
        var: fruit_prices

    - name: Dictionary Variable - Accessing individual specific fields
      debug:
        var: fruit_prices.apple

    - name: Get the price of an apple
      command: echo "{{ fruit_prices['apple'] }}"
      #Register the price of a apple to new variable - apple_price
      register: apple_price_as_registered_var

    - name: Print the value of register variable
      debug:
        var: apple_price_as_registered_var.stdout

    # Accessing variable inside playbook from my-vars.yml
    - name: Get the value of variable from my-vars.yml
      debug:
        var: vars_from_my_vars_yml
  ```
