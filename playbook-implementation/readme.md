# Playbook implementation

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


## Ansible inventory
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
