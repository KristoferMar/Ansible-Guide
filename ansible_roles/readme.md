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