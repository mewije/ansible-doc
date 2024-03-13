#Use Conditionals to Control Play Execution
Just like in programming languages, conditional statements are used when more than one outcome is possible. Let’s have a look at some of the commonly used conditional statements in Ansible playbooks.
#When statement
Sometimes, you may want to perform tasks on specific nodes and not others. The 'when' conditional statement is quite easy to use and implement in a playbook. When using the 'when' clause simply declare the condition adjacent to the clause as shown:
```
when: condition
```
When the condition is satisfied, then the task is performed on the remote system.

Let’s check out a few examples:
Example 1: Using When Operator
```
---
- hosts: all

  tasks:
  - name: Install Nginx on Debian
     apt: name=nginx state=present
     when: ansible_os_family == “Debian”
```
The play above installs Nginx webserver on hosts running the Debian family of distros.

You can also use the OR and AND operator alongside the when the conditional statement.

Example 2: Using AND Operator with When
```
---
- hosts: all

  tasks:
  - name: Install Nginx on Debian
     apt: name=nginx state=present
     when: ansible_os_family == “Debian” and
           ansible_distribution_version == “18.04”
```
When using the AND operator, both statements must be satisfied for the task to be executed.

The play above installs Nginx on Nodes running a Debian family of OS which is version 18.04. Obviously, this will be Ubuntu 18.04.

Example 3: Using OR Operator with When
With OR operator, the task is executed if either of the conditions is fulfilled.
```
---
- hosts: all

  tasks:
  - name: Install Nginx on Debian
     apt: name=nginx state=present
     when: ansible_os_family == “Debian” or
	      Ansible_os_family == “SUSE”
```
The play above installs Nginx webservers on either Debian or SUSE family of OS or both of them.

NOTE: Always ensure to use the double equality sign == when testing a condition.

#Conditionals in loops
Conditionals can also be used in a loop. Say for instance you have a list of multiple packages that need to be installed on remote nodes.

In the playbook below, we have an array called packages containing a list of packages that need to be installed. These tasks will be carried out one after the other if the required clause is set to True.
```
---
 - name: Install Software packages
   hosts: all
   vars:
	 packages:
    • name: nginx
      required: True
    • name: mysql
      required: True
    • name: apache
      required: False
   tasks:
    • name: Install “{{ item.name }}”on Debian
    apt:
    name: “{{ item.name }}”
    state: present
    When: item.required == True
    loop: “{{ packages }}”
```
#Lab 1:
There is a playbook named nginx.yaml under playbooks directory. It is starting nginx service on all hosts defined in  inventory file.
```
---
-  name: 'Execute a script on all web server nodes'
   hosts: all
   become: yes
   tasks:
     -  service: 'name=nginx state=started'
```
Use the when condition to run this task only on target_node2 host.
Solution:
```
---
-  name: 'Execute a script on all web server nodes'
   hosts: all
   become: yes
   tasks:
     -  service: 'name=nginx state=started'
        when: 'ansible_host=="node02"'
```

#Lab 02:
The playbook under playbooks/age.yaml , has a variable defined called age. The two tasks attempt to print if I am a child or an Adult.

```
---
- name: 'Am I an Adult or a Child?'
  hosts: localhost
  vars:
    age: 25
  tasks:
    - name: I am a Child
      command: 'echo "I am a Child"'

    - name: I am an Adult
      command: 'echo "I am an Adult"'
```
Use the when conditional to print if I am a child or an Adult based on whether my age is < 18 (Child) or >= 18 (Adult).

Solution:
```
---
- name: 'Am I an Adult or a Child?'
  hosts: localhost
  vars:
    age: 25
  tasks:
    - name: I am a Child
      command: 'echo "I am a Child"'
      when: 'age < 18'
    - name: I am an Adult
      command: 'echo "I am an Adult"'
      when: 'age >= 18'
```
#Lab 03:
Playbook /home/bob/playbooks/nameserver.yaml attempts to add an entry in /etc/resolv.conf file to add a new nameserver.

```
---
- name: 'Add name server entry if not already entered'
  hosts: localhost
  become: yes
  tasks:
    - shell: 'cat /etc/resolv.conf'

    - shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
```

The first task in the playbook is using the shell module to display the existing contents of /etc/resolv.conf file and the second one is adding a new line containing the name server details into the file. However, when this playbook is run multiple times, it keeps adding new entries of same line into the resolv.conf file.

To resolve this issue, update the playbook as per details mentioned below.

1) Add a register directive to store the output of the first task to a variable called command_output

2) Then add a conditional to the second task to check if the output already contains the name server (10.0.250.10). Use command_output.stdout.find(<IP>) == -1
Solution:
```
---
- name: 'Add name server entry if not already entered'
  hosts: localhost
  become: yes
  tasks:
    - shell: 'cat /etc/resolv.conf'
      register: command_output
    - shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
      when: 'command_output.stdout.find("10.0.250.10") == -1'
```
