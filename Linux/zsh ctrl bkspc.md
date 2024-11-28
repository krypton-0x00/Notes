```bash
#BINDS

bindkey '^[[1;5C' forward-word
bindkey '^[[1;5D' backward-word
# Bind Ctrl + Backspace (Ctrl + H) to delete a word
bindkey '^H' backward-kill-word

# Ensure Backspace key (normally it's ^? on most terminals) works as a normal backspace
bindkey '^?' backward-delete-char
```