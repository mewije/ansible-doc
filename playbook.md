# Ansible Plays and Playbooks
A playbook is a set of configuration management scripts that define how tasks are to be executed on remote hosts or a group of host machines. The scripts or instructions are written in YAML format.

# Modules
Modules are discrete units of code used in playbooks for executing commands on remote hosts or servers. Each module is followed by an argument.

The basic format of a module is key: value.

```
- name: Install apache packages
    yum:   name=httpd  state=present
```
In the above YAML code snippet, **-name** and **yum** are modules.

# Plays
An ansible play is a script or an instruction that defines the task to be carried out on a server. A collection of plays constitute a playbook. *In other words, a playbook is a collection of multiple plays*, each of which clearly stipulates the task to be carried out on a server. Plays exist in YAML format.

The objective of this subtopic is to give you an overview of various tasks that can be accomplished by Ansible modules.

# 1) Package Management in Linux
Package management is one of the most essential and frequent tasks carried by systems administrators. Ansible ships with modules that help you execute package management tasks both in RedHat and Debian based systems.

For instance, you can have a playbook file to install the Apache webserver on CentOS 7 and call it httpd.yml.

# Example 1: Installing the Apache Webserver on CentOS7

To create the playbook run the command, ```touch playbook_name.yml```

For example to create a playbook called httpd, run the command.
```
touch httpd.yml
```
 A YAML file begins with 3 hyphens as shown. Inside the file, add the following instructions.
 ```
 ---
- name: This installs and starts Apache webserver
  hosts: webservers
  tasks:
    - name: Install Apache Webserver
      yum:   
        name: httpd
        state: latest
```
The above playbook installs Apache web server on remote systems defined as webservers in the inventory file.

# Service Module
The service module allows system administrators to start, stop, update, upgrade and reload services on the system.

Example 1: Starting Apache Webserver
```
---
- name: Start service httpd, if not started
  service:
    name: httpd
    state: started
```
Example 2: Stopping Apache Webserver
```
---
- name: Stop service httpd
  service:
    name: httpd
    state: stopped
```
Example 3: Restarting a Network Interface enp2s0
```
---
- name: Restart network service for interface eth0
  service:
    name: network
    state: restarted
    args: enp2s0
```
# Copy Module
As the name suggests, copy module copies files from one location on the remote machine to a different location on the same machine.

Example 1: Copying Files from Local to Remote Linux
```
---
- name: Copy file with owner and permissions
  copy:
    src: /etc/files/abcd.conf
    dest: /opt/abcd.conf
    owner: ansible
    group: ansible
    mode: '0644'
```
Example 2: Copying Files from Local to Remote Linux
```
---
- name: Copy file with owner and permissions
  copy:
    src: /etc/files/abcd.conf
    dest: /opt/abcd.conf
    owner: ansible
    group: ansible
    mode: u=rw, g=r, o=r
```
The permissions in the previous examples can be represented as shown in the last line, The user is assigned read and write permissions, the group is assigned write permissions, and the rest of the world is assigned read permissions.

# File Module
The file module is used to carry many file operations including creating files & directories, assigning file permissions, and setting symlinks.

Example 1: Perform Linux File Permissions
```
---
- name: Change file ownership, group, and permissions
  file:
    path: /etc/abcd.conf
    owner: ansible
    group: ansible
    mode: '0644'
```
The above play creates a file called abcd.conf in the /etc directory setting permissions to 0644.

Example 2: Delete Linux File
```
---
- name: Remove file (delete file)
  file:
    path: /etc/abcd.conf
    state: absent
```
This removes or deletes the file abcd.conf.

Example 3: Create a Directory
```
---
- name: create a directory if it doesn’t exist
  file:
    path: /etc/mydirectory
    State: directory
```
This will create a directory in the /etc directory setting permissions to 0777.

# Lineinfile Module
The lineinfile module is helpful when you want to change a single line in a file. It can replace an existing line.

Example 1: Alter Files in Linux
```
---
- name: Add a line to a file if the file does not exist, without passing regexp
  lineinfile:
    path: /etc/hosts
    line: 192.168.1.50 abcd.com
    create: yes
```
This adds the entry 192.168.1.50 abcd.com to the /etc/hosts file.

Example 2: Manipulate Files in Linux
```
---
 - name: Ensure SELinux is set to enforcing mode
  lineinfile:
    path: /etc/selinux/config
    regexp: '^SELINUX='
    line: SELINUX=disabled
```
The play above sets SELINUX value to disabled.

# Archive Module
An Archive module is used for the creation of a compressed archive of a single or multiple files. It assumes the compression source exists is present on the target destination. After archival, the source file can later be deleted or removed using the statement **remove=True**.

Example 1: Create a Archive File

```
- name: Compress directory /path/to/db_dir/ into /path/to/db_backup.tgz
  archive:
    path: /path/to/db_dir
    dest: /path/to/db_backup.tgz
```
Example 2: Create a Archive File and Remove
```
- name: Compress regular file /path/to/db_dir into /path/to/db_backup.gz and remove it
  archive:
    path: /path/to/db_dir
    dest: /path/to/db_backup.tgz
    remove: yes
```
In the above play, the source file /path/to/db_dir is deleted after the archival is complete.

Example 3: Create a Archive File
```
- name: Create a bz2 archive of /path/to/tecmint
  archive:
    path: /path/to/tecmint
    format: bz2
```

# Command Module
One of the most commonly used modules, the command module takes the command name and later followed by a list of arguments. The command is a passed the same way that you’d type in a Linux shell.

Example 1: Run a Command
```
- name: Executing a command using the command module
  command: cat helloworld.txt
```

Example 2: Check Uptime of Remote Linux
```
---
 - name: Check the remote host uptime
    hosts: servers
    tasks:
      - name: Execute the Uptime command over Command module
        register: uptimeoutput
        command: "uptime"

- debug:
          var: uptimeoutput.stdout_lines
```
