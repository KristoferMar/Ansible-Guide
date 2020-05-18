<h1>Ansible-Guide</h1>
Ansible is automation language created for continuous integration and continues deployment. <br>

Ansible control machine can only be linux which means that the "controller machine" should be linux. <br>

<h2>Playbooks</h2>
- Is ansible's orcestration language. <br>
Playbooks are Ansible's configuration, deploymentm and orchestration language created as .yml files. 
- you can ex run series of commands on different servers or you can create complex playbooks where you configure and setup whole servers.
- All playbooks are created in YAML.
<h3>Task</h3>
- A single action on a host 
- Exeucte a command 
- run script 
- isntall a package 
- shutdown/restsrt.

<h3>How to run playbooks</h3>
Execute Ansible Playbook
<i>ansible-playbook playbook.yml</i>
Syntax
<i>ansible-playbook 'playbook file name'</i>

<b>Example not using playbooks (wrong way to do it)</b>
"ansible 'hosts' -a 'command'"
"ansible all -a '/sbin/reboot'"
"ansible 'hosts' -m 'module'"
"ansible target1 -m 'pint'"

<h2>Modules</h2>
- System
- Commands
- files 
- Database 
- Cloud 
- Windows 
- More..

<br>
<h2>Variables</h2>
Variables stores ifnormation that varies with each host. <br>
Example: <br>
fasdfasdf

<br>
<h2>Loops</h2>
Loops iterate multiple eg a list and gives us opertunity to execute logic foeach element in a eg a list. <br>
Example: <br>
fsdfsdfsdf 

We can use with directive to loop through items: <br>
- with_item <br>
- with_file <br>
- with_url <br>
- with_mongodb <br>

- Use variables in the following way: {{variable}}. <br> 
- Variables are stored in the inventory file. <br>
- If variable stands alone we use single quotes around: '{{inter_ip_rande}}' <br>
- If variable is part of a longer longer line we do not use quotes: something/{{inter_ip_range}}/something <br>

<br>
<h2>Conditionals</h2>
- An example of the use of a condition is if we want create a playbook which support two different servers. 

<br>
<h2>Roles</h2>
Roles are specified in the "roles_path" variable which is stored in the ansible project somewhere. <br><br>

Get list of roles <br>
<i>ansible-galaxy list</i><br>
<br>

<br>
<h2>Inventory file</h2>
Where we list variables and servers. You can also group servers with "[mail]" above all servers. You can have multiple servers listed in an inventory file.

ansible_host <br>
- Inventory parmeter to define a host.

ansible_connection <br>
- Defines how ansible connects to a server.

ansible_user 
- Defines the user who are doing to execute commands on the server. 

ansible_ssh_pass
- stores the password used, you should maybe find a way to not store this in pain text.


<h2>YAML<h2>
- Ansible playbooks are written and configured in YAML.
- We can store data in "Dictionaries", "Lists" or "List of Dictionaries". We also compare them to configre data in ansible playbooks. 