---
layout: single
title: "[Notes] My develop settings when reinstall"
date: 2019-08-30
modified:
description:
categories:
    - "Notes"
tags:
header:
---

Record my development environment

## Install Homebrew
```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

## Install develop tools
```sh
brew cask install visual-studo-code
brew cask install iterm2 
brew cask install docker
brew tap caskroom/fonts
brew install zsh
brew install python
brew install node
```

## Setting Zsh as default & install oh-my-zsh
```sh
sudo sh -c "echo $(which zsh) >> /etc/shells" 
chsh -s $(which zsh)
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## iTerm settings
```sh
# Preferences > Profiles > Terminal > Report Terminal Type> xterm-256color
git clone https://github.com/mbadolato/iTerm2-Color-Schemes.git
# iTerm import Tomorrow Night Eighties & select
git clone https://github.com/bhilburn/powerlevel9k.git ~/.oh-my-zsh/custom/themes/powerlevel9k

vi ~/.zshrc
ZSH_THEME="powerlevel9k/powerlevel9k"
# Left
POWERLEVEL9K_LEFT_PROMPT_ELEMENTS=(context dir dir_writable vcs vi_mode)
# Right
POWERLEVEL9K_RIGHT_PROMPT_ELEMENTS=(status background_jobs history ram load time)
# Hide user name
DEFAULT_USER="your_username"
# nerd font
POWERLEVEL9K_MODE='nerdfont-complete'
```

## Setting Git & GitHub with SSH
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
cd ~/.ssh
cat id_rsa.pub
ssh git@github.com
git config --global user.name "your_username"
git config --global user.email "your_email@example.com"
```