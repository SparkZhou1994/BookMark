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
- 一般以序列数据为输入，两个输入
- 通过网络内部的结构设计捕捉序列之间的关系特征
- 一般以序列形式进行输出，两个输出
上一层的输出作为下一层的隐藏输入，并加入新的输入，所以具有上一层的状态、记忆之前内容的能力
在最后的输出可以接入“全连接层”做到“用户意图识别”，RNN是序列输出并没有进行分类，

### RNN的作用和应用场景
#### RNN模型作用
对连续性的输入序列进行处理，如人类的语言、语音等
#### RNN应用场景有哪些
广泛应用与NLP领域的各项任务，如文本分类、情感分析、意图识别、机器翻译等
### RNN的API
```
import torch.nn as nn
nn.RNN(5,6,1) # 第一个参数为输入数据的尺寸，第二个参数为输出数据的尺寸，第三个参数为隐藏层的个数
# 还可以设置 batch_size，如果为1时，每次输入1个样本，如果为126时，模型能一次看到126个样本。再去决定梯度下降方向。
```
## ChatGPT项目实战之数据获取
### 数据获取
文件路径 x.txt
|列序号|列数据|列含义|列处理方式|
|:----:|:----:|:----:|:----:|
|第一列|人名|特征列|字符向量化(onehot编码)|
|第二列|国家|标签列|标签编码(labelencoder), 如German 映射成 0, Chinese 映射成 1|
#### 配置文件
```
# ~/config/config.py
# coding:utf8
"""
	存放配置文件
"""
train_path=r"x.txt"
```
#### 读取文件代码
```
# ~/utils/process.py
from config.config imprort train_path

def read_data(filename):
	my_list_x, my_list_y = [], []
	# 打开数据文件
	with open(filename, mode='r', encoding='utf-8') as f:
		for line in f.readlines():
			# 按照行提取样本x，y
			(x, y) = line.strip().split('\t')
			my_list_x.append(x)
			my_list_y.append(y)
	# 返回样本列表
	return my_list_x, my_list_y

if __name__ == '__main__':
	read_data(filename=train_path)
```
## ChatGPT项目实战之数据预处理模块
### 数据预处理
#### 独热编码 onehot
例如: lucy的序列标识: [[1,0,0,0],[0,1,0,0],[0,0,1,0],[0,0,0,1]]
|字母|l|u|c|y|
|:-:|:-:|:-:|:-:|
|l|1|0|0|0|
|u|0|1|0|0|
|c|0|0|1|0|
|y|0|0|0|1|
实现思路总结： 针对语料去重之后一共有多少个单词，然后以单词长度为基准建立列表，构建onehot编码形式
#### 标签编码 labelencoder
18个国家从0-17进行编号
#### 实现步骤
##### 构建数据源NameClassDataset
将原始数据封装成DataSet(封装成Torch张量形式)，使用DataLoader按照批次拿数据
###### 导入必备工具包
```
import torch
import torch.nn as nn
import torch.nn.functional as F
import torch.optim as optim
from torch.utils.data import Dataset, DataLoader
import string
import time
import matplotlib.pyplot as plt
from io import open
```
###### 获取常用的字符数量
```
# ~/config/config.py
all_letters = string.ascii_letters + " .,；"
all_letters.find("a") # str.find() 返回值如果包含字符串则返回索引值，否则返回-1，用于后面生成onehot编码
```
###### 获取国家各种类和个数
```
# ~/config/config.py
categorys = ['Italian', 'English', 'Arabic'] 
categorys.index("English") # 列表序号作为标签编码
```
###### 构建数据源NameClassDataset
```
# ~/utils/dataload.py
# TODO 第一步：构建DataSet数据源
class NameClassDataset(Dataset):
    # 1 init函数 设置样本x和y 将原始数据的my_list_x my_list_y 条目数self.sample_len 初始化
    def __init__(self, my_list_x, my_list_y):
        self.my_list_x = my_list_x
        self.my_list_y = my_list_y
        self.sample_len = len(my_list_x)

    # 2 __len__(self)函数   获取样本个数
    def __len__(self):
        return self.sample_len

    # 3 __getitem__(self, index)函数 获取第几条样本数据
    def __getitem__(self, index):
        # ①按索引 获取数据样本 x y 目标接下来转化为tensor,比如(zhang,china)
        x = self.my_list_x[index]
        y = self.my_list_y[index]
        # print("x代表的人名", x)  # x代表的人名 Adsit
        # print("y代表的国家名", y)  # y代表的国家名 Czech
        # ②zhang 编译成57个字符构成的独热编码 [5,57]
        tensor_x = torch.zeros(len(x), len(all_letters))
        # print("tensorX:", tensor_x, tensor_x.shape)
        # ③样本x one-hot张量化
        # enumerate("zhang")=[(0, 'z'), (1, 'h'), (2, 'a'), (3, 'n'), (4, 'g')]
        # tensor_x[0][all_letters.find('z')] = 1 字母z的onehot编码是[[0.......1........0]]，第25个位置下标为1其他全为0
        for li, letter in enumerate(x):
            # print(li) # 索引
            # print(letter)
            tensor_x[li][all_letters.find(letter)] = 1
            # break
        # print("tensor-onehot:", tensor_x, tensor_x.shape)
        # ④样本y 张量化 torch.tensor(categorys.index(y), dtype=torch.long)
        tensor_y = torch.tensor(categorys.index(y), dtype=torch.long)
        # print("tensor-y:", tensor_y, tensor_y.shape)
        # ⑤返回tensor_x, tensor_y
        return tensor_x, tensor_y
```
##### 构建迭代器DataLoader遍历数据
```
# ~/utils/dataload.py
# TODO 第二步：构建DataLoader函数，实现批次拿数据
def get_data():
    # 1 获取数据
    my_list_x, my_list_y = read_data(filename=train_path)
    # 2 实例化dataset对象
    name_class_dataset = NameClassDataset(my_list_x, my_list_y)
    # print(len(name_class_dataset))#20074
    # print(name_class_dataset[1])  # Adsit	Czech
    # 3 实例化dataloader
    train_loader = DataLoader(dataset=name_class_dataset, batch_size=1, shuffle=True) # shuffle=True 每次数据都不同
    return train_loader

# TODO 测试主函数
if __name__ == '__main__':
    train_loader = get_data()
    for x, y in train_loader:
        print('x.shape：\n', x.shape,
              x)  # torch.Size([1, 7, 57])      输入1个人名序列(1个样本)，人名单词由7个字母组成，每个人名单词由57个元素列表组成的(每个字母由57维向量表示的)
        print('y.shape：\n', y.shape, y)  # torch.Size([1]) tensor([8]) 国家来自于第8个国家
        break
```
## ChatGPT项目实战之ChatGPT模型构建实战
### RNN 人名分类模型
思路：输入人名，预测国家具体分类
数据 ----> RNN -> Linear -> 结果
输入[5,1,57], 5个字母组成的单词，1个batch_size, 57个字符onehot编码 -> RNN 输出 [5,1,128]， 输入 [1,128] -> 输出[1, 18]
```
# ~/model/rnn_model.py
import torch
# 导入nn准备构建模型
import torch.nn as nn
import torch.nn.functional as F


class RNNClassifier(nn.Module):
    #                     57          128          18
    def __init__(self, input_size, hidden_size, num_classes):
        super(RNNClassifier, self).__init__()
        self.hidden_size = hidden_size
        # 定义rnn层
        self.rnn=nn.RNN(input_size,hidden_size,batch_first=False) #[57,128]  [57,1,128] [1,57,128]
        # 定义linear层
        self.linear = nn.Linear(hidden_size, num_classes)

    def forward(self, x, hidden):
        # 数据形状 [5,57] -> [5,1,57]，指定的位置1处插入一个维度，表示batch_Size=1，每批次处理1个数据
        input = x.unsqueeze(1)
        # 1 数据经过模型 提取特征
        # 数据形状 [seqlen=5,1,57],[1,1,128]) -> rr [seqlen=6,1,128],hn [1,1,128]
        # rr 为 rnn的output
        rr, hn = self.rnn(input, hidden)
        # rnn模型 取最后一个128 作为人名特征，数据形状 [seqlen,1,128] - [1,128]  eg:[6,1,128] --> [1,128]
        tmprr = rr[-1]
        # 2 数据经过全连接层 [1,128] -->[1,18]
        output = self.linear(tmprr)
        return output

    def inithidden(self):
        # 初始化隐藏层输入数据 inithidden()
        # 形状是【隐层层数、批量大小batch_size、隐藏层大小】，这里[1,1,128]
        return torch.zeros(1, 1, self.hidden_size)


def dm01_test_myrnn():
    # 1-实例化rnn对象
    myrnn = RNNClassifier(57, 128, 18)
    print("myrnn的网络结构如何：",myrnn)
    # 2-准备数据,比如以人名zhang为例[5,57]
    input = torch.randn(5, 57)
    print("input shape:",input.shape)#torch.Size([5, 57])
    h0 = myrnn.inithidden()
    print("h0 shape:",h0.shape)#torch.Size([1, 1, 128])
    # 3.给模型输入数据
    # [seqlen,57],[1,1,128] -> [1,18],[1,1,128]
    # 一次性将数据送入模型,自动调用forward计算方法
    output = myrnn(input, h0)
    print(f"output shape is:{output.shape},output value is:{output}")#torch.Size([1, 18])


if __name__ == '__main__':
    dm01_test_myrnn()
```
## ChatGPT项目实战之ChatGPT模型训练实战
### 配置文件
```
# ~/config/config.py
# 学习率
mylr = 1e-3
# 训练轮次
epochs = 10
```
### 训练代码
```
# ~/model/train.py
# 导入计时工具
import time
# 导入之前定义的好包
from config.config import *
from utils.dataload import *
from model.rnn_model import *
import torch.optim as optim
import matplotlib.pyplot as plt
import os

os.environ["KMP_DUPLICATE_LIB_OK"] = 'True'


def train_rnn():
    # 1-从文件获取数据、构建数据迭代对象train_dataloader
    train_dataloader = get_data()
    # 2-实例化模型对象my_rnn
    input_size = 57
    hidden_size = 128
    num_classes = 18
    my_rnn = RNNClassifier(input_size, hidden_size, num_classes)
    # 3-损失函数采用nn.CrossEntropyLoss() 优化器对象myadam
    cross_entropy_loss = nn.CrossEntropyLoss()
    myadam = optim.Adam(my_rnn.parameters(), mylr)
    # 4-定义模型训练的参数:
    starttime = time.time()
    total_iter_num = 0  # 已训练的样本数
    total_loss = 0  # 已训练的损失和
    total_loss_list = []  # 每100个样本求1次平均损失，形成损失列表
    total_acc_num = 0  # 已训练样本预测准确总数
    total_acc_list = []  # 每100个样本求一次平均准确率，形成平均正确率列表
    # 5-模型训练
    # 5-1外层for循环 控制轮数 for epoch_idx in range(epochs)
    for epoch_idx in range(epochs):
        # 5-2-内层for循环 控制迭代次数 for i, (x, y) in enumerate(mydataloader)
        for i, (x, y) in enumerate(train_dataloader):
            # 5-3 给模型喂数据,因为x是3维的，这里采用x[0]得到二维数据
            output = my_rnn(x[0], my_rnn.inithidden())
            # 5-4 计算损失
            myloss = cross_entropy_loss(output, y)
            # 5-5 梯度清零-清空过往梯度
            myadam.zero_grad()
            # 5-5 反向传播-计算当前梯度
            myloss.backward()
            # 5-6 梯度更新-根据梯度更新网络参数
            myadam.step()
            # 简单的说就是进来一个batch的数据，计算一次梯度，更新一次网络
            # 6-计算辅助信息
            # 6-1 累加总损失和准确数
            total_iter_num += 1  # 已训练的样本数
            total_loss = total_loss + myloss.item()  # 已训练的损失和
            acc_num = 1 if torch.argmax(output).item() == y.item() else 0  # 真实值和预测值一致得1，否则为0
            total_acc_num = total_acc_num + acc_num  # 已训练样本预测准确总数
            # 6-2 每100次训练计算一个总体平均损失
            if total_iter_num % 100 == 0:
                avg_loss = total_loss / total_iter_num  # 已训练的损失和/已训练的样本数
                total_loss_list.append(avg_loss)
                avg_acc = total_acc_num / total_iter_num  # 已训练样本预测准确总数/已训练的样本数
                total_acc_list.append(avg_acc)
            # 6-3 2000次训练 打印日志
            if total_iter_num % 2000 == 0:
                avg_loss1 = total_loss / total_iter_num
                avg_acc1 = total_acc_num / total_iter_num
                print('轮次:%d, 损失:%.6f, 时间:%d，准确率:%.3f' % (epoch_idx, avg_loss1, time.time() - starttime, avg_acc1))
        torch.save(my_rnn.state_dict(), '../save_model/rnn_classfication_%s.bin' % (epoch_idx + 1))
    total_time = int(time.time() - starttime)
    # total_loss_list-每100个样本求1次平均损失，形成损失列表
    # total_time训练时间
    # total_acc_list 每100个样本求一次平均准确率，形成平均正确率列表
    return total_loss_list, total_time, total_acc_list

if __name__ == '__main__':
    train_rnn()
```
## ChatGPT项目实战之ChatGPT模型预测实战
```
# ~/predict.py
import torch
from model.rnn_model import *
from config.config import *


# 预测数据文本转数字
# 如：zhang --> [5,57]
def line_to_tensor(x):
    tensor_x = torch.zeros(len(x), len(all_letters))
    for i, letter in enumerate(x):
        tensor_x[i][all_letters.find(letter)] = 1
    return tensor_x


# 预测函数
def my_predict_rnn(x):
    input_size = 57
    hidden_size = 128
    num_classes = 18
    # 1 输入文本数据 张量化one-hot
    tensor_x = line_to_tensor(x)
    # 2 实例化模型 加载已训练模型参数 ./save_model/rnn_classfication_10.bin')
    my_rnn = RNNClassifier(input_size, hidden_size, num_classes)
    my_rnn.load_state_dict(torch.load("./save_model/rnn_classfication_10.bin"))
    # 3 模型预测 with torch.no_grad()，不计算梯度
    # Pytorch框架采用model.eval()表示进入模型评估模式，而不是训练模型
    my_rnn.eval()
    with torch.no_grad():
        # 4 从预测结果中取出前1名,显示打印结果 output.topk(1, 1, True)
        output = my_rnn(tensor_x, my_rnn.inithidden())
        print("output:",output)
        topValue,topIndex = output.topk(1, 1, True)
        print(topValue)#tensor([[2.1247]])
        print(topIndex)#tensor([[1]])
        value = topValue[0][0]
        index = topIndex[0][0]
        print(value)#tensor(2.1247)
        print(index)#tensor(1)
        result = categorys[index]
        print("value:%d,result:%s"%(value,result))


if __name__ == '__main__':
    my_predict_rnn('lucy')
    my_predict_rnn('zhang')
```
## ChatGPT项目实战之ChatGPT模型验证和结果分析
```
# ~/model/train.py
def dm_test_train_rnn():
    total_loss_list, total_time, total_acc_list = train_rnn()
    # 绘制损失曲线图,让大家更加直观了解模型训练损失下降，准确率提升
    plt.figure(0)
    plt.plot(total_loss_list, color='red', label='RNN')
    plt.legend(loc='upper left')
    plt.savefig('./rnn_loss_picture.png')
    plt.show()


if __name__ == '__main__':
    dm_test_train_rnn()
```
### 预测结果不正确的原因和解决方案
#### 原因
模型结构过于简单、数据集数量较少
#### 改进方法
增加模型深度(多加几个全连接层)、增加训练数据集
# 第三章 ChatGPT原理篇
## ChatGPT原理之深度学习基础
### 深度学习与神经网络
深度学习是解决机器学习在语音、图像识别不好的情况下，使用深度神经网络的学习技术
人工神经网络是模仿动物神经网络行为特征，进行信息处理的算法数学模型，简称ANN，也称NN
#### 感知机模型 Perceptron
误差的反向传播，利用梯度，更新参数取值来进行模型学习
#### BP 一层隐层的神经网络(三层神经网络)
#### CNN
### 如何设计神经网络结构
#### 确定神经网络层数
- 输入层 1层
- 输出层 1层
- 隐层   多层

#### 确定每层单元的个数
- 输入层单元个数根据输入数据个数确定
- 输出层单元个数根据目标分类个数确定
- 隐层根据准确度确定(流域法或者交叉验证法来验证隐层个数以及每层单元个数)

## ChatGPT原理之什么是语言模型
### N-gram 语言模型
引入马尔可夫假设: 随意一个词出现的概率只与它前面出现的有限的一个或者几个词有关
- 如果一个词的出现与它周围的词都是独立的，称为unigram(一元语言模型)
P(S) = P(W1) * P(W2) * …… * P(Wn)
- 如果一个词的出现仅依赖于它前面出现的一个词，称为bigram(二元语言模型)
P(S) = P(W1) * P(W2|W1) * P(W3|W2)* …… * P(Wn|Wn-1)
- 如果一个词的出现仅依赖于它前面出现的两个词，称为trigram(三元语言模型)
P(S) = P(W1) * P(W2|W1) * P(W3|W2, W1)* …… * P(Wn|Wn-1, Wn-2)
实践中用的最多的是bigram和trigram











