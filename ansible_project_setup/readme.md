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

## Vault
A vault is used to encrypt and decrypt any structured data file used by Ansible. This way you can safely store passwords and other kinds of sensable data and use them in your playbooks. 

Create an encrypted file
<pre>
[user@demo ~]$ ansible-vault create secret.yml
New Vault password: redhat
Confirm New Vault password: redhat

or

[user@demo ~]$ ansible-vault create --vault-password-file=vault-pass secret.yml
</pre>

Editing an existing encrypted file
This command decrypts the file to a temporary file and allows you t oedit it. When saved, it copies the content and removes the temporary file.
<pre>
[user@demo ~]$ ansible-vault edit secret.yml
Vault password: redhat
</pre>

When running playbook with specified vault

<pre>
[user@demo ~]$ ansible-playbook --vault-id @prompt site.yml
Vault password (default): redhat
</pre>

## First playbook
Playbooks are used to run multiple complex tasks against a set of hosts in an easily repeatable maner.

Playbook example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_project_setup/playbook.yml