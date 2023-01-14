# 1. Setup an Ansible Inventory 

Edit /etc/ansible/hosts with Vim or whatever editor is handy and comfortable. We'll use Vim here:

[root@server1 ]# vi /etc/ansible/hosts

Your inventory file should look something like this when you're done:

<pre>
[dbservers]
dbserver1

[webservers]
webserver1

[admins]
adminserver1
</pre>

# 2. Setup SSH Keys

The first step in setting SSH key up is generating a key:
<pre>
[root@server1 ]# ssh-keygen
</pre>

Then you can copy it over to webserver1 with the command:

<pre>
[root@server1 ]# ssh-copy-id ansible@dbserver1
</pre>

Repeat this with the other two servers, using the cloud_user password for the ansible user:
<pre>
[root@server1 ]# ssh-copy-id ansible@webserver1

[root@server1 ]# ssh-copy-id ansible@adminserver1
</pre>
Test by logging into one of the servers:

<pre>
[root@server1 ]# ssh ansible@dbserver1
</pre>



# 3. Setup Sudoers

Log into webserver1 as cloud_user:

<pre>
[root@server1 ]# cloud_user@webserver1
</pre>

Now gain root privileges:
<pre>
[cloud_user@webserver1 ]$ sudo -i
</pre>

Run visudo and add the following line to the end of the file:

<pre>
ansible ALL=(ALL) NOPASSWD: ALL
</pre>

Run logout to get out of this server, so that we're back in server1, and then repeat the process for dbserver1 and adminserver1.

Once that's done, test (from server1 with:

<pre>
[root@server1 ]# ssh ansible@webserver1
</pre>
Once you're in, try a sudo command


# 4. Write a Playbook to Install httpd, but Only on Web Servers

Write your playbook . When we're done writing the playbook, it should look something like this:

<pre>
[root@server1 ]# cat httpd.yml
---
- name: Install httpd on webservers
  hosts: webservers
  become: yes

  tasks:
   - yum:
      name: httpd
      state: present

   - service:
      name: httpd
      state: started
      enabled: yes
</pre>

Now run it:
<pre>
[root@server1 ]# ansible-playbook httpd.yml
</pre>
Run it again, just to make sure nothing changes.

# 5. Use an Ad Hoc Command to Install tcpdump on Adminserver1

The simplest ad hoc command here would be

<pre>
[root@server1 ]# ansible -m yum -a "name=tcpdump state=present" adminserver1 --become
</pre>
<br>


# 6. Use the LVM module in a playbook
To setup the disk attached to DBServer1(/dev/xvdg), then make sure it's formatted with XFS and mounted persistently on /mnt/dbdata. The disks's size should be 10G

We're going to create a new yml file,  Your playbook, disk.yml, should look similar to the following:

<pre>
[root@server1 ]# vim disk.yml

---
- name: Review Task 6
  hosts: dbserver1
  become: yes

  tasks:
   - name: LVG create
     lvg:
      vg: RHCE
      pvs: /dev/xvdg

   - name: Logical Volume Setup
     lvol:
      lv: AppDB2
      vg: RHCE
      size: 10G
      pvs: /dev/xvdg
      state: present 

   - name: Format the disk
     filesystem:
      dev: /dev/RHCE/AppDB2
      fstype: xfs
 
   - name: Mount the disk
     mount:
      fstype: xfs
      src: /dev/RHCE/AppDB2
      state: mounted
      path: /mnt/dbdata
</pre>

Save it and quit, then run it:
<pre>
[root@server1 ]# ansible-playbook disk.yml
</pre>

# 7. Create Multiple Users on All Servers

Write your playbook . When we're done writing the playbook, it should look something like this:

<pre>
[root@server1 ]# cat users.yml
---
- name: Review Task 7
  hosts: all
  become: yes

  tasks:
   - name: Create users
     user:
        name: "{{ item }}"
     with_items:
      - john
      - sara
      - adam
      - sam
</pre>

Save it and quit, then run it

<pre>
[root@server1 ]# ansible-playbook users.yml
</pre>

# 8. Ansible facts
Write a Bash script that queries each server for its Ansible facts, and outputs that information to a file in /tmpL; there should be one file for each server name.

We want to get each host's Ansible facts and dump the information into respective text files. So you've got to write a script, facts.sh, that will query each one and put its relevant info into a text file. Create the script:

<pre>
[root@server1 ]# cat facts.sh

#!/bin/bash
for i in webserver1 dbserver1 adminserver1
do
    ansible -m setup $1 > /tmp/$i\_facts
done

[root@server1 ]#
</pre>

Make the script executable:
<pre>
[root@server1 ]# chmod +x facts.sh
</pre>
Now and run it:
<pre>
[root@server1 ]# ./facts.sh
</pre>

Now, to check, run ls /tmp, which should show a file that corresponds to each of those three hosts (dbserver1_facts, adminserver1_facts, and webserver1_facts).
<br><br>

# 9. Create an SSH Configuration File and Distribute it

Currently, there is a file sitting in /root called ssh.tmpl. Open that for editing:

<pre>
[root@server1 ]# vim ssh.tmpl
</pre>

We need to alter two lines (starting with PasswordAuthentication and X11Forwarding). There are a few lines separating them. They should look similar to these:

<pre>
PasswordAuthentication {{ PAanswer }}

...

...

X11Forwarding {{ X11Answer }}
</pre>

Now we need to write a playbook to deploy all that. Create the file:

<pre>
[root@server1 ]# vim ssh.yml
</pre>

To apply the template, the playbook needs to look like this:

<pre>
---

- name: Review Task 9
  hosts: all:!admins
  become: yes
  vars:
   PAanswer: "no"
   X11Answer: "no"

  tasks:
   - name: Apply Template
     template:
      src: /root/ssh.tmpl
      dest: /etc/ssh/sshd_config
      validate: /sbin/sshd -t -f %s

   - name: Restart SSHD
     service:
      name: sshd
      state: restarted

- name: Review Task 9b
  hosts: admins
  become: yes
  vars:
   PAanswer: "no"
   X11Answer: "yes"
 
  tasks:
   - name: Apply template
     template:
      src: /root/ssh.tmpl
      dest: /etc/ssh/sshd_config
      validate: /sbin/sshd -t -f %s

   - name: Restart SSHD
     service:
      name: sshd
      state: restarted
</pre>

Now run it

<pre>
[root@server1 ]# ansible-playbook ssh.yml
</pre>
<br><br>

# 10. Create the Two Roles
The commands to create custom roles are:

<pre>
[root@server1 ]# ansible-galaxy init web

[root@server1 ]# ansible-galaxy init database
</pre>

Just to check, run ls on each of the directories that just got created (ansible and database)
<br><br>

# 11. Configure the database role and Encrypy the Password File

First, get into the files subdirectory of the database directory:

<pre>
[root@server1 ]# cd database/files
</pre>

Create a password file:

<pre>
[root@server1 ]# vim password
</pre>

Put something like this in there:

This is a password

Encrypt it with this:
<pre>
[root@server1 ]# ansible-vault encrypt password
</pre>

Enter a password that you won't forget. To check your work, run cat password and make sure that the file is in fact encrypted.

Now get into the tasks directory:
<pre>
[root@server1 ]# cd ../tasks
</pre>
Edit main.yml:
<pre>
[root@server1 ]# vim main.yml
</pre>
It should look like this when you're done:
<pre>
---
# tasks file for database

- name: Ensure user is created
  user:
   name: dba
 
- name: Copy password file
  copy:
   src: password
   dest: /home/dba
</pre>

Now go back to your home directory:

<pre>
[root@server1 ]# cd
</pre>
Now create db.yml
<pre>
[root@server1 ]# vim db.yml
</pre>
It should look like this when you're done:
<pre>
---
- hosts: dbservers
  roles:
    - database
</pre>

Now run the playbook:

<pre>
[root@server1 ]# ansible-playbook db.yml --become --ask-vault-pass`
</pre>

Enter the password you set in the ansible-vault encrypt password command you ran earlier, and this should work.
<br><br>

# 12. Configure the Web Role and Ensure It Deploys Correctly

Get into the web directory:
<pre>
[root@server1 ]# cd web
</pre>
Now open the main.yml file in the tasks directory:
<pre>
[root@server1 ]# vim tasks/main.yml
</pre>
In the file, paste the following:

<pre>
---
# tasks file for web
- name: Populate index.html
  lineinfile:
   path: /var/www/html/index.html
   create: yes
   line: "{{ inventory_hostname }} {{ansible_facts['all_ipv4_addresses'] }}"

- name: Install httpd
  yum:
   name: httpd
   state: present

- name: Start httpd
  service:
   name: httpd
   state: started
   enabled: yes
</pre>

Now go back to your home directory:
<pre>
[root@server1 ]# cd
</pre>
Here, let's write a new and write a quick role deployment routine. Create the file:

<pre>
[root@server1 ]# vim web.yml
</pre>
It should look like this:

<pre>
---
- hosts: webservers
  roles:
   - web
</pre>

Run the playbook:

<pre>
[root@server1 ]# ansible-playbook web.yml --become
</pre>