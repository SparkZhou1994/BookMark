# change user name
```
git config --global user.name "SparkZhou"
git config --global user.email "zhoujuhui@xxx.com"
```
# SSH(优先使用ed25519算法，如果机器太老再使用rsa)
```
ssh-keygen -t ed25519-sk -C "your_email@example.com" // for mac
ssh-keygen -t ed25519 -C "your_email@example.com" // for ubuntu
cd ~/.ssh
cat id_ed25519.pub
```
## add SSH on the github website

# for brew
```
git config --global http.version HTTP/1.1
git config --global http.postBuffer 524288000
git config --global http.lowSpeedLimit 0
git config --global http.lowSpeedTime 999999
```
## resetting
```
git config --global --unset http.version
git config --global --unset http.postBuffer
git config --global --unset http.lowSpeedLimit

git config --global --unset http.lowSpeedTime
```
## change directory into the local clone
```
git remote -v
git remote set-url origin git@github.com:username/your-repository.git
```
### commit the code

# remvoe .DS_Store
find . -name .DS_Store -print0 | xargs -0 git rm -f --ignore-unmatch

# gitignore
```
#mac
**/.DS_Store
Thumbs.db
#ubuntu
*.*~
#hidden file
.*
```
