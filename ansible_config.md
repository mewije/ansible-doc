## Ansible Configuration File Precedence Order

Ansible uses a specific precedence order to determine which configuration file (ansible.cfg) to use for its operations. Here is the order in which Ansible searches for the configuration file:

# Current Directory:
Ansible first looks for the configuration file in the current directory where the command is executed (./ansible.cfg).
# Home Directory:
 If not found in the current directory, Ansible then searches for the configuration file in the home directory of the user executing the command (~/.ansible.cfg).
# Environment Variable:
Ansible will use the configuration file specified by the ANSIBLE_CONFIG environment variable if it is defined.
# System-wide Configuration:
Lastly, if none of the above are found, Ansible will fall back to the system-wide configuration file located at /etc/ansible/ansible.cfg.

To list all Configurations
```
ansible-config list
```
To show current Configurations
```
ansible-config view
```
To show the current settings. Especially when checking the Configuration settings.
```
ansible-config dump
```
