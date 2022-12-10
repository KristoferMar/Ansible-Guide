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

https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/users_and_authentication.yml


## Managing the Boot Process and Scheduled Processes
Modules
- cron
- at
- reboot

Create crontab entry
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/boot_process_and_scheduled_processes/create_crontab_file.yml

Remove crontab entry
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/boot_process_and_scheduled_processes/remove_cron_job.yml

Create scheduled job
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/boot_process_and_scheduled_processes/schedule_at_task.yml

Set boot target
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/boot_process_and_scheduled_processes/set_default_boot_target_graphical.yml

Reboot target host
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/boot_process_and_scheduled_processes/reboot_hosts.yml


## Managing Storage
Modules
- parted
- lvg
- lvol
- filesystem
- mount

Storage playbook
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/manage_storage/storage.yml

Related variables
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_sys_admin_tasks/manage_storage/storage_vars.yml
