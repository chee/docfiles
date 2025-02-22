# zsh.pkg

been using this for like 15 years now. become very startled when it isn't
availabe on a computer

```zsh filename=".zshrc.d/aliases/packages.zsh"
pkg_manager=""

function check_path() {
    /usr/bin/env which $1 &> /dev/null
}

if check_path /opt/homebrew/bin/brew; then
    eval $(/opt/homebrew/bin/brew shellenv)
fi

check_path nix-env && pkg_manager=nix
check_path pacman && pkg_manager=pacman
check_path yay && pkg_manager=yay
check_path port && pkg_manager=macports
check_path brew && pkg_manager=homebrew
check_path apt-get && pkg_manager=apt
check_path dnf && pkg_manager=dnf
check_path emerge && pkg_manager=emerge

case $pkg_manager in
	nix)
		alias get="nix-env -i"
		alias search="nix-env -qa"
		alias remove="nix-env --uninstall"
		alias upgrade="nix-channel --update && nix-env --upgrade"
		alias information="nix-env -qa --description"
		;;
	apt)
		alias get="sudo apt install"
		alias search="apt search"
		alias remove="sudo apt remove"
		alias upgrade="sudo apt update; sudo apt upgrade"
		alias information="apt info"
		;;
	yay)
		alias get="yay -S"
		alias search="yay -Ss"
		alias remove="yay -R"
		alias upgrade="yay -Syu"
		alias information="yay -Si"
		;;
	pacman)
		alias get="sudo pacman -S"
		alias search="sudo pacman -Ss"
		alias remove="sudo pacman -R"
		alias upgrade="sudo pacman -Syu"
		alias information="sudo pacman -Si"
		;;
	homebrew)
		alias get="brew install --force-bottle"
		alias search="brew search"
		alias remove="brew remove"
		alias upgrade="brew update; brew upgrade"
		alias information="brew info"
		;;
	macports)
		alias get="sudo port install"
		alias search="port search"
		alias remove="port uninstall"
		alias upgrade="sudo port selfupdate; sudo port upgrade outdated"
		alias information="port info"
		;;
	dnf)
		alias get="sudo dnf install"
		alias search="dnf search"
		alias remove="dnf remove"
		alias upgrade="sudo dnf upgrade"
		alias information="dnf info"
		;;
	emerge)
		echo "who hurt you"
		;;
	*)
		echo "what operating system are you on?"
esac
```
