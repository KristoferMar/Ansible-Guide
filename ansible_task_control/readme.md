# Implementing Task Control

## Writing Loops and Conditional Tasks
<pre>
  tasks:
    - name: MariaDB packages are installed
      yum:
        name: "{{ item }}"
        state: present
      loop: "{{ mariadb_packages }}"
      when: ansible_distribution == "RedHat"</pre>
### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_task_control/loop_and_condition.yml

## Implementing Handlers
Think of handlers like this.. If the task has to be run then we run the assosiated handler aswell, if task does not have to run we skip the handlers.

- Handlers are defined on the same level as tasks in the playbook

### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_task_control/handlers.yml

## Handling Task Failures
We use the keywords block, rescue and always to handle a failing task
- the keyword "ignore_errors: yes" keeps the playbook runing even when a task fails
- the keyword "failed_when: web_package == 'httpd'" fails the task on a condition 

### Example
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_task_control/handling_task_failure.yml