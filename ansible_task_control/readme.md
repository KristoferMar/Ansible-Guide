## Implementing Task Control

### Writing Loops and Conditional Tasks
<pre>
  tasks:
    - name: MariaDB packages are installed
      yum:
        name: "{{ item }}"
        state: present
      loop: "{{ mariadb_packages }}"
      when: ansible_distribution == "RedHat"</pre>
Example