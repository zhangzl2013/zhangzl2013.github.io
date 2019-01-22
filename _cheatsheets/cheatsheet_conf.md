---
layout: default
title: cheatsheet_conf
---

# .tmux.conf
```conf
# ~/.tmux.conf
# reload the config: `Ctrl-b: source ~/.tmux.conf`

# switch windows alt+number
bind-key -n M-0 select-window -t 0
bind-key -n M-1 select-window -t 1
bind-key -n M-2 select-window -t 2
bind-key -n M-3 select-window -t 3
bind-key -n M-4 select-window -t 4
bind-key -n M-5 select-window -t 5
bind-key -n M-6 select-window -t 6
bind-key -n M-7 select-window -t 7
bind-key -n M-8 select-window -t 8
bind-key -n M-9 select-window -t 9
bind-key -n M-n next-window
bind-key -n M-p previous-window
# switch pane with alt+arrow
bind-key -n M-Left  select-pane -L
bind-key -n M-Right select-pane -R
bind-key -n M-Up    select-pane -U
bind-key -n M-Down  select-pane -D
# switch pane in vim style
bind-key -n M-h     select-pane -L
bind-key -n M-l     select-pane -R
bind-key -n M-k     select-pane -U
bind-key -n M-j     select-pane -D
```
