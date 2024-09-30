+++
title = "Ubuntu Server Setup"
date = "2024-09-30T16:17:54+08:00"
description = "Ubuntu Server Setup"
tags = ["cs","journal"]
+++

## Rename Host

* `sudo nano /etc/hostname`
* `sudo nano /etc/hosts`
* `sudo reboot`

## Change Password

* `sudo passwd username`

## Change Root Password

* `sudo -i`
* `passwd`

## 强制SSH使用密钥认证
```
sudo vim /etc/ssh/sshd_config

PasswordAuthentication no
PubkeyAuthentication yes
```
重启服务
```
sudo service ssh restart
```

## SSH

```
ssh-keygen

cat ~/.ssh/id_ed25519.pub >> ~/.ssh/authorized_keys

scp ubuntu@ip:/home/ubuntu/.ssh/id_ed25519 ~/.ssh/
```

## oh-my-zsh + powerlevel10k

```
# Install git fontconfig zsh etc.
sudo apt install git vim fontconfig curl zsh

# Install oh-my-zsh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Install powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

# edit .zshrc
ZSH_THEME="powerlevel10k/powerlevel10k"

# Install Fonts
sudo wget -P /usr/local/share/fonts https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf
sudo wget -P /usr/local/share/fonts https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf
sudo wget -P /usr/local/share/fonts https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf
sudo wget -P /usr/local/share/fonts https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf

fc-cache -fv

# Install Plugins

git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions

git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting

git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions

# add the following line before `source "$ZSH/oh-my-zsh.sh"` in `.zshrc`
fpath+=${ZSH_CUSTOM:-${ZSH:-~/.oh-my-zsh}/custom}/plugins/zsh-completions/src

# edit `.zshrc`

plugins=([...] zsh-autosuggestions zsh-syntax-highlighting)

# Configure powerlevel10k
p10k configure

```
