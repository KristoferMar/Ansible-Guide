# Troubleshooting Playbooks

## Configure ansible log
- Create the file ansible.cfg with the following pointers as example

<pre>
[defaults]
log_path = /home/student/troubleshoot-playbook/ansible.log
inventory = /home/student/troubleshoot-playbook/inventory
</pre>

## Verbosity for troubleshoot
Using the highest level of verbosity with Ansible, examining the Ansible log file is a better option than checking console output.

Running the plyabook
<pre>
[kristofer@workstation troubleshoot-playbook]$ ansible-playbook -vvvv samba.yml
</pre>

Roubleshooting the playbook
<pre>
[kristofer@workstation troubleshoot-playbook]$ grep -i fatal ansible.log
</pre>