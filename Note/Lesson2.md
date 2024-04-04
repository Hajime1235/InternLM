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

#### 下载并运行优秀作品
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

#### 打开 `http://127.0.0.1:6006` ,就可以对话了


