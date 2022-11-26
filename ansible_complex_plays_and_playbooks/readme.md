# Managing Complex Plays and Playbooks

## Selecting hosts worth Hosts Patterns
Can be done if you have one or more larger inventory files.

Use asteriks for "everything before and after"
<pre>
[kristofer@workstation projects-host]$ ansible '*.example.com' -i inventory1 \
> --list-hosts
  hosts (14):
    jupiter.lab.example.com
    saturn.example.com
    ...
</pre>


Exclution of hosts
<pre>
[kristofer@workstation projects-host]$ ansible '*.example.com, !*.lab.example.com' \
> -i inventory1 --list-hosts
  hosts (7):
    saturn.example.com
    db1.example.com
    db2.example.com
</pre>


List of specific hosts
<pre>
[kristofer@workstation projects-host]$ ansible \
> lb1.lab.example.com,s1.lab.example.com,db1.example.com -i inventory1 \
> --list-hosts
  hosts (3):
    lb1.lab.example.com
    s1.lab.example.com
    db1.example.com
</pre>


Hosts part of specific groups
<pre>
[kristofer@workstation projects-host]$ ansible 'db,&london' -i inventory1 \
> --list-hosts
  hosts (2):
    db2.example.com
    db3.example.com
</pre>



# Includeing and importing Files
https://github.com/KristoferMar/Ansible-Guide/blob/master/ansible_complex_plays_and_playbooks/including_and_importing_files.yml 