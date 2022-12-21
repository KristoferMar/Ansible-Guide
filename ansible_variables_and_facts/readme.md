## Variables and facts


## Using variables and facts
Remember to put the variables and facts in quotes
<pre>
  tasks:
    - name: Required packages are installed and up to date
      yum:
        name:
          - "{{ web_pkg  }}"
          - "{{ firewall_pkg }}"
          - "{{ python_pkg }}"
        state: latest
</pre>


## Using secrets
How to edit secrets

<pre>
[kris@workstation data-secret]$ ansible-vault edit secret.yml
Vault password: thePasswordForVault
</pre>


## Managing Facts
Ansible facts are the host-specific system data and properties to which you connect. A fact can be the IP address, BIOS information etc.

The setup command retrieves facts from a machine
<pre>
[kris@workstation data-facts]$ ansible webserver -m setup
</pre>

### Creating custom facts
You can create custom facts and copy them over to any given host the following way

1. Create the the .fact file with all specified data - in my case it's called custom.fact and looks like this.
<pre>
[general]
package = httpd
service = httpd
state = started
enabled = true
</pre> 

2. Copy the file into /etc/ansible/facts.d direcroy on the target host
<pre>
---
- name: Install remote facts
  hosts: webserver
  vars:
    remote_dir: /etc/ansible/facts.d
    facts_file: custom.fact
  tasks:
    - name: Create the remote directory
      file:
        state: directory
        recurse: yes
        path: "{{ remote_dir }}"
    - name: Install the new facts
      copy:
        src: "{{ facts_file }}"
        dest: "{{ remote_dir }}"
</pre>
