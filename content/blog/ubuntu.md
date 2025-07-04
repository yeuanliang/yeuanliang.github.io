+++
title = "Ubuntu Server Setup"
date = "2024-09-30T16:17:54+08:00"
description = "Ubuntu Server Setup"
tags = ["cs","journal"]
+++

## New Sudo User Ubuntu

* `adduser ubuntu`
* `usermod -aG sudo ubuntu`
* `su - ubuntu`

## Rename Host

* `sudo nano /etc/hostname`
* `sudo nano /etc/hosts`
* `sudo reboot`

## Change Password

* `sudo passwd username`

## Change Root Password

* `sudo -i`
* `passwd`

## Upgrade to new LTS

* `sudo apt update && sudo apt upgrade`
* `sudo do-release-upgrade`
* `sudo apt install --install-recommends linux-generic-hwe-{lts version}`

## 强制SSH使用密钥认证
```
sudo vim /etc/ssh/sshd_config

PermitRootLogin no
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
# Install terminal tools 
sudo apt install git vim fontconfig curl zsh fzf ripgrep bat eza zoxide plocate btop fd-find tldr

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

## mise

```
# Install
curl https://mise.run | sh

# edit .zshrc
export PATH=$HOME/.local/bin:$PATH
plugins=([...] mise)

# Install ruby
sudo apt install build-essential rustc libssl-dev libyaml-dev zlib1g-dev libgmp-dev
mise use -g ruby
```

## [clash for linux](https://github.com/wanhebin/clash-for-linux.git)

打开9090端口：`ufw allow 9090`  
打开界面：浏览器访问`http://<ip>:9090/ui`，在`API Base URL`中输入`http://<ip>:9090`，在`Secret(optional)`中输入启动成功后输出的Secret

## chromedriver

`sudo apt install chromium-chromedriver`

## selenium ssl proxy

```
options = Selenium::WebDriver::Options.chrome(args: ['--headless=new'])
options.proxy = Selenium::WebDriver::Proxy.new(ssl:'PROXY_URL')
```

## [acme.sh](https://github.com/acmesh-official/acme.sh) 自动签发Let’s Encrypt的免费数字证书

安装acme.sh：`curl https://get.acme.sh | sh -s email=my@example.com`

dnsapi：`https://github.com/acmesh-official/acme.sh/wiki/dnsapi`

issue a cert：`acme.sh  --issue -d example.com  -d '*.example.com'  --dns dns_cf`

install the cert: 
```
acme.sh --install-cert -d example.com \
--key-file       /path/to/keyfile/in/nginx/key.pem  \
--fullchain-file /path/to/fullchain/nginx/cert.pem \
--reloadcmd     "service nginx force-reload"
```
