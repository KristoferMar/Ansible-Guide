# Playbook implementation

## Implementing an ansible playbook
The following hosts file holds a list of known hosts
<pre>
sudo vim /etc/ansible/hosts
</pre>

Following comands gives us an overview of avaiable hosts 

All managed hosts
<pre>
ansible all --list-hosts
</pre>

all ungrouped hosts
<pre>
ansible ungrouped --list-hosts
</pre>

all hosts from the webservers group
<pre>
ansible webservers --list-hosts
</pre>


## Managing Ansible Configuration file
the ansible.cfg file located in each ansible directory can dictate stuff related to the project.

example of such a file

<br>

## Running Ad Hoc Commands
Ad hoc is usefull for getting a feeling about the system you are working with

<b>REMEMBER to make bash scritps executable with "chmod -x adhoc.sh"</b>

examples
https://github.com/KristoferMar/Ansible-Guide/blob/master/playbook-implementation/adhock/adhoc.md

How to create repository file
https://github.com/KristoferMar/Ansible-Guide/blob/master/playbook-implementation/adhock/create_repository_file 

How to create user and ssh keys
https://github.com/KristoferMar/Ansible-Guide/blob/master/playbook-implementation/adhock/create_use_and_ssh_key

<br>

## Writing and running ansible playbooks
This topic is pretty self explainatory... Just learn it

example




## Utilize the Ansible docs for all modules
There are examples for almost all modules - Just do the following
<pre>
ansible-doc service

/EXAMPES (press enter)

... The example appear ...
</pre> 


## Ping all hosts available
<pre>
ansible all -m ping
</pre>


## The Ansible inventory
It's a file that defines hosts and groups of hosts upon which commands, modules, and tasks in a playbook opreate. 

The variable "ANSIBLE_CONFIG" variable controls location of the cfg file, you can change it with.
<pre>
export ANSIBLE_CONFIG=/ANSIBLE/ansible.cfg
</pre>

## Ansible Configuration
You can list the precise current configuration in the following way
<pre>
ansible-config  dump
</pre>

## Running ad Hoc Commands
Example
<pre>
ansible localhost -m command -a 'id'
</pre>

## Check syntax of playbook
<pre>
ansible-playbook about.yml --syntax-check
</pre>
