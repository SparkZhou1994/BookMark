# 安装及配置vim插件
在命令行执行
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false

settings.json
```
{
  // defalut
  "debug.showBreakpointsInOverviewRuler": true,
  "diffEditor.renderSideBySide": false,

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
  // To improve performance for vim
  "extensions.experimental.affinity": {
    "vscodevim.vim": 1
  },

  "files.autoGuessEncoding": true,
  "files.autoSave": "afterDelay",
  "files.eol": "\n",
  // Git 
  "git.openRepositoryInParentFolders": "always",
  "git.path": "C:\\Program Files\\Git\\cmd\\git.exe",
  // JDK
  "java.configuration.maven.userSettings": "C:\\Users\\Spark\\Documents\\Software\\.m2\\settings.xml",
  "java.configuration.runtimes": [
    {
      "name": "JavaSE-1.8",
      "path": "C:\\Users\\Spark\\Documents\\Software\\Java\\jdk1.8.0_161",
    },
    {
      "name": "JavaSE-21",
      "path": "C:\\Users\\Spark\\Documents\\Software\\Java\\jdk-21.0.3",
      "default": true
    }
  ],
  "java.errors.incompleteClasspath.severity": "ignore",
  "java.jdt.ls.java.home": "C:\\Users\\Spark\\Documents\\Software\\Java\\jdk-21.0.3",
  // MAVEN
  "maven.executable.path": "C:\\Users\\Spark\\Documents\\Software\\apache-maven-3.6.0\\bin\\mvn.cmd",
  "maven.terminal.customEnv": [
    {
      "environmentVariable": "JAVA_HOME",
      "value": "C:\\Users\\Spark\\Documents\\Software\\Java\\jdk-21.0.3"
    }
  ],
  "maven.terminal.useJavaHome": true,
  // SVN
  "svn.path": "C:\\Program Files\\TortoiseSVN\\bin\\svn.exe",
  
  "update.mode": "none",

  "window.dialogStyle": "custom",
  "workbench.colorTheme": "Default Dark Modern",
  "workbench.list.smoothScrolling": true,

  // vim
  "vim.easymotion": true,
  "vim.handleKeys": {
    "<C-a>": false,
    "<C-f>": false
  },
  "vim.hlsearch": true,
  "vim.incsearch": true,
  "vim.insertModeKeyBindings": [
    {
      "before": ["j", "j"],
      "after": ["<Esc>"]
    }
  ],
  "vim.leader": "<space>",
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
    },
    // tab 相关
    {
      "before": ["t", "h"],
      "commands": [":tabp"]
    },
    {
      "before": ["t", "l"],
      "commands": [":tabn"]
    },
    // 由 editor 跳转 terminal
    {
      "before": ["<leader>", "t"],
      "commands": ["workbench.action.terminal.focus"]
    }
  ],
  "vim.useCtrlKeys": true,
  "vim.useSystemClipboard": true
}
```
# vim模式
## 普通模式 --NORMAL--
### NORMAL -〉 INSERT
i 在光标前插入 insert
I 在行首插入
a 在光标后插入 append
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
加上方向键或者动作motion, 可以选择文字
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
### 在当前行查找 find 
f{char} 光标跳到下个{char}所在位置
F{char} 反向移动到上一个{char}所在位置
t{char} 光标跳到下一个{char}的前一个字符的位置
T{char} 光标反向移动到上个{char}的后一个字符的位置
; 重复上次的字符查找操作
, 反向查找上次的查找命令
### 整文查找
输入/{char}\c
\c 表示大小写不敏感 \C 表示大小写敏感
n 查找下一个 N 表示查找上一个
# 操作符 operator
## 普通模式
d(delete) 删除
c(change) 修改(删除并进入插入模式)
y(yank) 复制
v(visual) 选中并进入可视模式
p(paste)  粘贴
u(undo) 撤销

# 操作符 operator + 动作 motion
## 操作符 operator
## 普通模式
d(delete) 删除
c(change) 修改(删除并进入插入模式)
y(yank) 复制
v(visual) 选中并进入可视模式
p(paste)  粘贴
u(undo) 撤销
## 动作 motion
### 普通模式
#### i(inner) 和 a(around) 区别
|command|raw text|i|a|
|:-:|:-:|:-:|:-:|
|x"|"foo"|foo|"foo"|
|xw| foo |foo| foo|
|x(|(foo)|foo|(foo)|
#### 动作
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
## 实操
### 删除两行
2dd
### 删除到、修改到、复制到s
dfs、cfs、yfs, p
### 删除到、修改到、复制到开头 ^ / 结尾 $
d^/$、c^/$、y^/$, p
### 删除、修改、复制整个文件
die(entire)、cie、yie, p
### 删除、修改、复制标签
dit、cit、yie, p
# 大小写转换
~ 将光标下的字母改变大小写
3~  将光标位置开始的3个字母改变其大小写
g~~ 改变当前行字母的大小写
gUU 将当前行的字母改成大写
guu 将当前行的字母改成小写
gUaw(gUiw)  将光标下的单词改成大写
guaw(guiw)  将光标下的单词改成小写
# Tips
gd 查看函数定义go to defination
gh 查看函数参数hover
gt 切换标签页tab
command 0 切到侧边栏 空格可以打开文件 l将光标返回
分屏时 command 1，2来相互切换
command shift p 输入 focus terminal
# easymotion
空格空格s{char} 快速查找字符
空格空格w{char} 快速查找单词开始的字符
空格空格e{char} 快速查找单词结束的字符
# vim-surround
ds{char}  删除成对字符
cs{char}{char用右成对字符}  替换成对字符
ys motion{char} 添加成对字符
# 多光标模式
command d或者gb
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
Partial Diff
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

# Continue For VSCode
```
name: Local Assistant
version: 1.0.0
schema: v1
models:
  - name: DeepSeek V3 0324 (free)
    provider: openrouter
    model: deepseek/deepseek-chat-v3-0324:free
    apiBase: https://openrouter.ai/api/v1
    apiKey: 26@xx.com
```