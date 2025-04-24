# 安装及配置vim插件
在命令行执行
defaults write com.microsoft.VSCode ApplePressAndHoldEnabled -bool false

settings.json
```
{
  "files.autoSave": "afterDelay",
  "files.autoGuessEncoding": true,
  "workbench.list.smoothScrolling": true,
  "editor.cursorSmoothCaretAnimation": "on",
  "editor.smoothScrolling": true,
  "editor.cursorBlinking": "smooth",
  "editor.mouseWheelZoom": true,
  "editor.formatOnPaste": true,
  "editor.formatOnType": true,
  "editor.formatOnSave": true,
  "editor.wordWrap": "on",
  "editor.guides.bracketPairs": true,
  "editor.bracketPairColorization.enabled": true,
  "editor.suggest.snippetsPreventQuickSuggestions": false,
  "editor.acceptSuggestionOnEnter": "smart",
  "editor.suggestSelection": "recentlyUsed",
  "window.dialogStyle": "custom",
  "debug.showBreakpointsInOverviewRuler": true,

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

  "// To improve performance",
  "extensions.experimental.affinity": {
    "vscodevim.vim": 1
  }
}
```
# 安装及配置vim插件
// TODO


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
