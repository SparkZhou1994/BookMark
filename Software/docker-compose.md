# Docker & Docker Compose 安装与使用完整指南
## 系统环境
- 操作系统：Xubuntu / Ubuntu 系列
- 适用版本：22.04 (Jammy) / 24.04 (Noble)
## 一、完整安装步骤
### 1. 卸载旧版本（可选，推荐执行）
```bash
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do 
  sudo apt-get remove -y $pkg
done
```
### 2. 安装基础依赖
```bash
sudo apt-get update
sudo apt-get install -y ca-certificates curl gnupg lsb-release
```
### 3. 添加 Docker GPG 密钥
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```
### 4. 添加 Docker 软件源（无换行问题版）
```bash
echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
> **国内用户备选**（使用阿里云镜像源，速度更快）：
> ```bash
> curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
> echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
> ```
### 5. 安装 Docker Engine + Docker Compose
```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-compose-plugin
```
## 二、安装后配置（必做）
### 1. 配置免 sudo 使用 Docker
```bash
# 将当前用户添加到 docker 用户组
sudo usermod -aG docker $USER
# 让配置立即生效（不需要重新登录）
newgrp docker
```
### 2. 配置镜像加速器（国内用户必做）
```bash
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": [
    "https://mirror.iscas.ac.cn",
    "https://docker.mirrors.ustc.edu.cn",
    "https://hub-mirror.c.163.com"
  ],
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "100m",
    "max-file": "3"
  }
}
EOF
# 重启 Docker 服务使配置生效
sudo systemctl daemon-reload
sudo systemctl restart docker
```