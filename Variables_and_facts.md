# variables
A variable, just like in many programming languages, is essentially a key that represents a value.

What Constitutes a Valid Variable Name?
A variable name includes letters, numbers, underscores or a mix of either 2 or all of them. However, bear in mind that a variable name must always begin with a letter and should not contain spaces.

Let’s take a look a few examples of valid and unacceptable variable names:
Valid Variable Name Examples:
```
football
foot_ball
football20
foot_ball20
```
Non-valid Variable Name Examples:
```foot ball
20
foot-ball
```
Let’s discuss the variable types:
1. Playbook Variables
2. Special Variables
3. Inventory Variables
4. Ansible Facts

# 1. Playbook Variables
Playbook variables are quite easy and straightforward. To define a variable in a playbook, simply use the keyword vars before writing your variables with indentation.

To access the value of the variable, place it between the double curly braces enclosed with quotation marks.

Here’s a simple playbook example:
```
- hosts: all
  vars:
    greeting: Hello world!

  tasks:
  - name: Ansible Basic Variable Example
    debug:
      msg: "{{ greeting }}"
```
In the above playbook, the greeting variable is substituted by the value Hello world! when the playbook is run. The playbook simply prints the message Hello world! when executed.

Additionally, you can have a list or an array of variables as shown:

The playbook below shows a variable called continents. The variable holds 5 different values – continent names. Each of these values can easily be accessed using index 0 as the first variable.

The example of the playbook below retrieves and displays Asia (Index 1).
```
- hosts: all
  vars:
    continents:
      - Africa
      - Asia
      - South America
      - North America
      - Europe

  tasks:
  - name: Ansible List variable Example
    debug:
      msg: "{{ continents [1] }}"
```
The variable list can similarly be structured as shown:
```
vars:
    Continents: [Africa, Asia, South America, North America, Europe]
```
To list all the items on the list, use the **with_items** module. This will loop through all the values in the array.

```
- hosts: all
  vars:
    continents: [Africa, Asia, South America, North America, Europe]

  tasks:
  - name: Ansible array variables example
    debug:
      msg: "{{ item }}"
    with_items:
      - "{{ continents }}"
```
Another type of Ansible variable is the dictionary variable.
Dictionary variables are additionally supported in the playbook. To define the dictionary variable, simply ident the key-value pair just below the dictionary variable name.
```
hosts: switch_f01

vars:
   http_port: 8080
   default_gateway: 10.200.50.1
   vlans:
       id: 10
       port: 2
```

# Lab 1:

Inventory file:

```
localhost ansible_connection=local nameserver_ip=8.8.8.8 snmp_port=160-161
node01 ansible_host=node01 ansible_ssh_pass=caleston123
node02 ansible_host=node02 ansible_ssh_pass=caleston123
[web_nodes]
node01
node02

[all:vars]
app_list=['vim', 'sqlite', 'jq']
user_details={'username': 'admin', 'password': 'secret_pass', 'email': 'admin@example.com'}
```

 playbook.yaml printing some personal information of an employee. We would like to move the car_model, country_name and title values to the respective variables, and these variables should be defined at the play level.
```
 ---
 - hosts: localhost
   tasks:
     - command: 'echo "My car is BMW M3"'
     - command: 'echo "I live in the USA"'
     - command: 'echo "I work as a Systems Engineer"'
```
 Add three new variables named car_model, country_name and title under the play and move the values over there. Use these variables within the task to remove the hardcoded values.

# Solution:
 ```
 ---
- hosts: localhost
  vars:
    car_model: 'BMW M3'
    country_name: USA
    title: 'Systems Engineer'
  tasks:
    - command: 'echo "My car is {{ car_model }}"'
    - command: 'echo "I live in the {{ country_name }}"'
    - command: 'echo "I work as a {{ title }}"'
```
Run the playbook.
```
ansible-playbook -i inventory playbook.yaml
```
# Lab
The ```app_install.yaml``` playbook is responsible for installing a list of packages on a remote server(s). The list of packages to be installed is already added to the inventory file as a list variable called **app_list**.

```
---
- hosts: all
  become: yes
  tasks:
    - name: Install applications
      yum:
        name: "{{ item }}"
        state: present
      with_items:
        - vim
        - sqlite
        - jq
```
Right now the list of packages to be installed is hardcoded in the playbook. Update the ***app_install.yaml*** playbook to replace the hardcoded list of packages to use the values from the **app_list** variable defined in the inventory file. Once updated, please run the playbook once to make sure it works fine.

Solution:
```
---
- hosts: all
  become: yes
  tasks:
    - name: Install applications
      yum:
        name: "{{ item }}"
        state: present
      with_items:
        - "{{ app_list }}"
```
# lab
The **playbooks/user_setup.yaml** playbook is responsible for setting up a new user on a remote server(s). The user details like ***username, password,*** and ***email*** are already added to the **playbooks/inventory** file as a dictionary variable called ***user_details.***
```
user_details={'username': 'admin', 'password': 'secret_pass', 'email': 'admin@example.com'}
```
Right now the user details is hardcoded in the playbook. Update the **playbooks/user_setup.yaml** playbook to replace the hardcoded values to use the values from the ***user_details*** variable defined in the inventory file. Once updated, please run the playbook once to make sure it works fine.

Solution:
```
---
- hosts: all
  become: yes
  tasks:
    - name: Set up user
      user:
        name: "{{ user_details.username }}"
        password: "{{ user_details.password }}"
        comment: "{{ user_details.email }}"
        state: present
```
Run the playbook.
