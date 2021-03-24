# Description

My server configuration files. feel free to steal whatever you like.

See also my [personal dotfiles](https://git.compilenix.org/CompileNix/dotfiles) repo.

# Install
__Keep always an old terminal open, in case of failures!__

```sh
curl https://git.compilenix.org/CompileNix/server-dotfiles/-/raw/master/install.sh | bash
```

# Requirements
- python 3.7+
- git
- zsh
- neovim or vim
- sudo
- tee

## Debian / Ubuntu
```sh
# How to get rid of purple background color in newt apps? -> https://askubuntu.com/q/750237
sudo ln -sf /etc/newt/palette.original /etc/alternatives/newt-palette

sudo apt install python3 python3-pip python git zsh vim vim-airline neovim tmux curl wget net-tools acl htop ncdu iftop iotop mutt lsb-release rsync brotli gzip zip unzip bind9utils language-pack-de build-essential cmake sqlite mlocate
```

## Fedora
```sh
sudo dnf install util-linux-user findutils which python3 python git zsh vim vim-airline neovim tmux curl wget ncdu redhat-lsb-core python3-pip NetworkManager-tui acl htop iftop iotop mutt bind-utils rsync iptables langpacks-de make gcc-c++ gcc cmake sqlite mlocate
```

# Update
Use the zsh function `update-dotfiles`.

If you have a really old version, you may need to update it manually.

## Manual Update
Copy and paste into terminal.

__Keep always an old terminal open, in case of failures!__

```sh
cd ~/.homesick/repos/server-dotfiles
git status
popd >/dev/null
echo "This will reset all changes you may made to files which are symlinks at your home directory, to check this your own: \"# cd ~/.homesick/repos/dotfiles && git status\""
echo "Do you want proceed anyway?"
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

# Windows
## Tools / Software
- [7-ZIP](https://www.7-zip.org/): archive file management
- [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer): more advanced Task Manager
- [TeraCopy](https://www.codesector.com/teracopy): better file copy & move
- [Visual Studio Code](https://code.visualstudio.com/download): text editor
- [NetLimiter](https://www.netlimiter.com/): alternative firewall (not based on Windows Firewall)
