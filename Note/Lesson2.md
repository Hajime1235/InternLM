# 大模型Demo笔记

## 1. 部署 InternLM2-Chat-1.8B 模型

### 1.1 配置模型基础环境

#### 创建开发机并选择相应配置：
![image](https://github.com/Hajime1235/InternLM/assets/165744158/e372db0c-5302-4ec1-a84c-6642dbcc965e)

#### 配置环境：
```bash
studio-conda -o internlm-base -t demo
```

#### 进入conda环境：
```bash
conda activate demo
```

#### 安装环境包：
```bash
pip install huggingface-hub==0.17.3
pip install transformers==4.34 
pip install psutil==5.9.8
pip install accelerate==0.24.1
pip install streamlit==1.32.2 
pip install matplotlib==3.8.3 
pip install modelscope==1.9.5
pip install sentencepiece==0.1.99
```

### 1.2 下载模型

#### 创建文件夹（看不懂代码）
```bash
mkdir -p /root/demo
touch /root/demo/cli_demo.py
touch /root/demo/download_mini.py
cd /root/demo
```

#### 打开 `/root/demo/download_mini.py` ，并输入：
```bash
import os
from modelscope.hub.snapshot_download import snapshot_download

# 创建保存模型目录
os.system("mkdir /root/models")

# save_dir是模型保存到本地的目录
save_dir="/root/models"

snapshot_download("Shanghai_AI_Laboratory/internlm2-chat-1_8b", 
                  cache_dir=save_dir, 
                  revision='v1.1.0')
```

#### 下载模型参数文件
```bash
python/root/demo/download_mini.py
```

### 1.3 测试模型
#### 打开 `/root/demo/cli_demo.py` ，并输入代码，执行demo程序
```bash
import torch
from transformers import AutoTokenizer, AutoModelForCausalLM


model_name_or_path = "/root/models/Shanghai_AI_Laboratory/internlm2-chat-1_8b"

tokenizer = AutoTokenizer.from_pretrained(model_name_or_path, trust_remote_code=True, device_map='cuda:0')
model = AutoModelForCausalLM.from_pretrained(model_name_or_path, trust_remote_code=True, torch_dtype=torch.bfloat16, device_map='cuda:0')
model = model.eval()

system_prompt = """You are an AI assistant whose name is InternLM (书生·浦语).
- InternLM (书生·浦语) is a conversational language model that is developed by Shanghai AI Laboratory (上海人工智能实验室). It is designed to be helpful, honest, and harmless.
- InternLM (书生·浦语) can understand and communicate fluently in the language chosen by the user such as English and 中文.
"""

messages = [(system_prompt, '')]

print("=============Welcome to InternLM chatbot, type 'exit' to exit.=============")

while True:
    input_text = input("\nUser  >>> ")
    input_text = input_text.replace(' ', '')
    if input_text == "exit":
        break

    length = 0
    for response, _ in model.stream_chat(tokenizer, input_text, messages):
        if response is not None:
            print(response[length:], flush=True, end="")
            length = len(response)
```

```bash
conda activate demo
python /root/demo/cli_demo.py
```

#### 测试：
请创作一个 300 字的小故事

## 2.实战部署优秀作品

### 2.1配置基础环境

#### 进入conda环境：
```bash
conda activate demo
```

#### 获取Demo文件（git命令）
```bash
cd /root/
git clone https://gitee.com/InternLM/Tutorial -b camp2
# git clone https://github.com/InternLM/Tutorial -b camp2
cd /root/Tutorial
```

### 2.2下载并运行优秀作品
```bash
python /root/Tutorial/helloworld/bajie_download.py
```

```bash
streamlit run /root/Tutorial/helloworld/bajie_chat.py --server.address 127.0.0.1 --server.port 6006
# 运行程序
```

打开powershell并输入命令
![image](https://github.com/Hajime1235/InternLM/assets/165744158/c66923a4-fbb8-490d-acc7-912596472046)
```bash
# 从本地使用 ssh 连接 studio 端口
# 将下方端口号 38374 替换成自己的端口号
ssh -CNg -L 6006:127.0.0.1:6006 root@ssh.intern-ai.org.cn -p 38374
```
在平台中查看端口、密码并填入
![image](https://github.com/Hajime1235/InternLM/assets/165744158/20b401f6-8a0f-45dc-bd64-4dac66f4b380)


#### 打开 `http://127.0.0.1:6006` ,就可以对话了

## 3.使用 Lagent 运行 InternLM2-Chat-7B 模型

### 3.1 Lagent 框架图和特性
![image](https://github.com/Hajime1235/InternLM/assets/165744158/1534ab4a-83fe-4a8d-ba44-82e9ac19f402)
#### 特性（不是很理解诶）：
* 流式输出：提供 stream_chat 接口作流式输出，本地就能演示酷炫的流式 Demo。
* 接口统一，设计全面升级，提升拓展性，包括：
  * Model : 不论是 OpenAI API, Transformers 还是推理加速框架 LMDeploy 一网打尽，模型切换可以游刃有余；
  * Action: 简单的继承和装饰，即可打造自己个人的工具集，不论 InternLM 还是 GPT 均可适配；
  * Agent：与 Model 的输入接口保持一致，模型到智能体的蜕变只需一步，便捷各种 agent 的探索实现；
* 文档全面升级，API 文档全覆盖。

### 3.2 配置基础环境

#### 进入conda环境：
```bash
conda activate demo
```

#### 打开文件子路径
```bash
cd /root/demo
```

下载 Lagent 相关的代码库（git 命令）：
```bash
git clone https://gitee.com/internlm/lagent.git
# git clone https://github.com/internlm/lagent.git
cd /root/demo/lagent
git checkout 581d9fb8987a5d9b72bb9ebd37a95efd47d479ac
pip install -e . # 源码安装
```

### 3.3 使用 Lagent 运行 InternLM2-Chat-7B 模型为内核的智能体
#### 打开 lagent 路径：
```bash
cd /root/demo/lagent
```

#### 在 terminal 中输入指令，构造软链接快捷访问方式：
```bash
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm2-chat-7b /root/models/internlm2-chat-7b
```

#### 打开 lagent 路径下 examples/internlm2_agent_web_demo_hf.py 文件，并修改对应位置 (71行左右) 代码：
```bash
# 其他代码...
value='/root/models/internlm2-chat-7b'
# 其他代码...
```
![image](https://github.com/Hajime1235/InternLM/assets/165744158/1ea13469-6e16-42b6-aec5-9516cbbf582b)

#### 输入运行命令(要等5分钟)
```bash
streamlit run /root/demo/lagent/examples/internlm2_agent_web_demo_hf.py --server.address 127.0.0.1 --server.port 6006
```

随后重复2.2的powershell步骤，输入密码后点开链接，勾上数据分析，其他的选项不要选择，进行计算方面的 Demo 对话。
```bash
请解方程 2*X=1360 之中 X 的结果
```

## 4. 实践部署 浦语·灵笔2（XComposer2，图文多模态大模型） 模型

### 5.1 配置基础环境（50% A100）

#### 进入conda环境：
```bash
conda activate demo
# 补充环境包
pip install timm==0.4.12 sentencepiece==0.1.99 markdown2==2.4.10 xlsxwriter==3.1.2 gradio==4.13.0 modelscope==1.9.5
```

#### 下载 InternLM-XComposer 仓库 相关的代码资源：
```bash
cd /root/demo
git clone https://gitee.com/internlm/InternLM-XComposer.git
# git clone https://github.com/internlm/InternLM-XComposer.git
cd /root/demo/InternLM-XComposer
git checkout f31220eddca2cf6246ee2ddf8e375a40457ff626
```

#### 在 terminal 中输入指令，构造软链接快捷访问方式：
```bash
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm-xcomposer2-7b /root/models/internlm-xcomposer2-7b
ln -s /root/share/new_models/Shanghai_AI_Laboratory/internlm-xcomposer2-vl-7b /root/models/internlm-xcomposer2-vl-7b
```

### 5.2图文写作实战

#### 启动 InternLM-XComposer：

```bash
cd /root/demo/InternLM-XComposer
python /root/demo/InternLM-XComposer/examples/gradio_demo_composition.py  \
--code_path /root/models/internlm-xcomposer2-7b \
--private \
--num_gpus 1 \
--port 6006
```

随后重复2.2的powershell步骤，输入密码后点开链接，进行使用。

### 5.3 图片理解实战

#### 关闭并重新启动一个新的 terminal，继续输入指令，启动 InternLM-XComposer2-vl ，并打开链接 `http://127.0.0.1:6006` 进行使用
```bash
conda activate demo

cd /root/demo/InternLM-XComposer
python /root/demo/InternLM-XComposer/examples/gradio_demo_chat.py  \
--code_path /root/models/internlm-xcomposer2-vl-7b \
--private \
--num_gpus 1 \
--port 6006
```
```bash
请分析一下图中内容
```

# 结束
