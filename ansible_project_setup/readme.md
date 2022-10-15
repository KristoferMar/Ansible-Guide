# Ansible Project Setup

## ansible.cfg
Create the ansible.cfg file within your ansible directory 

This file is used to customize and modify the behavior of ansible. Where you define paths to invetory files, users, ask_pass, escalated preveliges etc. 

<pre>
touch ansible.cfg
</pre>
example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_project_setup/ansible.cfg

## inventory
Defines a collection of hosts that Ansible will manage. Hosts can be assigned to groups, which can have sub-groups which can be managed collectively. The inventory can also set variables that apply to the hosts and groups that it defines.

<pre>
touch inventory
</pre>

Inventory example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_project_setup/inventory 

Check and verify all hosts
<pre>
[user@workstation ~]$ ansible all --list-hosts
</pre>


## First playbook
Playbooks are used to run multiple complex tasks against a set of hosts in an easily repeatable maner.

Playbook example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_project_setup/playbook.yml