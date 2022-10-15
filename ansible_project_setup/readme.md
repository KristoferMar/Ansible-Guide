# Ansible Project Setup

## ansible.cfg
Create the ansible.cfg file within your ansible directory 

This file is used to customize and modify the behavior of ansible. Where you define paths to invetory files, users, ask_pass, escalated preveliges etc. 

<pre>
touch ansible.cfg
</pre>

## inventory
Defines a collection of hosts that Ansible will manage. Hosts can be assigned to groups, which can have sub-groups which can be managed collectively. The inventory can also set variables that apply to the hosts and groups that it defines.