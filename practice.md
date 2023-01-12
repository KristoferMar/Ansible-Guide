# Ansible Installation and Configuration

## Question 1
Creating inventory files is the most critical aspect of an ansible environment, and this exam question is imminent.

- Install ansible in controller node.
- Group creation for your inventory file should be as shown below:

<pre>
node1 should be the member of proxy group
node2 and node3 should be the member of webservers group.
node4 should be the member of database group.
Create a new group called “prod” and put your webservers group in that new group.
</pre>

### Answer
To install Ansible, you can follow this link seimaxim.
You can create an inventory file like shown below:
<pre>
[proxy] 
node1 

[webservers] 
node2 
node3 

[database] 
node4 

[prod:children] 
webservers
</pre>


<br>


# AD-HOC Commands
## Question:1

- Create repository file for MariaDB server using the ad-hoc command with the following commands:
- name: MariaDB
- description: Repository MariaDB-Server
- base URL: http://yum.mariadb.org/10.5/centos8-amd64
- GPGKEY: https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
- GPGCHECK: yes
- enabled: active

*You can even create a playbook instead of a shell script.

"-b" stands for "become" and is used to become root for instance

### Answer

<pre>
ansible database -m yum_repository -a "name=mariadb description='Repository MariaDB-Server' baseurl='http://yum.mariadb.org/10.5/centos8-amd64' gpgkey='https://yum.mariadb.org/RPM-GPG-KEY-MariaDB' gpgcheck=yes enabled=yes" -b 
</pre>
You can create a file repo.sh and put the above-mentioned query into that file, make it executable, and run it.


## Question:2

This question prepares your environment so your ansible user can perform the tasks.

- Generate the script which creates the user named “ali” and copies the SSH public key of user “ansible”.
- User ali, which you have created, should be able to be a SUDO.

[Note] Your environment of 4 nodes and one controller already has root access to all the nodes. so manually, you are generating your ssh-key of user ali on the controller node, which you will copy on all the nodes. 

### Answer
Create a file “vim adhoc.sh”.
<pre>
#!/bin/bash

/usr/local/bin/ansible all -b -m user -a "name=ali state=present"
/usr/local/bin/ansible all -b -m file -a "path=/home/ali/.ssh state=directory owner=ali"
/usr/local/bin/ansible all -b -m copy -a "src=/home/ali/.ssh/id_rsa.pub dest=/home/ali/.ssh/authorized_keys directory_mode=yes"
/usr/local/bin/ansible all -b -m lineinfile -a "path=/etc/sudoers state=present line='ali ALL=(ALL) NOPASSWD: ALL'"
</pre>

once you exit from adhoc.sh file, make it executable with ” chmod +x adhoc.sh”.


<br>


# File content - variables & Facts

Question: 1
- Create a playbook, which can do as shown below:
- Create a file on all the servers at this path /etc/motd.
- The file contains “MY SERVER”.

<pre>
[kris@control ansible]$ vi motd.yml
---
- hosts: all
  become: yes
  tasks:
  - name: Write content in the file
    copy:
      dest: /etc/motd
      content: "MY SERVER"
</pre>


<br>


Question: 2
- Create a playbook, which can do as shown below:
- Create a file on all the servers at this path /etc/motd.
- The file contains  “MY SERVER: 'Server Name' “.

you replace the server Name with the actual HOSTNAME of the server.

<pre>
[kris@control ansible]$ vi motd.q2.yml
---
- hosts: webservers
  become: yes
  tasks:
  - name: Write content in the file
    copy:
      dest: /etc/motd
      content: "MY SERVER: {{ ansible_facts['hostname']}}"
</pre>


<br>


Question: 3
Create a playbook, which can do as shown below:

- The existing file (/etc/motd) should be removed ( if any).
- host group proxy should have the line “This Is PROXY Server”.
- Host group webservers should have the line “These Are The WEBSERVERS”.
- Host group database should have the line “This is “DATABASE server

Answer

<pre>
[kris@control ansible]$ cat motd.q3.yml
---
- hosts: all
  become: yes
  tasks:
  - name: Generate the file motd content
    file:
      path: /etc/mtd
      state: absent
  - name: Proxy Server file content
    copy:
      dest: /etc/motd
      content: "This Is PROXY Server"
    when: "'proxy' in group_names"
  - name: WEBSERVERS file content
    copy:
      dest: /etc/motd
      content: "These Are The WEBSERVERS"
    when: "'webservers' in group_names"
  - name: Database Server file content
    copy:
      dest: /etc/motd
      content: "This is DATABASE Server"
    when: "'database' in group_names"
</pre>


<br>


Question: 4

- In this question, you are going to practice the configuration of the SSH Server.
- Create a playbook, which can do the following tasks:

<pre>
Set banner to /etc/motd
X11Forwarding is disabled
MaxAuthTries is set to 3
</pre>

Answer

<pre>
---

- hosts: all
  become: yes
  tasks:
  - name: Set Banner to /etc/motd
    lineinfile:
      path: /etc/ssh/sshd_config
      regex: "^Banner"
      line: "Banner /etc/motd"

  - name: Disable X11Forwarding
    lineinfile:
      path: /etc/ssh/sshd_config
      regex: "^X11Forwarding"
      line: "X11Forwarding no"

  - name: Set MaxAuthTries to 3
    lineinfile:
      path: /etc/ssh/sshd_config
      regex: "^MaxAuthTries"
      line: "MaxAuthTries 3"
</pre>


<br>


Question: 5 

- Create a playbook such as servers.yml, which should generate the network file as shown below:

- Servers.yml must use a jinja2 file called servers.j2, and when servers.yml is executed, it creates the file servers.txt on the servers group of webservers and database servers.
    for example, your file should contain  information as shown below:
<pre>
'FQDN' 'HOSTNAME' 'IPADDRESS'
node1.seimaxim.com node1 192.168.1.12
</pre>


Answer


<pre>
 cat servers.yml
---
- hosts: all
  become: yes
  tasks:
  - name: Generating a file by the jinja2 file
    template:
      src: servers.j2
      dest: /root/servers.txt

</pre>

<pre>
[kris@control ansible]$ vi servers.j2
{% for host in groups['webservers']%}
{{ hostvars[host]['ansible_facts']['fqdn']}} {{ hostvars[host]['ansible_facts']['hostname']}} {{ hostvars[host]['ansible_facts']['default_ipv4']['address']}}
{% endfor %}
</pre>

<pre>
To check if the files has been generated. 
[kris@control ansible]$ ansible all -m shell -a "cat /root/servers*" -b
node3 | CHANGED | 
node2.seimaxim.com node2 10.0.2.15
node3.seimaxim.com node3 10.0.2.15

node4 | CHANGED | 
node2.seimaxim.com node2 10.0.2.15
node3.seimaxim.com node3 10.0.2.15

node1 | CHANGED | 
node2.seimaxim.com node2 10.0.2.15
node3.seimaxim.com node3 10.0.2.15

node2 | CHANGED | 
node2.seimaxim.com node2 10.0.2.15
node3.seimaxim.com node3 10.0.2.15
</pre>


<br>


Question: 6

- Create a playbook, which generates a file named “details.txt” file on each server   with the following information:

- The file should be placed in the root directory: /root

<pre>
HOSTNAME: 'hostname'
MEMORY_TOTAL: 'Total actual memory should be presented here'
MEMORY_FREE: 'Free Memory available on the system'
IPV4: 'IP address of the system'
FQDN: 'fqdn'
SDA_DISK_SIZE: '/dev/sda1 disk size'
SDC_DISK_SIZE: '/dev/sdc' #if disk is not attach,it should put NONE
</pre>

Answer

<pre>
---
- hosts: all
  become: yes
  tasks:
  - name: generate a report
    blockinfile:
      path: /root/report.txt
      create: yes
      block: |
        MEMORY_TOTAL: {{ ansible_facts['memtotal_mb' ]}}
        MEMORY_FREE: {{ ansible_facts['memfree_mb' ]}}
        HOSTNAME: {{ ansible_facts['hostname']}}
        IPV4: {{ ansible_facts['default_ipv4']['address']}}
        FQDN: {{ ansible_facts['fqdn'] }}
        SDA_DISK_SIZE: {{ ansible_facts['devices']['sda']['partitions']['sda1']['size'] }}
        SDB_DISK_SIZE: {{ ansible_devices.sdc.size | default('NONE') }}
</pre>

check the work

<pre>
#To check the report's content run shown below command
[ali@control ansible]$ ansible all -m shell -a "cat /root/report.txt" -b
</pre>


<br>


Question: 7

- Create a playbook that checks the file’s existence.
  print the message “File Exists” if the file exists and print the message “File does not exist”if the file does not exist.


Answer

<pre>
---
- hosts: all
  become: yes
  tasks:
  - name: Check if the file exists
    stat:
      path: /var/www/html/index.html # or you can put the path of your desired file
    register: result
  - debug:
      msg: "THE FILE EXISTS"
    when: result.stat.exists == true
  - debug:
      msg: "THE FILE DOES'T EXIST"
    when: result.stat.exists == false
</pre>


<br>


Question: 8

- Create a playbook that performs the shown below actions:
- Change the line “Listen 80” in the /etc/httpd/conf/httpd.conf to “Listen 8080”.
- Allow the port 8080 in firewall and restart the httpd services

Answer

<pre>
# you must have installed httpd or you can put any other file to practice this senario
---
- hosts: webservers
  become: yes
  tasks:
  - name: Modify the content of httpd.conf
    replace:
      path: /etc/httpd/conf/httpd.conf
      regexp: 'Listen 80'
      replace: 'Listen 8080'
    notify: restart_httpd
  handlers:
  - name: restart_httpd # must be same as notify
    service: 
      name: httpd
      state: restarted
</pre>


<br>


Question: 9

- Create a playbook that performs the shown below actions:
- Change the default target to the multi-user.target with the file module by using links. don’t use shell command to set-default target.

Answer

<pre>
---
- hosts: database
  become: yes
  tasks:
  - name: Change the default target
    file:
      src: /usr/lib/systemd/system/multi-user.target
      dest: /etc/systemd/system/default.target
      state: link
</pre>


<br>


Question: 10

- Create a playbook that performs the shown below actions:
- Run this playbook against the webservers group.
- A custom Ansible fact server_role=webservers is created that can be retrieved from ansible_local.custom.sample_exam when using Ansible setup module.

Answer

<pre>
---
- hosts: webservers
  become: yes
  tasks:
  - name: Ensure the directory exists
    file:
      path: "/etc/ansible/facts.d/"
      state: directory
      recurse: yes
  - name: Copy content to file in new directory
    copy:
      content: "[sample_exam]nserver_role=webserversn"
      dest: "/etc/ansible/facts.d/custom.fact"
</pre>

<br>

# Packages Installation

Question: 1

- Create a yml file, which performs the below tasks on webservers(httpd).
- Install the latest firewall and httpd.
- Make sure that your httpd service is allowed through firewalls.
- Create a file /var/www/html/index.html and put the entry “Server name is: hostname” so the hostname in the index.html should be replaced with the actual hostname from the system.

For example, when we do curl http://node2 it should give me “Server name is: node2”.

Answer

<pre>
---
- hosts: webservers
  become: yes
  tasks:
  - name: Install packages on webservers
    yum:
      name:
        - httpd
        - firewalld
      state: latest
  - name: Start firewalld and httpd on webservers
    service:
      name: "{{ item }}"
      state: started
      enabled: yes
    loop:
      - httpd
      - firewalld
  - name: Opening Port through FW
    firewalld:
      service: http
      permanent: yes
      immediate: yes
      state: enabled
  - name: Creation of index.html
    copy:
      dest: /var/www/html/index.html
      content: "Server name is: {{ ansible_facts['hostname'] }}"
</pre>


Question: 2

- Create a playbook that performs the shown below activities:
- The playbook will create a directory /webdir with the permission of 2755, and the owner is also webdir user.
- You should have already configured apache to run your page from /var/www/html/index.html.
- Create a link of your index.html in the /webdir, which should be accessible with the http://node1/webdir/index.html.
- Your /var/www/webdir/index.hml should contain “I AM FROM WEBDIR”.

Answer

<pre>
---
- hosts: node2
  become: yes
  tasks:
  - name: Create the webdir user
    user:
      name: webdir
      state: present
  - name: Create a directory
    file:
      mode: '2755'
      path: /webdir
      state: directory
  - name: Create Symbolic Link
    file:
      src: /webdir
      dest: /var/www/html/webdir
      state: link
  - name: Create index.html
    copy:
      dest: /var/www/html/webdir/index.html
      content: "I AM FROM WEBDIR"
</pre>


# Users & Groups

Question: 1

- Create a playbook that performs the shown below tasks:
- Create a group on all the servers named “admin”.
- Create a group on all the servers named “members”.

Here is the file user_list.yml, In the exam, it will be provided.

<pre>
---
users:
  - username: eli
    uid: 1201
    password: abcdeXYZ
    group: admin
  - username: vincent
    uid: 1202
    password: cdefgXYZ
    group: admin
  - username: sandy
    uid: 2201
    password: edfegZBC
    group: members
  - username: patrick
    uid: 2202
    password: gjfezBCG
    group: members
</pre>


- Users whose user ID starts with 1 should be created on servers in the webservers host group. The user password, shell, and UID should be used from the user_list.yml variable file.

- Users whose user ID starts with 2 should be created on servers in the database host group. The user password, shell, and UID should be used from the user_list.yml variable.

- Users uid starting from 1* should be the member of supplementary group admin.

- Users uid starting from 2* should be the member of supplementary group members.

- The shell should be set to /bin/bash for all users.

- Account passwords should use the SHA512 hash format.

- The SSH Key of your main ansible user should be uploaded. Dont generate the new SSH key just use your primary ansible user key, which you are using to connect from the controller node.

Answer
<pre>
---
- hosts: all
  become: yes
  vars_files:
    - user_list.yml
  tasks:
  - name: Create the group named admin
    group:
      name: admin
      state: present
  - name: Create the group named admin
    group:
      name: members
      state: present
  - name: Create users on webservers whose user ID starts with 1
    user:
      name: "{{ item.username }}"
      shell: /bin/bash
      groups: "{{ item.group }}"
      append: yes
      uid: "{{ item.uid }}"
      password: "{{ item.password | password_hash('sha512') }}"
    with_items: "{{ users }}"
    when:
      - "'webservers' in group_names"
      - "item.uid|string|first == '1'"
  - name: Create users on database whose user ID starts with 2
    user:
      name: "{{ item.username }}"
      shell: /bin/bash
      groups: "{{ item.group }}"
      append: yes
      uid: "{{ item.uid }}"
      password: "{{ item.password | password_hash('sha512') }}"
    with_items: "{{ users }}"
    when:
      - "'database' in group_names"
      - "item.uid|string|first == '2'"      
  - name: Create SSH directory for users on webservers hosts
    file:
      path: "/home/{{ item.username }}/.ssh/"
      state: directory
      owner: "{{ item.username }}"
      group: "{{ item.group}}"
      mode: "0700"
    with_items: "{{ users }}"
    when:
      - "'webservers' in group_names"
      - "item.uid|string|first == '1'"
  - name: Create SSH directory for users on database hosts
    file:
      path: "/home/{{ item.username }}/.ssh/"
      state: directory
      owner: "{{ item.username }}"
      group: "{{ item.group }}"
      mode: "0700"
    with_items: "{{ users }}"
    when:
      - "'database' in group_names"
      - "item.uid|string|first == '2'"
  - name: Copy private SSH-key to users on the webserver hosts
    copy:
      src: /home/ali/.ssh/id_rsa.pub
      dest: "/home/{{ item.username }}/.ssh/authorized_keys"
      owner: "{{ item.username }}"
      group: "{{ item.group }}"
      mode: "0600"
    with_items: "{{ users }}"
    when:
      - "'webservers' in group_names"
      - "item.uid|string|first == '1'"
  - name: Copy private SSH-key to users on the database hosts
    copy:
      src: /home/ali/.ssh/id_rsa.pub
      dest: "/home/{{ item.username }}/.ssh/authorized_keys"
      owner: "{{ item.username }}"
      group: "{{ item.username }}"
      mode: "0600"
    with_items: "{{ users }}"
    when:
      - "'database' in group_names"
      - "item.uid|string|first == '2'"
</pre>



# Roles

## Question: 1

- Use ansible Galaxy to download and Install the ansible-galaxy role named geerlingguy.nginx
- The requirement file should install this role.
- change the name to Nginx

### Answer
<pre>
Step 1: Type "ansible-galaxy search nginx --author geerlingguy --platforms EL" to find the geeglinguy.nginx role
Step 2: "ansible-galaxy info geerlingguy.nginx" to check more information about this role.
Step 3: Create requirement.yml file as show below:
</pre>

<pre>
 - src: geerlingguy.nginx
  version: "2.7.0"
  name: Nginx
</pre>

<pre>
Step 4: Run this commad "ansible-galaxy install -r requirement.yml" Step 5: Run "ansible-galaxy list" to make sure that new role has been installed sucessfully. [Note] Prior to this task you need to have ansible-galaxy in your system. Make sure you have added the correct path of roles directory in your ansible.cfg file with roles_path =< Path of your ansible directory to have the roles >
</pre>


## Question: 2

Create a role called apache and store it in your ansible directory “roles”. This role should satisfy the requirements below:

- The httpd, mod_ssl and php packages are installed. Apache service is running and enabled on boot.
- The firewall is configured to allow all incoming traffic on HTTP port TCP 80 and HTTPS port TCP 443.
- Apache service should be restarted every time the file /var/www/html/index.html is modified.
- A Jinja2 template file index.html.j2 is used to create the file /var/www/html/index.html with the following content:

<pre>
The address of the server is: IPV4ADDRESS
</pre>

IPV4ADDRESS is the IP address of the managed node.

Create a playbook /home/automation/plays/apache.yml that uses the role and runs on hosts in the webservers host group.

<br>

### Answer

<pre>
Step 1: Create the empty role by "ansible-galaxy init apache" run this command in your roles directory, for it is /home/ali/ansible/roles
Step 2: Create the handler file roles/apache/handlers/main.yml.
</pre>

<pre>
---
- name: restart_apache
  service:
    name: httpd
    state: restarted
    enabled: yes
</pre>

<pre>
Step 3: Create the main task file roles/apache/tasks/main.yml
</pre>

<pre>
---
- name: Ensure the packages httpd, mod_ssl and php are installed
  yum:
    name: "{{ item }}"
    state: latest
  loop:
    - httpd
    - mod_ssl
    - php
- name: Ensure that the service httpd is enabled
  service:
    name: httpd
    state: started
    enabled: yes

- name: Ensure the firewall ports 80 and 443 are open
  firewalld:
    service: "{{ item }}"
    permanent: yes
    immediate: yes
    state: enabled
  loop:
    - http
    - https
- name: Create index.html from template
  template:
    src: index.html.j2
    dest: /var/www/html/index.html
  notify: restart_apache
...
</pre>

<pre>
Step 4: Create a template file index.html.j2.
</pre>

<pre>
The address of the server is: {{ ansible_facts['default_ipv4']['address']}}
</pre>

<pre>
Step 5: Create a general apache file to run the role.
</pre>

<pre>
---
- hosts: webservers
  become: yes
  roles:
    - apache
</pre>

<br>

## Question: 3

- Create a playbook of any name, and that playbook should do as shown below:
- The playbook runs over all the managed hosts and uses the time sync role ( Red Hat system role)
- This role should be able to change the time server to nl.pool.ntp.org.

### Answer

<pre>
Step 1: you must have RHEL system roles installed and settled all the paths.
Step 2: Copy the roles into your ansible directory or change the path of roles_path in ansible.cfg file.
Step 3: Copy /usr/share/doc/rhel-sytem-roles/timesync/example-timesync-playbook.yml to home directory.
Step 4: Edit the file has shome below:
---
- hosts: all
  vars:
    timesync_ntp_servers:
    - hostname: nl.pool.ntp.org 
      ibrust: yes
  roles:
    - rhel-system-roles.timesync


Step 5: Run shown above yaml file to settle the ntp server.
</pre>


<br>


# Disk Partitions and LVM

## Question: 1-A

- Create a playbook that should perform the following tasks:
- Create a primary partition on /dev/sdb disk with the size of 2GB ( or anything according to your availability: just keep a couple of GBs for the next question).
- Create a volume group named “RedHat”.
- Create an lv named “exam”.
- Formate the new lv as xfs.
- Create a new directory named /mydir on it.
- Mount /mydir so it can sustain the reboot.

### Answer - Q1A

<pre>
---
- hosts: database
  become: yes
  tasks:
  - name: Create The Partition
    parted:
      device: "/dev/sdb"
      number: 1
      state: present
      part_end: 1GiB
  - name: Create VG
    lvg:
      vg: "RedHat"
      pvs: "/dev/sdb1"
  - name: Create LV
    lvol:
      lv: "exam"
      vg: "RedHat"
      size: 500m
  - name: Formate
    filesystem:
      fstype: "xfs"
      dev: "/dev/mapper/RedHat-exam"
  - name: Mount the FS
    mount:
      path: "/mydir"
      src: "/dev/mapper/RedHat-exam"
      fstype: "xfs"
      state: mounted
</pre>

## Question: 2-B

- Create a playbook that performs the following tasks:
- Use the remaining VG “RedHat” space and create a new LV named “ex294”.
- Formate the new lv with ext4.
- Create a folder named “/exam” and mount it so it can sustain the reboot.

### Answer - Q1B

<pre>
---
- hosts: database
  become: yes
  tasks:
  - name: Create New LV On existing VG
    lvol:
      lv: "ex294"
      vg: "RedHat"
      size: 100%FREE
  - name: Format new LV
    filesystem:
      fstype: "ext4"
      dev: "/dev/mapper/RedHat-ex294"
  - name: Mount new volume
    mount:
      path: "/exam"
      src: "/dev/mapper/RedHat-ex294"
      fstype: "ext4"
      state: mounted
</pre>


<br>


# Cron

## Question: 1

- Create a playbook that performs the following tasks:
- Should create a crontab file /etc/cron.d/uptime.
- The task must run by the user ( any user you wish in your server).
- Every 5th minute from 9 to 5 on weekdays.
- The task should run uptime and put an entry here: /home/ali/ansible/my_uptime_cron.txt.

### Answer

<pre>
---
- hosts: all
  become: yes
  tasks:
  - name: Crontab file
    cron:
      name: uptime command
      user: ali
      minute: "*/5"
      hour: 9-5
      days: 1-5
      job: "uptime &amp;amp;amp;amp;amp;amp;amp;gt;&amp;amp;amp;amp;amp;amp;amp;gt; /home/ali/ansible/my_uptime_cron.txt"
      cron_file: my_uptime_cron.txt
      state: present
</pre>

## Question: 2

- Create a playbook that performs the following tasks:
- Create a root crontab record that runs every hour.
- The cron job appends the file /var/log/time.log with the output from the date command.

### Answer

<pre>
---
- hosts: all
  become: yes
  tasks:
  - name: Create crontab-record on proxy hosts
    cron:
      name: "Append the output of 'date' to /var/log/time.log"
      minute: "0"
      job: "date &amp;amp;amp;amp;amp;amp;amp;gt;&amp;amp;amp;amp;amp;amp;amp;gt; /var/log/time.log"
</pre>

## Question 3 
- Create a playbook, which should perform shown below tasks:
- Runs a fstrim command every day at 8:30 AM and 4:30 PM.

### Answer
<pre>
---
- hosts: all
  become: 
  tasks:
  - name: Run Fstrim
    cron:
      name: "Run FSTRIM"
      minute: "30"
      hour: "8,4"
      job: "fstrim -a"
</pre>


## Question: 4
- Create a playbook, which should perform shown below tasks:
- Remove the corn job which you created in Q3.

### Answer
<pre>
---
- hosts: all
  become: 
  tasks:
  - name: Run Fstrim
    cron:
      name: "Run FSTRIM"
      state: absent
</pre>


## Question: 5
- Create a yaml file, which should perform shown below tasks:
- Run a backup of /etc/passwd, /etc/group & /etc/shadow

### Answer
<pre>
---
- hosts: all
  become: yes
  tasks: 
  - name: Backup user data
    cron:
      name: Backup Users
      hour: 5
      minute: 30
      weekday: 1-5
      user: root
      job: 'tar -czf /root/user.tgz /etc/passwd /etc/shadow'
      cron_file: user_backup
</pre>
