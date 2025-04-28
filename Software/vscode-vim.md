# 安装及配置vim插件
在命令行执行
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false

settings.json
```
{
  // defalut
  "editor.acceptSuggestionOnEnter": "smart",
  "editor.bracketPairColorization.enabled": true,
  "editor.cursorBlinking": "smooth",
  "editor.cursorSmoothCaretAnimation": "on",
  "editor.formatOnPaste": true,
  "editor.formatOnSave": true,
  "editor.formatOnType": true,
  "editor.fontSize": 18,
  "editor.guides.bracketPairs": true,
  "editor.mouseWheelZoom": true,
  "editor.smoothScrolling": true,
  "editor.suggestSelection": "recentlyUsed",
  "editor.suggest.snippetsPreventQuickSuggestions": false,
  "editor.wordWrap": "on",

  "debug.showBreakpointsInOverviewRuler": true,

  "files.autoGuessEncoding": true,
  "files.autoSave": "afterDelay",
  
  "git.openRepositoryInParentFolders": "always",

  "update.mode": "none",

  "window.dialogStyle": "custom",
  "workbench.colorTheme": "Default Dark Modern",
  "workbench.list.smoothScrolling": true,

  // vim
  "vim.easymotion": true,
  "vim.incsearch": true,
  "vim.useSystemClipboard": true,
  "vim.useCtrlKeys": true,
  "vim.hlsearch": true,
  "vim.insertModeKeyBindings": [
    {
      "before": ["j", "j"],
      "after": ["<Esc>"]
    }
  ],
  "vim.normalModeKeyBindingsNonRecursive": [
    {
      "before": ["<leader>", "d"],
      "after": ["d", "d"]
    },
    {
      "before": ["<C-n>"],
      "commands": [":nohl"]
    },
    {
      "before": ["K"],
      "commands": ["lineBreakInsert"],
      "silent": true
    }
  ],
  "vim.leader": "<space>",
  "vim.handleKeys": {
    "<C-a>": false,
    "<C-f>": false
  },

  // To improve performance
  "extensions.experimental.affinity": {
    "vscodevim.vim": 1
  }
}
```
# vim模式
## 普通模式 --NORMAL--
### NORMAL -〉 INSERT
i 在光标前插入 insert
I 在行首插入 append
a 在光标后插入
A 在行尾插入
o 在下一行插入 one line
O 在上一行插入
### NORMAL -〉 VISUAL
v
### NORMAL -〉 命令模式
：

## 插入模式 --INSERT--
### INSERT -〉 NORMAL
ESC jj / CapsLock

## 可视模式 --VISUAL--
可以选中文字
### VISUAL -〉 NORMAL
ESC v

## 命令模式 ：
### 命令模式 -〉 NORMAL
ESC

# 光标移动
## 普通模式
### 字母为单位 
记忆口诀 h在最左边 就是左，l在最右边 就是右
h 左
j 下
k 上
l 右
### 单词为单位 
w 跳到下一个单词开头 forward
b 跳到本单词或上一个单词开头 begin
e 跳到本单词或下一个单词结尾 end
ge 跳到上一个单词结尾
### 行为单位
0 跳到行首
^ 跳到从行首开始第一个非空字符 正则时^表示开头
$ 跳到行尾 正则时$表示结尾
gg 跳到第一行
G 跳到最后一行
### 查找 find
f{char} 光标跳到下个{char}所在位置
F{char} 反向移动到上一个{char}所在位置
t{char} 光标跳到下一个{char}的前一个字符的位置
T{char} 光标反向移动到上个{char}的后一个字符的位置
; 重复上次的字符查找操作
, 反向查找上次的查找命令
# 动作 motion
## 普通模式
### i(inner) 和 a(around) 区别
|command|raw text|i|a|
|:-:|:-:|:-:|:-:|
|x"|"foo"|foo|"foo"|
|xw| foo |foo| foo|
|x(|(foo)|foo|(foo)|
### 动作
iw / aw word
i( / a( 或 ib / ab bracket
i{ / a{ 或 iB / aB
i" / a"
i' / a'
i` / a`
i< / a<
i[ / a[
it / at tag
is / as sentence
ip / ap paragraph

# 插件 
## theme
houston 
one dark pro
material icon theme
carbon product icons
Bracket Pair Colorizer （彩色的括号）
Highlight Matching Tag （高亮对应HTML标签 & 表示对应括号，高效）
indent-rainbow 代码缩进

## perf
mysql
rainbow csv
bookmarks
project manager
Auto Rename Tag （自动匹配HMTL标签）
drawio 绘图
figma for vs code 设计
remote ssh
dev containers
diff
Path Intellisense （智能路径提示）
docker
DotENV 配置临时环境变量
git graph
MIT license generator
todo tree
continue
cline
github copilot
any-rule 正则表达式
import cost 计算包大小
error lens
image preview
GBK to UTF8
Hex editor
doxygen documentation generator
hungry delete
codeSnap
code Runner

## test
rest client
thunder clinet

## lint
eslint
stylelint （css/sass/less语法检查）

## css
css peek

## deon

## Egg
Egg.js dev tools （NodeJs中 EggJs框架，方便）

# Go

## markdown
markdown preview enhanced
markdown pdf

## nuxt
nuxtr

## python
jupyter notebook
pylance
autopep8

## React
Prettier （格式化插件，比Beautify好）

## twind
twind intellisense

## windi
windiCSS intellisense

## typescript
Pretty typeScript Errors
typescript import sorter

## Vue
Vetur （Vue必备，提示的嘛，方便）
Live Server （代码保存后，浏览器自动更新）
typescript vue plugin volar
vue language features volar
