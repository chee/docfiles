# tmux config

```sh filename=".tmux.conf"
set -g status-keys emacs
```

## binding

i press `C-b` to go back a character. i use `transpose` a lot too, but not often
enough that it's annoying to press `t` twice. binding `t` to send `C-t` helps
with that as well as nested tmices.

```sh filename=".tmux.conf"
unbind C-b
set -g prefix C-t
bind-key t send-prefix
```

## speed

this makes it feel responsive, smarter, more aggressive. 👾

```sh filename=".tmux.conf"
set -s escape-time 10
set -g focus-events on
setw -g aggressive-resize on
```

## theme

a deep crimson background for the toolbar. tmux spells "colour" with a "u".

🇨🇦

```sh filename=".tmux.conf"
set-option -g status-bg '#b02352'
set-option -g status-fg colour231
set-option -g status-position bottom
```

## 🐭

let's all be mice for a change.

did you know tmux has a context menu?? it's really cute.

`C-t m` to toggle mouse. thanks remy

```sh filename=".tmux.conf"
set -g mouse
bind-key m set mouse
bind -Tcopy-mode WheelUpPane send -N1 -X scroll-up
bind -Tcopy-mode WheelDownPane send -N1 -X scroll-down
```

```sh filename=".tmux.conf"
bind-key C-n next-window
bind-key C-p previous-window
bind-key R source-file ~/.tmux.conf
set -g base-index 1
set -g display-time 1000
```

## history

i prioritize my memories over the computer's

```sh filename=".tmux.conf"
set -g history-limit 50000
```

## miscellany

```sh filename=".tmux.conf"
set -g set-titles on
set -g status-interval 1
set -ga terminal-overrides ',xterm-256color:Tc'
set -ga terminal-overrides ',xterm-termite:Tc'
setw -g automatic-rename on
setw -g monitor-activity on
setw -g visual-activity on
```
