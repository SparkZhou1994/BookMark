# docker
## install[docker-compose.yml]
```
services:
  openclaw:
    image: ghcr.io/openclaw/openclaw:latest
    container_name: openclaw
    restart: unless-stopped
    ports:
      - "18789:18789"
    environment:
      - HOME=/home/node
      - HTTP_PROXY=http://172.17.0.1:7890
      - HTTPS_PROXY=http://172.17.0.1:7890
      - http_proxy=http://172.17.0.1:7890
      - https_proxy=http://172.17.0.1:7890
      - NO_PROXY=localhost,127.0.0.1,localaddress,.localdomain.com
      - no_proxy=localhost,127.0.0.1,localaddress,.localdomain.com
    volumes:
      # 默认持久化
      - ~/Documents/Software/OpenClaw/.openclaw:/home/node/.openclaw
      - ~/Documents/Software/OpenClaw/.openclaw/workspace:/home/node/.openclaw/workspace
      # ========== 自定义宿主机挂载 ==========
      # 宿主机路径:容器内路径:权限 ro = readonly rw = read-write
      # - /home/ubuntu/文档:/host/docs:ro
      # /root/.openclaw/config.yaml
      # agents:
      #  defaults:
      #    sandbox:
      #      docker:
      #        binds:
      #          - "/host/docs:/host/docs:ro"
```
### permission
``` bash
id spark
sudo chown -R 1000:1000 /home/spark/Software/OpenClaw/
chown -R node:node /home/node/.openclaw
chmod -R 755 /home/node/.openclaw
```
### onboard
openclaw onboard
#### gateway config[openclaw.json]
```
"bind": "lan",
```
#### permission config[openclaw.json]
```
"tools": {
  "profile": "full"
}
```
#### channels
##### dingtalk
###### install
openclaw plugins install @soimy/dingtalk
###### config
openclaw config
#### model
```
"models": {
  "mode": "merge",
  "providers": {
    "volcengine-plan": {
      "baseUrl": "https://ark.cn-beijing.volces.com/api/coding/v3",
      "apiKey": "",
      "api": "openai-completions",
      "models": [
        {
          "id": "deepseek-v3.2",
          "name": "deepseek-v3.2",
          "contextWindow": 128000,
          "maxTokens": 32000,
          "input": [
            "text"
          ]
        },
        {
          "id": "kimi-k2.5",
          "name": "kimi-k2.5",
          "contextWindow": 256000,
          "maxTokens": 32000,
          "input": [
            "text",
            "image"
          ]
        }
      ]
    }
  }
},
"agents": {
  "defaults": {
    "workspace": "/home/node/.openclaw/workspace",
    "models": {
      "volcengine-plan/deepseek-v3.2": {},
      "volcengine-plan/kimi-k2.5": {}
    },
    "model": {
      "primary": "volcengine-plan/deepseek-v3.2"
    },
    "compaction": {
      "mode": "safeguard"
    }
  }
},
```
# skill
eddygk/skill-vetting
xiucheng/xiucheng-self-improving-agent
matthew77/liang-tavily-search
summarize
fangkelvin/find-skills-skill
superpowers
analsharqy/react-best-practices-2
michaelmonetized/frontend-design-3
steipete/github
guohongbin-git/github-trending-cn
matrixy/agent-browser-clawdbot
# 安全
## 本地网关认证
```
# 生成token并放入 ~/.openclaw/token.txt
openssl rand -hex 32
# 设置环境变量
setx OPENCLAW_GATEWAY_TOKEN "xxxxxxxxxxxxxx"
# ~/.openclaw/openclaw.json
{
  "gateway": {
    "auth": {
      "type": "token",
      "token": "xxxxxxxxxxxxxx"
    }
  }
}
# 可通过openclaw dashboard --no-open，获取带有token的链接
```
