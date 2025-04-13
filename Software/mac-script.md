# ~/.bash_profile
```
export http_proxy=socks5://127.0.0.1:7890
export https_proxy=socks5://127.0.0.1:7890

eval "$(/usr/local/bin/brew shellenv)"
```
# git from brew
download openssl from homebrew-core/Formula
delete test
brew install --build-from-source --formula /path/to/openssl@3.rb

## change user name
```
git config --global user.name "SparkZhou"
git config --global user.email "zhoujuhui@xxx.com"
ssh-keygen -t rsa -C "zhoujuhui@xx.com"
```
cd ~/.ssh
cat id_ed25519.pub
