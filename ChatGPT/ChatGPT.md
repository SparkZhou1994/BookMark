# 第一章 ChatGPT介绍
## ChatGPT背景介绍
### 什么是ChatGPT
Optimizing Language Models for Dialogue
- 优化 相对于 GPT-3
- 对话 交互谈话
- 语言 NLP自然语言处理
- 模型 基于已有的数据，结合机器学习或深度学习算法，构建模型，对未知事物进行预测（类似人类积累经验）

## ChatGPT入门程序
### 获取API-KEY
- https://openai.com/api/
- view api key
- create new secret key

### 安装OpenAI的Python库
- pypi.org
- search openai
- pip install openai -i 清华镜像源

### Python实现ChatGPT模型
```
import os
import openai

openai.api_key = ""
response = openai.Completion.create(
	model = "text-davinci-003", # chatGPT背后的模型
	prompt = "向chatgpt提的问题",
	max_tokens = 256, # 返回的最大字符个数
)
# 打印结果
message = response.choices[0].text
print(message)
```
# 第二章 ChatGPT项目实战
## ChatGPT项目实战之项目背景介绍
人名分类器，根据人名预测是哪个国家的
### 项目解决方案确认
NLP 解决问题的领域
- 文本分类/情感分析
文本属于新闻、论文、小说
- 文本摘要
聚集核心部分
- 问答系统
- 对话系统
- 文本聚类
- 知识图谱

## ChatGPT项目实战之项目架构
### 数据获取
### 数据处理
目的: 将文本数据处理成符合模型输入的文本
文本张量化: 将文本转化为符合模型输入x以及对应标签y(lucy-English)
构建数据迭代器: 将上一步数据源进一步封装，方便模型输入
### 模型构建与训练
模型构建: RNN系列模型+全连接层
模型训练: 模型预测值和真实值Loss(计算损失，反向传播，梯度更新，模型保存)
### 模型评估
作用: 检验模型效果(实例化模型，加载保存模型，模型预测)
### 项目整体架构
#### 项目代码结构
- data 数据文件夹
- models 模型文件夹
- save_model 保存模型文件夹
- utils 工具类
- config 配置文件

#### 环境搭建
##### Windows 测试环境
pip install torch torchvision torchaudio -i https://pypi.tuna.tsinghua.edu.cn/simple # 安装Pytorch1.13
##### UCloud云服务测试环境
PyCharm连接UCloud云服务器，输入IP和端口
conda list 查看环境
conda activate chatgpt 激活环境
## ChatGPT项目实战之RNN、模型简介
### 什么是RNN
RNN(Recurrent Neural Network)，循环神经网络
- 一般以序列数据为输入
- 通过网络内部的结构设计捕捉序列之间的关系特征
- 一般以序列形式进行输出














