# Automating Linux Administration Tasks

## Managing Software and Subscriptions
Modules
- yum_repository
- rpm_key

Example of implementation to do verification
<pre>
    - name: Ensure Example Repo exists
      yum_repository:
        name: example-internal
        description: Example Inc. Internal YUM repo
        file: example
        baseurl: http://materials.example.com/yum/repository/
        gpgcheck: yes

    - name: Ensure Repo RPM Key is Installed
      rpm_key:
        key: http://materials.example.com/yum/repository/RPM-GPG-KEY-example
        state: present
</pre>

https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/repository_ensurance.yml


## Managing Users and Authentication
Modules
- group
- user
- authorized_key

