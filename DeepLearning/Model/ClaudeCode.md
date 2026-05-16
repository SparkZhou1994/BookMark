# 快速入门与配置
## 课程介绍与安装
### install
npm install -g @anthropic-ai/claude-code
### .claude/settings.json
```
{
  "attribution": {
    "commit": "Co-Authored-By: Claude Sonnet <noreply@anthropic.com>",
    "pr": "Generated with Claude Code"
  },
  "env": {
    "HTTP_PROXY": "http://127.0.0.1:7890",
    "HTTPS_PROXY": "http://127.0.0.1:7890",
    "ANTHROPIC_AUTH_TOKEN": "sk-",
    "ANTHROPIC_BASE_URL": "https://api.deepseek.com/anthropic",
    "ANTHROPIC_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_OPUS_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_SONNET_MODEL": "deepseek-chat",
    "ANTHROPIC_DEFAULT_HAIKU_MODEL": "deepseek-chat",
    "CLAUDE_CODE_SUBAGENT_MODEL": "deepseek-chat",
    "CLAUDE_CODE_MAX_OUTPUT_TOKENS": "32000",
    "CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC": "1"
  },
  "permissions": {
    "defaultMode": "plan",
    "allow": [],
    "deny": []
  },
  "enabledPlugins": {
    "claude-mem@thedotmack": true,
    "superpowers@claude-plugins-official": true
  },
  "alwaysThinkingEnabled": false
}
```
### 环境变量(没用需要登陆)
```
setx ANTHROPIC_API_KEY "sk-xxxx"
setx ANTHROPIC_BASE_URL "http://127.0.0.1:3456"
setx CLAUDE_CODE_GIT_BASH_PATH "C:\Program Files\Git\bin\bash.exe"
```
### ccr(config.json)
```
{
  "LOG": false,
  "LOG_LEVEL": "debug",
  "CLAUDE_PATH": "",
  "HOST": "127.0.0.1",
  "PORT": 3456,
  "APIKEY": "sk-123456789",
  "API_TIMEOUT_MS": "600000",
  "PROXY_URL": "http://127.0.0.1:7890",
  "transformers": [],
  "Providers": [
    {
      "name": "openrouter",
      "api_base_url": "https://openrouter.ai/api/v1/chat/completions",
      "api_key": "sk-xxx",
      "models": [
        "google/gemini-2.5-pro-preview",
        "anthropic/claude-sonnet-4",
        "anthropic/claude-3.5-sonnet",
        "anthropic/claude-3.7-sonnet:thinking"
      ],
      "transformer": {
        "use": ["openrouter"]
      }
    },
    {
      "name": "deepseek",
      "api_base_url": "https://api.deepseek.com/chat/completions",
      "api_key": "sk-xxx",
      "models": ["deepseek-chat", "deepseek-reasoner"],
      "transformer": {
        "use": ["deepseek"],
        "deepseek-chat": {
          "use": ["tooluse"]
        }
      }
    },
    {
      "name": "ollama",
      "api_base_url": "http://localhost:11434/v1/chat/completions",
      "api_key": "ollama",
      "models": [
        "qwen2.5-coder:1.5b"
      ]
    },
    {
      "name": "any-router",
      "api_base_url": "https://anyrouter.top",
      "api_key": "sk-123456789",
      "models": [
        "claude-4-sonnet-20250514",
        "claude-opus-4-1-20250805"
      ],
      "transformer": { "use": ["anthropic"] }
    },
    {
      "name": "gemini",
      "api_base_url": "https://generativelanguage.googleapis.com/v1beta/models/",
      "api_key": "sk-xxx",
      "models": ["gemini-2.5-flash", "gemini-2.5-pro"],
      "transformer": {
        "use": ["gemini"]
      }
    },
    {
      "name": "volcengine",
      "api_base_url": "https://ark.cn-beijing.volces.com/api/v3/chat/completions",
      "api_key": "sk-xxx",
      "models": ["deepseek-v3-250324", "deepseek-r1-250528"],
      "transformer": {
        "use": ["deepseek"]
      }
    },
    {
      "name": "modelscope",
      "api_base_url": "https://api-inference.modelscope.cn/v1/chat/completions",
      "api_key": "",
      "models": ["Qwen/Qwen3-Coder-480B-A35B-Instruct", "Qwen/Qwen3-235B-A22B-Thinking-2507"],
      "transformer": {
        "use": [
          [
            "maxtoken",
            {
              "max_tokens": 65536
            }
          ],
          "enhancetool"
        ],
        "Qwen/Qwen3-235B-A22B-Thinking-2507": {
          "use": ["reasoning"]
        }
      }
    },
    {
      "name": "dashscope",
      "api_base_url": "https://dashscope.aliyuncs.com/compatible-mode/v1/chat/completions",
      "api_key": "",
      "models": ["qwen3-coder-plus"],
      "transformer": {
        "use": [
          [
            "maxtoken",
            {
              "max_tokens": 65536
            }
          ],
          "enhancetool"
        ]
      }
    },
    {
      "name": "aihubmix",
      "api_base_url": "https://aihubmix.com/v1/chat/completions",
      "api_key": "sk-",
      "models": [
        "Z/glm-4.5",
        "claude-opus-4-20250514",
        "gemini-2.5-pro"
      ]
    }
  ],
  "StatusLine": {
    "enabled": false,
    "currentStyle": "default",
    "default": {
      "modules": []
    },
    "powerline": {
      "modules": []
    }
  },
  "Router": {
    "default": "ollama,qwen2.5-coder:1.5b",
    "background": "ollama,qwen2.5-coder:1.5b",
    "think": "any-router,claude-sonnet-4-20250514",
    "longContext": "any-router,claude-sonnet-4-20250514",
    "longContextThreshold": 60000,
    "webSearch": "ollama,qwen2.5-coder:1.5b",
    "image": ""
  },
  "CUSTOM_ROUTER_PATH": ""
}
```
### continue
```
name: Local Config
version: 1.0.0
schema: v1
models: 
  - name: qwen2.5-coder:1.5b
    provider: ollama
    model: qwen2.5-coder:1.5b 
    apiBase: http://127.0.0.1:11434
    roles:
      - chat
      - edit
      - apply
      - rerank
      - autocomplete
```
## 基础操作：命令与配置是起点
|命令|解释|使用场景|
|:-:|:-:|:-:|
|/clear|清空上下文|如果需要重新开始，或者感觉AI已经无法解决问题|
|/compact|压缩对话|重开对话，但不希望丢掉之前的记忆|
|/cost|花费||
|/model|切换模型||
|/status|状态|CC的状态|
|/doctor|检测|CC的安装状态|
## 核心模式：按场景切换，效果拉满
### 自动编辑模式：面确认批量操作(Shift+Tab一次)
### Plan模式：前期规划神器(Shift+Tab两次)
### Yolo模式：全权限放手干(claude --dangerously-skip-permissions)
或者在.claude/settings.json添加
```
{
  "permissions": {
    "defaultMode": "bypassPermissions"  // 永久跳过所有权限提示
  }
}
```
## CLAUDE.md：全局记忆的核心
### 工作流
- 创建初始化CLAUDE.md
- 对话长度接近溢出，运行/compact续命
- 达到里程碑时要求CC根据进度更新CLAUDE.md
- 循环直到结束

### 注意事项
- 文件不要太长
- 会话可以简介省事
- 文件中适合放提醒事项，比如“要求CC每次宣布成功时都要带上证据文件链接”，以及“代理服务端口是9890”。然后会话时，可以要求“查询claude.md相关部分”。
- 官方的“#”进文档据GPT说有个bug不稳定

### 举例
我想设计一个xxx，基于java语言开发，请帮我单独创建一个项目文件夹：xxx，首先生成项目需求和技术方案到plan.md文件中，然后将一些java项目代码生成规范输出到CLAUDE.md文件中，之后执行plan.md中计划去写代码的时候，需要参考CLAUDE.md文件中提到的规范。

开始按照plan.md中的计划进行开发，在编写代码时参考CLAUDE.md中的规范来确保代码质量和一致性
## 会话管理：避免失控，高效推进
### 随时暂停与回滚
在工作中按Esc可暂停当前操作
按两次Esc回退到历史对话节点(无redo功能，回退前确认)
代码结果不满意，直接说“回滚到上次的代码”
### 应对历史溢出
当会话提示“Context left until auto-compact”，执行/compact续命，避免对话中断。
### 恢复与查看历史
用claude -c 进入上次对话
用claude -r 选择历史会话恢复，适合中途退出后 继续工作
## 资源监控与批量任务：把控节奏不浪费
### 实时监控token用量
想知道每天消耗多少资源时，运行命令
```
npx ccusage@latest
// 或者
npx ccusage blocks --live
```
### 批量任务高效处理
需要执行几十个重复任务用脚本式用法
1. 把任务按行写入TASK.md(一行一个任务)
2. 执行命令
```
cat TASK.md | while IFS= read -r line; do echo $line; claude -p "$line" --debug; done
```
建议加上timeout，同时用--allowedTools "Edit"限制权限，避免意外操作。不要并发执行，否则会触发限额封禁。
## 避坑与进阶：让Claude更“听话”
### 给足自由，也要做好防护
- 开启auto-accept edits(Shift+Tab切换)
- 执行Bash命令时，用Docker隔离环境，或者用btrfs文件系统做快照
- 避免在会话目录存放 ssh key等敏感信息，防止跨机操作风险
### 严防“虚假成功”
在CLAUDE.md中加入规则“每次宣称成功必须附证据文件链接”
### 核心是按需切换模式、用好全局记忆，加上适当的监控和防护
# 官方最佳实践
## 上下文规则配置
### 自定义您的设置
#### 创建CLAUDE.md文件 （或者在命令行模式输入# 后粘贴CLAUDE.md文件内容）
Claude在开始对话时自动将其拉入上下文。使其成为记录以下内容的理想场所：
- 常用bash命令
- 核心文件和实用函数
- 代码风格指南
- 测试说明
- 代码库规范（分支命名、合并与变基）
- 开发环境设置（pyenv使用、哪些编译器有效）
- 项目特有的任何意外行为或警告
- 希望Claude记住的其他信息
##### 例子
```
# Bash命令
- npm run build：构建项目
- npm run typecheck： 运行类型检查器

# 代码风格
- 使用ES模块（import / export）语法，而不是CommonJS（require）
- 尽可能使用解构导入（例如 import { foo } form 'bar'）

# 工作流程
- 在完成一系列代码更改后务必进行类型检查
- 为了性能考虑，优先运行单个测试，而不是整个测试套件
```
#### 调优您的CLAUDE.md文件
在命令行模式输入# 后追加CLAUDE.md内容
#### 管理Claude的允许的工具列表
- 使用/permissions命令想允许列表添加或移除工具。
- 编辑.claude/settings.json或~/.claude.json
- 使用--allowedTools CLI标志会话特定的权限设置。
如Edit、Bash（git commit：*）
```
{
  "permissions": {
    "allow": [
      "Bash(ls:*)",
      "WebFetch",
      "Bash(git commit:*)"
    ],
    "deny": [],
    "ask": []
  }
}
```
#### 如果使用GitHub，安装gh CLI
如果没有gh时，可通过GitHub API或者MCP服务器来安装
也可通过winget install --id GitHub.cli
linux可以通过 sudo apt install gh
gh登陆授权：gh auth login 获取github token
用自然语言告诉ClaudeCode用gh命令，提交代码
### 为Claude提供更多工具
#### 与bash工具一起使用Claude
- 告诉Claude工具名称和使用示例
- 告诉Claude运行--help来查看工具文档
- 在CLAUDE.md中记录常用工具

#### 与MCP一起使用Claude
- 在项目配置中 --scope local
- 在全局配置中 --scope user
- 在检入的.mcp.json文件中，如Puppeteer和Sentry

使用MCP时，使用claude --mcp-debug标识启动Claude可以帮助识别配置问题。
##### 管理MCP服务器
```
# 列出所有配置的服务器
claude mcp list
claude mcp add --transport sse github-server https://api.github.com/mcp
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp
# 获取特定服务器的详细信息
claude mcp get github

# 删除服务器
claude mcp remove github

# 在Claude中 检查服务器状态
/mcp
```
#### 使用自定义斜杠命令
将提示模板存储在.claude/commands文件夹下，文件名为命令名的md文件，如fix-github-issue.md，使用/project:fix-github-issue 1234来修复问题#1234
```
请分析并修复GitHub问题：$ARGUMENTS。

按照这些步骤：

1. 使用 `gh issue view` 获取问题详情
2. 理解问题中描述的问题
3. 搜索代码库中的相关文件
4. 实施必要的更改来修复问题
5. 编写并运行测试来验证修复
6. 确保代码通过代码检查和类型检查
7. 创建描述性的提交信息
8. 推送并创建PR

记住对所有GitHub相关任务使用GitHub CLI(`gh`)。
```

### 尝试常见工作流程
#### 探索、规划、编码、提交
- 要求其阅读相关文件、图像或URL，明确告诉它暂时不要编写任何代码。对于复杂问题，告诉它使用子智能体来验证细节或调查它可能有的特定问题，特别时在对话或任务的早期，往往能在不损失效率的情况下保持上下文可用性。
- 要求Claude制定解决特定问题的计划。建议使用"think"触发扩展思维模式。think < think hard < think harder < ultrathink。可以让Claude创建一个文档，记录其计划，如果不符合要求可以重复第二步。
- 要求Claude在代码中实施其解决方案，并验证其方案的合理性。
- 要求Claude提交结果并创建拉取请求，让Claude更新任何README

#### 编写测试、提交、编码、迭代、提交(TDD)
- 要求Claude基于预期的输入、输出对编写测试。
- 告诉Claude运行测试并确认它们失败。
- 在您满意时把测试提交到git
- 要求Claude编写通过测试的代码，可通过子智能体来验证是否过度拟合测试
- 在通过测试后将代码提交git

#### 编写代码、截图结果、迭代 
- 为Claude提供拍摄游览器截图的方法，如Puppeteer MCP、IOS模拟器MCP或手动复制给Claude
- 要求Claude在代码中实现设计，拍摄结果，直到迭代到与结果匹配
- 提交代码

#### 安全YOLO模式
使用claude --dangerously-skip-permissions绕过所有权限检查，在没有互联网访问的容器中使用。可以使用Docker Dev Containers的参考实现。

### 优化您的工作流程
#### 在指令中要具体，可提供图像、提供希望Claude查看或处理的文件、提供网址(在/permissions将域名添加到您的允许列表中)
#### 尽早且经常地修正方向
- 要求Claude在编码前制定计划。
- 按Escape中断，并保留上下文以便3纠偏或扩展指令。
- 双击Escape跳回历史，编辑之前的提示词，并探索不同的方向。
- 要求Claude撤销更改，通常和2结合使用
 
#### 使用/clear保持上下文聚焦
#### 为复杂工作流程使用检查清单和草稿板
- 告诉Claude运行代码检查命令并将所有结果错误(包括文件名和行号)写入Markdown检查清单
- 告诉Claude逐一解决每个问题，修复并验证后勾选并移至下一个

#### 向Claude传递数据的方式
- 复制粘贴
- 管道输入如 cat foo.txt | claude，特别适用于日志，CSV和大数据
- 告诉Claude通过bash命令、MCP工具或自定义斜杠命令拉取数据
- 告诉Claude读取文件或获取URLs

### 使用无头模式自动化您的基础设施
claude -p是无头模式（非交互模式），这种模式适合自动化和CI/CD流程中使用。
claude -p "分析代码，使用中文输出" --output-format json
claude -p "分析代码，使用中文输出" --output-format stream-json --verbose
#### 进行问题分类并分配适当的标签
#### 使用Claude作为代码检查器，主观代码审查，如识别拼写错误、过时注释、误导性函数或变量名
### 多Claude协作
#### 一个Claude来编写代码，另一个来进行验证
#### 拥有代码库的多个checkout
- 在单独的文件夹中创建3-4个git checkout
- 在单独的终端标签中打开每个文件夹
- 在每个文件夹中启动Claude执行不同的任务
- 轮流检查各个Claude进度并批准/拒绝权限请求

#### 使用git工作树
在多个独立任务中表现出色，为多个检出提供了轻量级的替代方案。Git工作树允许您从同一代码库将多个分支检出到单独的目录中。每个工作树都有自己的工作目录和隔离的文件，同时共享相同的Git历史和reflog。
使用git工作树使您能够在项目的不同部分同时运行多个Claude会话，每个都专注与自己的独立任务。例如，您可能让一个Claude重构您的身份验证系统，而另一个构建完成不相关的数据可视化组件。由于任务不重叠，每个Claude都可以全速工作，无需等待其他的更改或处理合并冲突。
- 创建工作树 git worktree add ../project-feature-a feature-a
- 在每个工作树中启动Claude
- 根据需要创建额外的工作树

#### 使用自定义工具的无头模式
使用无头模式有两种主要模式
##### 扇出： 处理大型迁移或分析
###### 编写脚本生成任务列表
###### 循环遍历任务 claude -p "将xxx完成后返回字符串OK，失败返回FAIL" --allowedTools Edit Bash(git commit:*)
###### 运行脚本多次并完善提示以获取所需的结果
##### 管道
使用--verbose标识调试有帮助，在生产中建议关闭
# 企业级应用实战
## 案例一：效率提升3-5倍的大型项目改造
### 需求分析
### 代码修改
- 实时提供API兼容性检查
- 智能识别潜在的破坏性变更
### 人工介入
- 代码审查
- 测试功能
## 案例二：会议中的高效编码
### 会议前
#### 准备功能清单，为每个功能编写简单的需求描述
#### 设置好测试环境
### 会议中
#### 按照UI设计图是实现功能
## 案例三：Playwright MCP增强Bug修复
### 安装
npm install -g @playwright/test
npm install -g @executeautomation/playwright-mcp-server
npx playwright install
### 配置mcp
claude mcp add-json playwright '{"command": "cmd", "args": ["/c"， "npx", "@executeautomation/playwright-mcp-server "]}'
### 检查测试
claude mcp list
claude --print "测试playwright" --debug | findstr "connected"
### 使用
启动当前项目下index.html企业官网，然后使用Playwright mcp工具获取企业官网首页内容，用中文沟通
## 案例四：快速理解和改造开源项目
### 安全扫描
### 架构分析
- 可视化的架构图
- 核心模块功能说明
- 关键代码路径标注
- 扩展点识别
## 案例五：多任务并行开发
### 左上 前端功能开发
### 右上 后端API开发
### 左下 性能优化
### 右下 文档编写
## 11个技巧让Claude Code成功率翻倍
### 应对AI幻觉：果断重启
- 立即停止当前对话 /clear
- 回滚到上一个稳定版本 git reset --hard
- 总结已尝试的错误方案，形成“负面清单”
- 重新开始，明确告知AI避免这些错误方向

### 版本控制是生命线
- 原子化提交：每完成一个小功能就提交
- 有意义的提交信息：描述清楚做了扫描改动
- 分支策略：为每个新功能创建独立分支
- 标签管理：为重要版本打标签
```
git checkout -b feature/user-auth

git add .
git commit -m "feat: 添加用户登录接口"

git tag -a v1.0.0 -m "完成用户认证功能"

git reset --hard HEAD^1
```
### 善用Plan Mode 规划先行
### 需求文档决定成功率
Product Specs(产品规格说明书)，将文档放入docs目录下
#### 功能规格
- 详细的功能描述
- 用户故事和使用场景
- 输入输出定义
- 边界条件处理
#### 技术规格
- 技术架构设计
- 数据模型定义
- API接口规范
- 性能指标要求
#### 实施细节
- 开发步骤分解
- 测试策略
- 部署方案
- 回滚计划
### 建立项目规则记忆
在CLAUDE.md文件中
```
# 项目开发规范
## 代码规范
- 使用ESLint + Prettier
- 函数采用小驼峰命名
- 组件采用大驼峰命名
- 常量使用全大写下划线分割

## Git规范
- 使用conventional commits
- feat: 新功能
- fix：修复bug
- docs：文档更新
- refactor: 代码重构

## 开发原则
- 单一职责原则
- 每个PR只解决一个问题
- 代码必须有单元测试
- 注释用中文，代码用英文

## 个人偏好
- 优先使用函数式组件
- 状态管理使用Zustand
- 样式使用CSS Modules
- 避免使用any类型

## 语言规范
- 所有对话和文档都使用中文
- 注释使用中文
- 错误提示使用中文
- 文档使用中文Markdown格式
```
#### 项目级配置
project.md
```
# 项目特定规范
## API规范
- RESTful风格
- 使用JWT认证
- 统一错误处理格式

## 数据库规范
- 表名使用复数
- 主键统一命名为id
- 时间字段使用UTC
```
### 全程使用中文交流和文档
### 免授权模式： 提升工作流畅度
### 多用/clear即时清理上下文
清理时机
- 完成一个独立任务后
- 切换到不相关的新任务
- 发现AI开始混淆概念
### 智能的审查工作流
#### 功能验证
- 测试功能是否正常
- 检查是否满足需求
- 验证边界条件
#### AI自审
- 性能优化机会
- 代码重复
- 潜在的bug
- 不符合规范的地方
#### 人工详审
重点关注
- 业务逻辑正确性
- 安全性问题
- 代码可维护性
- 架构合理性
审查检查清单
- 功能是否完整实现
- 是否有明显的性能问题
- 错误处理是否完善
- 是否有安全漏洞
- 代码是否易于理解
- 是否符合项目规范
### 合理设定AI参与度
#### AI擅长领域
- 样板代码生成
- CRUD
- 常见设计模式应用
- 测试用例编写
- 文档生成
- 代码重构
#### 需要人工介入的领域
- 复杂的业务逻辑决策
- UI细节的像素级调整
- 特定的性能优化
- 架构级别的设计决策
- 与外部系统的特殊集成
### 良好架构和命名的重要性
## 安全风险[claude-ignore文档](https://github.com/li-zhixin/claude-ignore/blob/main/README_CN.md)
敏感代码不宜交给AI进行分析
- 许可证验证逻辑
- 防破解机制
- 核心算法实现
- 商业机密代码
## 代码审查的新挑战
## 对开发者的影响





# Claude Code 从 0 到 1 全攻略：MCP / SubAgent / Agent Skill / Hook / 图片 / 上下文处理/ 后台任务
## 复杂任务处理与终端控制
### ! 进入Bash模式
可Ctrl+g进入VsCode进行编辑
/task可以查看后台任务
## 多模态与上下文管理
### 版本回滚
/rewind 或 两下Esc
### 安装 MCP Server
claude mcp add --transport http figma https://mcp.figma.com/mcp
在Figma里Copy link to selection，在Claude里“修改当前页面，使它与figma稿件保持一致， link”

figma-remote-mcp
## 高级功能扩展与定制
### Hook
### Agent Skill
创建一个文件夹skill，适合与上下文关联大的任务
### SubAgent
独立上下文
### Plugin
claude-mem
superpowers

# claude-101
## Meet Claude
### What is Claude
#### Key takeaways
- Claude is built to be helpful, harmless, and honest
- Claude is more than a chatbot
- Claude is designed to be steerable and collaborative
Claude can take direction on personality, tone, and behavior, and customers report that Claude is much less likely to produce harmful outputs, easier to converse with, and more steerable—so you can get your desired output with less effort.
- You can access Claude wherever you work
#### Understanding Claude's capabilities
Here's a few things Claude excels at
- Writing and content creation
Claude can collaborate with you on social media posts, professional emails, and complex reports.
- Research and analysis
Claude helps you explore research angles, compile findings, and analyze data to surface meaningful insights.This is enabled by Claude's large context window, which can ingest 200K+ tokens (about 500 pages of text or more)
- Coding assistance
- Problem-solving and reasoning
Claude Opus 4.7 and Sonnet 4.7 are hybrid models offering two modes: near-instant responses and extended thinking for deeper reasoning. 这种hybrid是推理hybrid，运行策略融合，普通输出+深度思考。还有一种是架构hybrid，结构融合，CNN+Transformer，传统模型+深度学习。
- Learning new things

Get inspired on ways to use Claude in your specific function by exploring our *use-case* gallery on claude.com. For a deeper dive into what AI can (and can't) do, see our *AI Capabilities* course.

#### Ways to access Claude
- Claude.ai 
Here, you can ask questions, brainstorm ideas, create and edit documents, and a lot more. Claude.ai is ideal for conversations, writing assistance, research, analysis, and creating files. This is our focus in this course.
- Claude Code
It is an agentic coding tool that is designed for developers but can be used for all kinds of file manipulation on your desktop.
- Claude and Slack
- Claude for Excel
### Your first conversation with Claude
