# Description

My server configuration files. feel free to steal whatever you like.

See also my [personal dotfiles](https://git.compilenix.org/CompileNix/dotfiles) repo.

# Requirements
- python 3.7+
- git
- zsh
- vim
- sudo
- tee

## Debian / Ubuntu
```sh
sudo apt install python3 python3-pip python git zsh vim vim-airline tmux curl wget net-tools htop ncdu iftop iotop mutt lsb-release rsync brotli gzip zip unzip bind9utils language-pack-de neovim postfix
```

### More Packages
```sh
sudo apt install build-essential cmake
```

## Fedora
```sh
sudo dnf install python3 python git zsh vim vim-airline tmux curl wget ncdu redhat-lsb-core python3-pip htop iftop iotop mutt bind-utils rsync iptables langpacks-de neovim NetworkManager-tui postfix
```
### More Packages
```sh
sudo dnf install make gcc-c++ gcc cmake sqlite
```

## CentOS 7
```sh
sudo yum install python3 python git zsh vim vim-airline tmux curl wget redhat-lsb-core make gcc-c++ gcc ncurses-devel python3-pip ncdu htop iftop iotop mutt bind-utils rsync iptables langpacks-de neovim NetworkManager-tui postfix
# because centos ships an ancient version of ZSH we have to build a recent version by our self
# see https://sourceforge.net/projects/zsh/files/zsh/
cd /opt
wget https://sourceforge.net/projects/zsh/files/zsh/5.8/zsh-5.8.tar.xz/download
tar -xJf download
rm -f download
cd zsh-*
./configure
make -j$(nproc) && sudo make install
cd ..
rm -rf zsh-*
cd ~
echo "/usr/local/bin/zsh" >>/etc/shells
chsh -s /usr/local/bin/zsh
exec zsh
```

### More Packages
```sh
sudo yum install cmake
```

# Install
__Keep always an old terminal open, in case of failures!__

```sh
curl https://git.compilenix.org/CompileNix/server-dotfiles/-/raw/master/install.sh | bash
```

# Update
Copy and paste into terminal.

__Keep always an old terminal open, in case of failures!__

```sh
cd ~/.homesick/repos/server-dotfiles
git status
popd >/dev/null
echo "This will reset all changes you may made to files which are symlinks at your home directory, to check this your own: \"# cd ~/.homesick/repos/server-dotfiles && git status\"\nDo you want preced anyway?"
function ask_yn_y_callback {
    if [[ $EUID -eq 0 ]]; then
        rm /usr/local/bin/tmux-mem-cpu-load
    fi
    pushd ~
    rm -rf .vim/bundle
    pushd ~/.homesick/repos
    rm -rf server-dotfiles
    git clone --recursive https://git.compilenix.org/CompileNix/server-dotfiles.git
    popd >/dev/null
    pushd ~
    rm -rf .antigen
    ln -sfv .homesick/repos/server-dotfiles/antigen .antigen
    popd >/dev/null
    antigen-cleanup
    git-reset ~/.homesick/repos/*
    homeshick pull
    homeshick link
    antigen update
    rm ~/.tmux.conf_configured

    exec zsh
}
function ask_yn_n_callback {
    echo -n ""
}
ask_yn
```
