# NVM
## install nvm
```
brew install nvm
```
## add to PATH
```
mkdir ~/.nvm
vi ~/.bash_profile

# NVM
export NVM_DIR="$HOME/.nvm"
  [ -s "/usr/local/opt/nvm/nvm.sh" ] && \. "/usr/local/opt/nvm/nvm.sh"  # This loads nvm
  [ -s "/usr/local/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/usr/local/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```
# NODE
## install node
```
nvm ls-remote
nvm install v10.24.1
nvm use v10.24.1
```
## change source
```
npm cache clean --force
npm config set registry https://registry.npmmirror.com
```
