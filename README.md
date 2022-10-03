# Introduction to Ansible

https://github.com/KristoferMar/Ansible-Guide/tree/master/installing-ansible

##  Installing Ansible

## Implementing an Ansible Playbook



<h1>Ansible-Guide</h1>
Ansible is automation language created for continuous integration and continues deployment. <br>

Ansible control machine can only be linux which means that the "controller machine" should be linux. <br>


<br>
<h2>Playbooks</h2>
- Is ansible's orcestration language. <br>
Playbooks are Ansible's configuration, deploymentm and orchestration language created as .yml files.  <br>
- you can ex run series of commands on different servers or you can create complex playbooks where you configure and setup whole servers.<br>
- All playbooks are created in YAML. <br>

<br>
<h3>Task</h3>
- A single action on a host <br>
- Exeucte a command <br>
- run script <br>
- isntall a package <br>
- shutdown/restsrt. <br>

<br>
<h3>How to run playbooks</h3>
It's possible to combine the executions together: <br><br>
Execute Ansible Playbook<br>
<i>ansible-playbook playbook.yml</i><br>
Execute Playbook with inventory <br>
<i>ansible-playbook -i LocatedInventoryFile.yml playbook.yml</i><br>

<br>
<b>Example not using playbooks (wrong way to do it)</b>
"ansible 'hosts' -a 'command'" <br>
"ansible all -a '/sbin/reboot'" <br>
"ansible 'hosts' -m 'module'" <br>
"ansible target1 -m 'pint'" <br>

<br>
<h2>Modules</h2>
We have the following modules which all have sub components. <br>
- System <br>
Commands<br>
<a href="https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/command.yml" target="_blank">https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/command.yml</a><br><br>
Template <br>
<a href="https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/template.yml" target="_blank">https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/template.yml</a><br><br>
<h3>Copy module</h3>
https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/Copy-Module/copy_module.yml <br>
<h3>File module</h3>
https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/File-Module/File-Module.yml <br>
- Database <br> 
- Cloud <br>
- Windows <br>
Fetch <br>
<a href="https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/fetch.yml" target="_blank">https://github.com/KristoferMar/Ansible-Guide/blob/master/Modules/fetch.yml</a><br>
- More.. <br>

<br>
<h2>Variables</h2>
Variables stores ifnormation that varies with each host. <br>
- Use variables in the following way: {{variable}}. <br> 
- Variables are stored in the inventory file. <br>
- If variable stands alone we use single quotes around: '{{inter_ip_rande}}' <br>
- If variable is part of a longer longer line we do not use quotes: something/{{inter_ip_range}}/something <br><br>


<br>
<h2>Loops</h2>
Loops iterate multiple eg a list and gives us opertunity to execute logic foeach element in a eg a list. <br>

- All memebers of a list are lines beginning at the same indentaion level starting with a "-" ( a dash and a space). <br>

We can use with directive to loop through items: <br>
- with_item <br>
- with_file <br>
- with_url <br>
- with_mongodb <br>

Example: <br>
<a href="https://github.com/KristoferMar/Ansible-Guide/blob/master/Loop.yml" target="_blank">https://github.com/KristoferMar/Ansible-Guide/blob/master/Loop.yml</a>


<br>
<h2>Conditionals</h2>
- An example of the use of a condition is if we want create a playbook which support two different servers. <br>

Example: <br>
<a href="https://github.com/KristoferMar/Ansible-Guide/blob/master/Condition.yml" target="_blank">https://github.com/KristoferMar/Ansible-Guide/blob/master/Condition.yml</a>


<br>
<h2>Roles</h2>
Roles are specified in the "roles_path" variable which is stored in the ansible project somewhere. <br><br>

Get list of roles <br>
<i>ansible-galaxy list</i><br>
<br>

<br>
<h2 class="subsubTitleSection">File and Directories</h2>

<h3>Directory manipulation</h3>
<a>link here</a>



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

<br>
<h2>Executions</h2>

<h3>Local executions</h3>
- "local_action" is an alternative way to doing delegate_to: localhost. <br>
Executes command localy <br>
<i>local_action: command ping -c 1 {{ inventory_hostname }}</i>

<h2>YAML</h2>
- Ansible playbooks are written and configured in YAML. <br>
- We can store data in "Dictionaries", "Lists" or "List of Dictionaries". <br> We also compare them to configre data in ansible playbooks. <br>

<h2>Experienced issues</h2>
<h3>Ansible job hangs indefinitely</h3>
Whenever an asible job hangs indefinitly then you should try deleating the ~./ansible folder from your home directory. It will be recreated when an ansible playbook is run, and hopefully fixes the issue. <br>
