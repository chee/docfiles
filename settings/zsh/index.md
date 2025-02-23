# zsh config

i'm back, baby!

```zsh filename=".zshrc"
# .--------------------------------------------.
# | these are prioritised by 10- sort aliasing |
# *--------------------------------------------*

# 0x: zsh setup (prompt, completion, etc.)
# 1x: environment variables
# 2x: alias
# 3x: reserved
# 4x: initialisation of programs with their own shell scripts
# 5x: reserved
# 6x: functions
# 7x: reserved
# 8x: miscellaneous settings and setup
# 9x: client

for sourceable in $HOME/.zshrc.d/*.zsh; do
	source $sourceable
done

fpath=($fpath "$HOME/.zfunctions")
```

## 00-zsh

the base config to get zsh to behave normally

```zsh filename=".zshrc.d/00-zsh.zsh"
# chee's zsh config
current_path="$PATH"
source /etc/profile
export PATH="$current_path:$PATH"
bindkey -e

setopt nobeep
HISTFILE=$HOME/.zsh_history
HISTSIZE=SAVEHIST=100000000000000000
setopt append_history
setopt share_history
setopt extended_history
setopt histignorealldups
setopt histignorespace

setopt auto_cd
setopt auto_pushd
setopt pushd_ignoredups
typeset -U path cdpath fpath manpath

declare -A abk
setopt interactivecomments

setopt extendedglob
setopt extended_glob
setopt noshwordsplit
setopt unset

setopt longlistjobs
setopt notify

setopt nonomatch

setopt hash_list_all
setopt completeinword

setopt nohup

for mod in parameter complist deltochar mathfunc ; do
	zmodload -i zsh/${mod} 2>/dev/null || print "Notice: no ${mod} available :("
done
zmodload -a  zsh/stat    zstat
zmodload -a  zsh/zpty    zpty
zmodload -ap zsh/mapfile mapfile

autoload compinit

zstyle ':completion:*:approximate:' max-errors 'reply=( $((($#PREFIX+$#SUFFIX)/3 )) numeric )'
zstyle ':completion:*:complete:-command-::commands' ignored-patterns '(aptitude-*|*\~)'
zstyle ':completion:*:correct:*' insert-unambiguous true
zstyle ':completion:*:corrections' format $'%{\e[0;31m%}%d (errors: %e)%{\e[0m%}'
zstyle ':completion:*:correct:*' original true
zstyle ':completion:*:default' list-colors ${(s.:.)LS_COLORS}
zstyle ':completion:*:descriptions' format $'%{\e[0;31m%}completing %B%d%b%{\e[0m%}'
zstyle ':completion:*:*:cd:*:directory-stack' menu yes select
zstyle ':completion:*:expand:*' tag-order all-expansions
zstyle ':completion:*:history-words' list false
zstyle ':completion:*:history-words' menu yes
zstyle ':completion:*:history-words' remove-all-dups yes
zstyle ':completion:*:history-words' stop yes
zstyle ':completion:*' matcher-list 'r:|?=** m:{a-z\-}={A-Z\_}'
zstyle ':completion:*' matcher-list 'm:{a-z\-}={A-Z}\_'
zstyle ':completion:*:matches' group 'yes'
zstyle ':completion:*' group-name ''
zstyle ':completion:*' menu select=5
zstyle ':completion:*:messages' format '%d'
zstyle ':completion:*:options' auto-description '%d'
zstyle ':completion:*:options' description 'yes'
zstyle ':completion:*:processes' command 'ps -au$USER'
zstyle ':completion:*:*:-subscript-:*' tag-order indexes parameters
zstyle ':completion:*' verbose true
zstyle ':completion:*:-command-:*:' verbose false
zstyle ':completion:*:warnings' format $'%{\e[0;31m%}No matches for:%{\e[0m%} %d'
zstyle ':completion:*:*:zcompile:*' ignored-patterns '(*~|*.zwc)'
zstyle ':completion:correct:' prompt 'correct to: %e'
zstyle ':completion::(^approximate*):*:functions' ignored-patterns '_*'
zstyle ':completion:*:processes-names' command 'ps c -u ${USER} -o command | uniq'
zstyle ':completion:*:manuals' separate-sections true
zstyle ':completion:*:manuals.*' insert-sections true
zstyle ':completion:*:man:*' menu yes select
zstyle ':completion:*:sudo:*' command-path /usr/local/sbin \
	/usr/local/bin  \
	/usr/sbin       \
	/usr/bin        \
	/sbin           \
	/bin            \
	/usr/X11R6/bin
zstyle ':completion:*' special-dirs ..
setopt correct
zstyle -e ':completion:*' completer '
if [[ $_last_try != "$HISTNO$BUFFER$CURSOR" ]] ; then
	_last_try="$HISTNO$BUFFER$CURSOR"
	reply=(_complete _match _ignored _prefix _files)
else
	if [[ $words[1] == (rm|mv) ]] ; then
		reply=(_complete _files)
	else
		reply=(_oldlist _expand _complete _ignored _correct _approximate _files)
	fi
fi'
zstyle ':completion:*:urls' local 'www' '/var/www/' 'public_html'
zstyle ':vcs_info:*' enable git svn hg

zstyle ':completion:*'  matcher-list \
	'' \
	'm:{a-z\-}={A-Z\_}' \
	'r:[^[:alpha:]]||[[:alpha:]]=** r:|=* m:{a-z\-}={A-Z\_}' \
	'r:|?=** m:{a-z\-}={A-Z\_}'

autoload zmv
autoload zed
autoload colors
autoload -Uz vcs_info
setopt prompt_subst

precmd() {
    vcs_info
    echo -n $host_reminder
}

newline=$'\n'
host_reminder="${newline}"
p0="%F{red}%(?..%? )%f"
p1="%F{blue}%n%f${NO_COLOR}%m%f${NO_COLOR}%40<...<%B%~%b%<<"'${vcs_info_msg_0_}'"${newline}▶ "

if (( EUID == 0 )); then
	PROMPT="%F{blue}${p0}%f%F{red}${p1}%f"
else
	PROMPT="%F{red}${p0}%f%F{blue}${p1}%f"
fi
unset p0 p1

bindkey "^[[1~" beginning-of-line
bindkey "^[[4~" end-of-line
bindkey "^[[H" beginning-of-line
bindkey "^[[F" end-of-line
bindkey "^[OH" beginning-of-line
bindkey "^[OF" end-of-line
bindkey "^[^[[C" forward-word
bindkey "^[^[[D" backward-word
```

Here i remove the `-` and `_` from wordchars because i often want to jump
between kebab parts with alt-b/alt-f

```zsh filename=".zshrc.d/00-zsh.zsh"
#WORDCHARS='*?_-.[]~=&;!#$%^(){}<>'
WORDCHARS='*?.[]~=&;!#$%^(){}<>'
```

## environment vars

```zsh filename=".zshrc.d/10-env.zsh"
env=(
	BROWSER=firefox
	EDITOR='code -w'
	LESS_TERMCAP_mb=$'\E[01;31m'
	LESS_TERMCAP_md=$'\E[01;31m'
	LESS_TERMCAP_me=$'\E[0m'
	LESS_TERMCAP_se=$'\E[0m'
	LESS_TERMCAP_so=$'\E[01;44;33m'
	LESS_TERMCAP_ue=$'\E[0m'
	LESS_TERMCAP_us=$'\E[01;32m'
	GOPATH=$HOME/go
	PAGER="less -FIRX --use-color -DS233.222 -DE231.197 -Dd+69 -Du+37"
)

if [ "$TERM" == 'xterm' ]; then
	env=(TERM=xterm-256color $env)
fi

if which ack > /dev/null; then
	env=($env FZF_DEFAULT_COMMAND='ack -g ""')
fi

if which ag > /dev/null; then
	env=($env FZF_DEFAULT_COMMAND='ag -g ""')
fi

for var in $env; do
	export $var
done
```

## $path config

```zsh filename=".zshrc.d/10-path.zsh"
path=(
       $HOME/bin
       # locally built binaries live in .local/bin
       $HOME/.local/bin
       $GOPATH/bin
       # surpisingly missing in some of environments
       /usr/local/bin
       /usr/local/sbin
       # this causes a bunch of things to fail if they're written assuming bsd tools
		 # imo those things have bugs
       /opt/homebrew/opt/coreutils/libexec/gnubin/
   $path
)

fpath=(
	$HOME/.local/share/zsh/site-functions/
	$HOME/.local/share/zsh/site-functions/
	$HOME/.local/share/zsh-completions/
	$HOME/.zshrc.d/completions/
	/usr/share/zsh/site-functions/
	/usr/local/share/zsh/site-functions/
	$fpath
)

# if gnu coreutils are installed, i'd like to see their manpages
export MANPATH=/opt/homebrew/opt/coreutils/libexec/gnuman:$MANPATH

compinit

export PATH
```

## aliases

```zsh filename=".zshrc.d/20-alias.zsh"
alias change-directory="cd"
alias c="pygmentize -O style=monokai -f console256 -g"
alias cx="chmod +x"
alias edit="$EDITOR"
alias grc="git rebase --continue"
alias grep="grep --color=auto"
alias less="less -FIRX --use-color -DS233.222 -DE231.197 -Dd+69 -Du+37"
alias ls="ls --color -F"
alias manual="man"
alias npr="npm run-script -s"
alias please="" # be polite
alias root="project"
alias sdot="source ~/.zshrc"
alias sshd='sudo mkdir /var/run/sshd; sudo `sudo which sshd`'
alias sudo="sudo "
alias thanks="echo you\'re welcome #"
source ~/.zshrc.d/aliases/git.zsh
source ~/.zshrc.d/aliases/packages.zsh
```

## functions

```zsh filename=".zshrc.d/60-function.zsh"
source ~/.zshrc.d/functions/git.zsh

function mkcd () {
	mkdir -p "$1"
	cd "$1"
}

function cd () {
	if (( ${#argv} == 1 )) && [[ -f ${1} ]]; then
		[[ ! -e ${1:h} ]] && return 1
		print "s/${1}/${1:h}/"
		builtin cd ${1:h}
	else
		builtin cd "$@"
	fi
}

npmball () {
	curl -O $(npm view "$1" dist.tarball)
}
```

## miscellaneous settings and setup

```zsh filename=".zshrc.d/80-misc.zsh"
mkdir -p /tmp/chee/.ssh
export VOLTA_HOME=$HOME/.volta
export path=("$VOLTA_HOME/bin" $path)
[ -n "$EAT_SHELL_INTEGRATION_DIR" ] && \
 source "$EAT_SHELL_INTEGRATION_DIR/zsh"
```

### oh, to be a nintendo

```zsh filename=".zshrc.d/80-misc.zsh"
export DEVKITPRO=/opt/devkitpro
export DEVKITARM=$DEVKITPRO/devkitARM
export DEVKITPPC=$DEVKITPRO/devkitPPC
export PATH="$PATH:$DEVKITPRO/tools/bin"
export PATH="$PATH:$DEVKITPRO/pacman/bin"
```

## eat

this emacs shell is better than the others

```zsh filename=".zshrc.d/82-eat.zsh"
if [ ! -z "$EAT_SHELL_INTEGRATION_DIR" ]; then
   source "$EAT_SHELL_INTEGRATION_DIR/zsh"
   export LC_ALL=en_CA.UTF-8
fi
```

## 99-client _(example)_

a ~/.zshrc.d/99-client.zsh is usually made fresh on every machine, for stuff
that's specific to that computer

```zsh
source /opt/homebrew/share/zsh-autosuggestions/zsh-autosuggestions.zsh
source /opt/homebrew/share/zsh-fast-syntax-highlighting/fast-syntax-highlighting.plugin.zsh
ZSH_AUTOSUGGEST_HIGHLIGHT_STYLE='fg=5'
export PATH=$PATH
# source /opt/homebrew/etc/profile.d/z.sh
export GUILE_LOAD_PATH="/opt/homebrew/share/guile/site/3.0"
export GUILE_LOAD_COMPILED_PATH="/opt/homebrew/lib/guile/3.0/site-ccache"
export GUILE_SYSTEM_EXTENSIONS_PATH="/opt/homebrew/lib/guile/3.0/extensions"
```
