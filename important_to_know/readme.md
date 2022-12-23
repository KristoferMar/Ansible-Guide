# Important to know

# Ansible facts
## Get facts from remote host
<pre>
ansible -m setup -i inventory.yml all 
 
    or

ansible serverb.lab.example.com -m setup
</pre>



# VIM

## See where lines end and begin
See where lines begin and where they end
<pre>:set list</pre>

## Delete block
1. Put your cursor on the top line of the block of text/code to remove.
2. Press V (That's capital "V" : Shift + v )
3. Move your cursor down to the bottom of the block of text/code to remove.
4. Press d. 


## Move multiple lines
1. Go into Visual with "shift + Ctl + v"
2. Mark all lines you want to move
3. pree "<<" to go left and ">>" to go right
