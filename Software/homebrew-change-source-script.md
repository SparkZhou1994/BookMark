# 更换为清华源
## 更换brew.git
```
cd "$(brew --repo)"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git
```
## 更换核心软件仓库（homebrew-core.git)
```
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
```
## 重置Homebrew cask软件仓库
```
cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
git remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
```
## 应用生效
```
brew update
```
## 更换Home Bottles源
```
// 长期更换：
echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bash_profile
source ~/.bash_profile
```
# 重置默认源
## 重置brew.git
```
cd "$(brew --repo)"
git remote set-url origin https://github.com/Homebrew/brew.git
```
## 重置核心软件仓库
```
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
git remote set-url origin https://github.com/Homebrew/homebrew-core.git
```
## 重置Homebrew cask软件仓库
```
cd "$(brew --repo)"/Library/Taps/homebrew/homebrew-cask
git remote set-url origin https://github.com/Homebrew/homebrew-cask.git
```
## 应用生效
```
brew update
```
## 重置Homebrew Bottles源
注释掉bash配置文件里的有关Homebrew Bottles即可恢复官方源。 重启bash或让bash重读配置文件。