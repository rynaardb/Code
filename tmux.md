# tmux

## Basic commands

```bash
# Start a new session
tmux

# Start a new named session
tmux new -s <name>

# Attach to session
tmux a

# Attach to named session
tmux a -t <name>

# List sessions
tmux ls
```

## Prefix

In tmux the default prefix ⌃ + B (CTRL + B)

## Windows & Tabs

```bash
c  create window
w  list windows
n  next window
p  previous window
f  find window
,  name window
&  kill window
```

## Panes & Splits 
```bash
%  vertical split
"  horizontal split
    
o  swap panes
q  show pane numbers
x  kill pane
+  break pane into window (e.g. to select text by mouse to copy)
-  restore pane from window
⍽  space - toggle between layouts
<prefix> q (Show pane numbers, when the numbers show up type the key to goto that pane)
<prefix> { (Move the current pane left)
<prefix> } (Move the current pane right)
<prefix> z toggle pane zoom
```