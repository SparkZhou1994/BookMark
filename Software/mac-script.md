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
# chrome
version 116.0.5845.187
# vscode 
version Version: 1.85.2
https://update.code.visualstudio.com/1.85.2/darwin/stable
# window management
rectangle version 0.73
https://github.com/rxhanson/Rectangle/releases
# spotify
version
Spotify for macOS (Intel)
1.2.20.1216.ge7a7b92f
# docker desktop
version 4.6.1 released 2022-03-22
https://desktop.docker.com/mac/main/amd64/76265/Docker.dmg
# f.lux
version 42.2
# Mac Mouse Fix
version 2.2.5
# INNA 
version 1.3.5
# ollama
## installation
docker pull ollama/ollama
docker run -d -v /Users/sparkzhou/Documents/ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
## Usage
### Command-Line
docker container ls
docker exec -it ollama /bin/bash
ollama run model(1.5B)
### continue
apiBase: http://localhost:11434
# python
version 3.10
# xcode
11.3.1