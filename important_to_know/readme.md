# Important to know

## Get facts from remote host
<pre>
ansible -m setup -i inventory.yml all 
 
    or

ansible serverb.lab.example.com -m setup
</pre>