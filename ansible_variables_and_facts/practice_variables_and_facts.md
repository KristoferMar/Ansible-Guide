Question: 1
        Create a playbook, which can do as shown below:
        Create a file on all the servers at this path /etc/motd.
        The file contains “MY SERVER”.

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
        Create a playbook, which can do as shown below:
        Create a file on all the servers at this path /etc/motd.
        The file contains  “MY SERVER: <Server Name> “.

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
      content: "This is DATABASE Server&amp;amp;amp;amp;amp;amp;amp;quot"
    when: "'database' in group_names"
</pre>