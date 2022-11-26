# Modifying and Copying Files to Hosts

Most important module for file management

# fetch module
You can use the Fetch module go securely move files back and forth
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_file_deployment/secure_log_backups.yml


# copy module
You can use the copy module to copy files back and forth both internally and externally with configurations
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_file_deployment/copy_file.yml


# file module
You can use the file module to create files, directories etc
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_file_deployment/selinux_defaults_file.yml

# lineinfile module
It's used for modifying and adding lines in a file
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_file_deployment/add_line.yml


# blockinfile module
It's used for adding a block of text to an existing file
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_file_deployment/add_block.yml


# Jinja2 Templates
It's files that use variables to include static and dynamic values. Jinja templates are taken into use with the template module.
### Example