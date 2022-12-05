# Reusing Content with System Roles

# Ansible Galaxy roles
Ansible Galaxy is essentially a large public repository of Ansible roles.

The ansible-galaxy command searches three directories for roles, as indicated by the "roles_path" entry in the ansible.cfg file:
- ./roles
- /usr/share/ansible/roles
- /etc/ansible/roles


# Usage of roles

# Creating roles
Roles let you automatically load related vars, files, tasks, handlers, and other Ansible artifacts based on a known file structure. After you group your content in roles, you can easily reuse them and share them with other users.

You can create a role this way
<pre>
[kristofer@workstation roles]$ ansible-galaxy init myvhost
- myvhost was created successfully
</pre>


# Deploying Roles with Ansible Galaxy
Skeletons can be stored and installable via ansible-galaxy.

Can be installed the following way

<pre>
[kristofer@workstation role-galaxy]$ ansible-galaxy install -r \
> roles/requirements.yml -p roles
- extracting student.bash_env to /home/student/role-galaxy/roles/student.bash_env
- student.bash_env (master) was installed successfully
</pre>


# Getting Roles and Modules from Content Collections
A collection downloaded from ansible-galxy will land in the following collection
<pre>
~/.ansible/collections/ansible_collections
</pre>

Install a collection from a URL using ansible-galxy
<pre>
[kristofer@workstation ~]$ ansible-galaxy collection install http://materials.example.com/labs/role-collections/gls-utils-0.0.1.tar.gz
</pre>

Alternativly you can install and download collections from a configured yaml file as this one:

https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_roles/collections.yml

Afterwards you can install all of it the following way
<pre>
[kristofer@workstation role-collections]$ ansible-galaxy collection install \
> -r requirements.yml
</pre>