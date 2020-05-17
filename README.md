<h1>Ansible-Guide</h1>
Ansible is automation language created for continuous integration and continues deployment. <br>

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