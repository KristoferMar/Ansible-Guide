# Important to know

## Get facts from remote host
<pre>
ansible -m setup -i inventory.yml all 
 
    or

ansible serverb.lab.example.com -m setup
</pre>



# VIM
## Delete block
1. Put your cursor on the top line of the block of text/code to remove.
2. Press V (That's capital "V" : Shift + v )
3. Move your cursor down to the bottom of the block of text/code to remove.
4. Press d. 
