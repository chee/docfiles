# `~/.gitconfig` && `~/.gitconfig.d/*`

## aliases

```zsh filename=".gitconfig.d/alias"
[alias]
	co = checkout
	cob = checkout -b
	cp = cherry-pick
	fire = !git add . && git commit -m "FIRE" && git pull && git push --no-verify
	wt = "!wt() { git worktree add --checkout --guess-remote ../\"$1\" \"$1\"; }; wt"
	last-branch = !git checkout $(git branch --sort=-committerdate | head -2 | tail -1)
	root = rev-parse --show-toplevel
	staged = diff --cached
```

## shell prompt settings

```zsh filename=".gitconfig.d/bash"
[bash]
	showInformativeStatus = true
	showDirtyState = true
	showUntrackedFiles = true
```

## branch settings

```zsh filename=".gitconfig.d/branch"
[branch]
autosetuprebase = always
autosetupmerge = true
```

## life is colourful

```zsh filename=".gitconfig.d/color"
[color]
ui = auto
interactive = auto
diff = auto
branch = auto
status = auto
```

## global ignore file

```gitignore filename=".gitignore"
Icon
*~
.DS*Store
.AppleDouble
.LSOverride
.Spotlight-V100
.Trashes
.#* #_#
,_\_flymake._
.tern-port
.log/t_.log
```

### gotta tell git about it

```zsh filename=".gitconfig.d/core"
[core]
excludesfile = ~/.gitignore
askPass = /bin/ksshaskpass
```

## sign commits with ssh key

```zsh filename=".gitconfig.d/signing"
[commit]
gpgsign = true
[user]
signingkey = ~/.ssh/id_ed25519.pub
[gpg]
format = ssh
```

## format

what is this?

```zsh filename=".gitconfig.d/format"
[format]
numbered = auto
```

## autocorrect to a close command quickly

```zsh filename=".gitconfig.d/help"
[help]
autocorrect = 5
```

## set a default branch

```zsh filename=".gitconfig.d/init"
[init]
defaultBranch = main
```

## decorate logs with refs

```zsh filename=".gitconfig.d/log"
[log]
decorate = short
```

## pulls

pulls are always rebases

```zsh filename=".gitconfig.d/pull"
[pull]
rebase = true
```

## pushes

pushes default to the current branch

```zsh filename=".gitconfig.d/push"
[push]
default = current
autosetupremote = true
```

## automatically stash dirty tree before rebasing

```zsh filename=".gitconfig.d/rebase"
[rebase]
autostash = true
```

## rerere - let's not go through all that again

stores previous conflict solves and automatically reäpplies them. i'm surprised
this has never caused me any trouble, it's only ever a good time.

```zsh filename=".gitconfig.d/rerere"
[rerere]
enabled = true
```

## tell me about yourself

that's me, a little rabbit

```zsh filename=".gitconfig.d/user"
[user]
name = chee
email = yay@chee.party
zodiac = pisces

[github]
user = chee
```

-  the include file

git lets you use a .gitconfig.d folder, though it isn't really very convenient,
you still need to include every file. i need to add some kind of scripting to
lima so this can be done automatically.

```zsh filename=".gitconfig"
[include]
path = ~/.gitconfig.d/alias
path = ~/.gitconfig.d/bash
path = ~/.gitconfig.d/branch
path = ~/.gitconfig.d/color
path = ~/.gitconfig.d/core
path = ~/.gitconfig.d/credential
path = ~/.gitconfig.d/filter
path = ~/.gitconfig.d/format
path = ~/.gitconfig.d/help
path = ~/.gitconfig.d/init
path = ~/.gitconfig.d/log
path = ~/.gitconfig.d/pull
path = ~/.gitconfig.d/push
path = ~/.gitconfig.d/rebase
path = ~/.gitconfig.d/rerere
path = ~/.gitconfig.d/signing
path = ~/.gitconfig.d/user
```
