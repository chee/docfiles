## zsh git

### clone helpers

apparently at some time in my life it seemed vitally important to optimize the
"cloning git repositories and cding into them" part of my workflow

```zsh filename=".zshrc.d/functions/git.zsh"
function gcl() {
	git clone --recursive $*
	if [ -z "$2" ]; then
		dir="$(basename $1)"
		dir="${dir%%.git}"
	else
		dir="$2"
	fi
	cd $dir
}

function ghcl() {
	gcl git@github.com:$*
}

function ncl() {
	ghcl netlify/$*
}

function cl() {
	gcl chee@chee.party:repos/$* || gcl chee@chee.party:repos/private/$*
	git remote set-url --add origin git@github.com:chee/$1
}
```

### git aliases

```zsh filename=".zshrc.d/aliases/git.zsh"
alias ga="git add -p"
alias gc="git commit -v"
alias gcp="git cherry-pick"
alias gd="git diff"
alias gpl="git pull"
alias gps="git push"
alias gr="git root"
alias gs='git status; git ls-files -v | grep \^h'
```
