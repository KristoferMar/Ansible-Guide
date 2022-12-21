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
- The file contains  “MY SERVER: <Server Name> “.

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