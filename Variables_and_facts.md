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
# 2. Special Variables
Ansible provides a list of predefined variables that can be referenced in Jinja2 templates and playbooks but cannot be altered or defined by the user.

Collectively, the list of Ansible predefined variables is referred to as Ansible facts and these are gathered when a playbook is executed.

To get a list of all the Ansible variables, use the setup module in the Ansible ad-hoc command as shown below:

```ansible -m setup hostname
```
This displays the output in JSON format as shown:
From the output, we can see that some of the examples of Ansible special variables include:
```
ansible_architecture
ansible_bios_date
ansible_bios_version
ansible_date_time
ansible_machine
ansible_memefree_mb
ansible_os_family
ansible_selinux
```

There are many other Ansible special variables these are just a few examples.

These variables can be used in a Jinja2 template as shown:
```
<html>
<center>
   <h1> The hostname of this webserver is {{ ansible_hostname }}</h1>
   <h3> It is running on {{ ansible_os_family}}system </h3>
</center>
</html>
```
# 3. Inventory Variables
Lastly, on the list, we have Ansible inventory variables. An inventory is a file in INI format that contains all the hosts to be managed by Ansible.

In inventories, you can assign a variable to a host system and later use it in a playbook.
```
[web_servers]

web_server_1 ansible_user=centos http_port=80
web_server_2 ansible_user=ubuntu http_port=8080
```
The above can be represented in a playbook YAML file as shown:
```
---
   web_servers:
     web_server_1:
        ansible_user=centos
	   http_port=80

     web_server_2:
        ansible_user=ubuntu
	   http_port=8080
```
If the host systems share the same variables, you can define another group in the inventory file to make it less cumbersome and avoid unnecessary repetition.

For example:
```
[web_servers]

web_server_1 ansible_user=centos http_port=80
web_server_2 ansible_user=centos http_port=80
```
The above can be structured as:
```
[web_servers]
web_server_1
web_server_2


[web_servers:vars]
ansible_user=centos
http_port=80
```
And in the playbook YAML file, this will be defined as shown:

```
---
   web_servers:

     hosts:
       web_server_1:
	     web_server_2:

     vars:
        ansible_user=centos
        http_port=80
```

# 4. Ansible Facts
***TASK:  [Gathering facts] *********
When running playbooks, the first task that Ansible does is the execution of setup task. I’m pretty sure that you must have come across the output:

Ansible facts are nothing but system properties or pieces of information about remote nodes that you have connected to. This information includes the System architecture, the OS version, BIOS information, system time and date, system uptime, IP address, and hardware information to mention just a few.

To get the facts about any system simply use the setup module as shown in the command below:

```
ansible -m setup <hostname>
```
This prints out a large set of data in JSON format.

# Custom Facts
Did you also know that you can create your own custom facts that can be gathered by Ansible? Yes, you can. So how do you go about it? Let’s shift gears and see how.

The first step is to create a /etc/ansible/facts.d directory on the managed or remote node.

Inside this directory, create a file(s) with a .fact extension. This file(s) will return JSON data when the playbook is run on the Ansible control node, which is inclusive of the other facts that Ansible retrieves after a playbook run.

Here’s an example of a custom fact file called date_time.fact that retrieves date and time.

```
# mkdir -p /etc/ansible/facts.d
# vim /etc/ansible/facts.d/date_time.fact
```
Add the following lines in it.
```
#!/bin/bash
DATE=`date`
echo "{\"date\" : \"${DATE}\"}"
```
Save and exit the file.

Now assign the execute permissions:
```
# chmod +x /etc/ansible/facts.d/date_time.fact
```
Now, I created a playbook on Ansible control node called check_date.yml.
```
---

- hosts: webservers

  tasks:
   - name: Get custom facts
     debug:
      msg: The custom fact is {{ansible_local.date_time}}
```
Now run the playbook and observe Ansible retrieving information saved on the fact file:
```
# ansible_playbook check_date.yml

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
