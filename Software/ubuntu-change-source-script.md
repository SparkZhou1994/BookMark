# change source for 22.04.4
## backup
```
sudo cp /etc/apt/sources.list /etc/apt/sources.list.backup
```
## change
gedit sources.list
```
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-proposed main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
```
## update
```
sudo apt-get update
```
# git
```
sudo apt-get update
sudo apt-get install git
```
使用ssh来拉取项目
# chrome
https://mirror.cs.uchicago.edu/google-chrome/pool/main/g/google-chrome-stable/
https://www.slimjet.com/chrome/google-chrome-old-version.php
version 104.0.5112.102
# startup
search Applications named Startup Applications
add your command
# snapd
sudo apt update
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys XXXXXXXXXXXXXXX
sudo apt install snapd
sudo snap set system proxy.http="http://127.0.0.1:xxx"
sudo snap set system proxy.https="http://127.0.0.1:xxx"
# vscode
sudo snap install code --classic
