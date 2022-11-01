# Vim configurations

## Customize vim to prettify yaml 
Create a "$HOME/.vimrc" to personalize vim for your user. In that file you can putit.
<pre>
autocmd FileType yaml setlocal ai ts=2 sw=2 et nu
autocmd FileType yaml colo desert
</pre>

## See where lines end and begin
See where lines begin and where they end
<pre>:set list</pre>