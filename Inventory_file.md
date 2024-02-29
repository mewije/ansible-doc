# What is Ansible Inventory File?
Red Hat Ansible Automation Platform works against a list of managed nodes or hosts in your infrastructure that are logically organized, using an inventory file. By using an inventory file, Ansible can manage a large number of hosts with a single command. Inventories also help you use Ansible more efficiently by reducing the number of command line options you have to specify.

The inventory file can be in one of many formats, depending on the inventory plugins that you have. The most common formats are INI and YAML. Inventory files listed in this document are shown in INI format.

# Default location of inventory file
```
$ /etc/ansible/hosts
```
Ansible inventory file allows system administrators to keep track of their managed remote systems. The default inventory file is known as the hosts file and is located in the /etc/ansible directory. This is where all the managed remote nodes are specified.

```
sudo nano /etc/ansible/hosts
```
```
# This is the default ansible 'hosts' file.
#
# It should live in /etc/ansible/hosts
#
#   - Comments begin with the '#' character
#   - Blank lines are ignored
#   - Groups of hosts are delimited by [header] elements
#   - You can enter hostnames or ip addresses
#   - A hostname/ip can be a member of multiple groups

# Ex 1: Ungrouped hosts, specify before any group headers:

## green.example.com
## blue.example.com
## 192.168.100.1
## 192.168.100.10

# Ex 2: A collection of hosts belonging to the 'webservers' group:

## [webservers]
## alpha.example.org
## beta.example.org
## 192.168.1.100
## 192.168.1.110
```
By default, all entries are commented out and no hosts are specified. In the next step, you will connect to the remote hosts and create a custom inventory file.

## Create Custom Inventory File
Ansible uses the default inventory file that is located at /etc/ansible/hosts to reference managed nodes, unless you specify a custom inventory file through the ```-i``` option.

The default inventory file works just fine. In fact, you can put your remote nodes in it using their IP addresses, as shown below:

```
192.168.1.51
192.168.1.52
```
You can then execute a ping module on the remote nodes without explicitly specifying the remote hosts:
```
sudo ansible -m ping all
```
Sure, this works without any glitches, but when faced with multiple managed nodes under different projects, the best practice is to create separate inventory files for each project. This way you will facilitate resource tracking and collaborate with other sysadmins more easily without having everyone tangled up in a single hosts file.

Now, let’s create a custom inventory file. To do this, you should first create a project directory and navigate into it:

```
mkdir project_dir && cd project_dir
```

Next, create a simple text file. You can give it any arbitrary name:

```
nano inventory
```
Then list your managed nodes per line using their IP addresses:
```
192.168.1.51
192.168.1.52
```
Save the changes and exit the file. Now you can verify your managed hosts using the ansible-inventory command. Note that you must reference the full path to the inventory file when using the -i option.

```
ansible-inventory -i project_dir/inventory --list
```
From here, you can execute a playbook or a ping module on your managed hosts using the custom inventory file:

```
sudo ansible -i project_dir/inventory -m ping all
```
