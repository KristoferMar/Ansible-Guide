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


# Facts
When written in playbooks, ansible facts have the following layout

Task to display ip adress
<pre>
- name: Get node ip adress
  debug:
    msg: "{{ ansible_facts['default_ipv4']['address'] }}
</pre>

task to display FQDN
<pre>
- name: Get fully qualified domain name
  debug:
    msg: "{{ ansible_facts['fqdn'] }}
</pre>

Task to display hostname
<pre>
- name: Get hostname
  debug:
    msg: "{{ ansible_facts['hostname']}
</pre>


## when used in a 'when'
When ansible facts are used inside a when flag then we dont have to specify "ansible_facts['something']", we can just use the facts directly with their full name eg.

<pre>
- fail:
    msg: Fail when criterias are not met
  when: >
    ansible_memtotal_mb > min_ram_mb
</pre>

# built-in variables
Importnat built-in variables
- inventory_hostname
- ansible_hostname
  - takes the hostname of the remote machine from the facts collected during the gather_facts section.
<pre>
ansible prod -m debug -a "msg={{ inventory_hostname }}"
</pre>

Example of real usecase
<pre>
---
- name:
  hosts: all
  tasks:
    - name:
      copy:
        content: "Development"
        dest: /etc/issue
      when: inventory_hostname in groups['dev']
</pre>