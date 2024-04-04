##大模型Demo#
##1.部署 InternLM2-Chat-1.8B 模型##
###配置基础环境###
####创建开发机并选择相应配置####：
![image](https://github.com/Hajime1235/InternLM/assets/165744158/e372db0c-5302-4ec1-a84c-6642dbcc965e)
####配置环境####：
studio-conda -o internlm-base -t demo
####进入conda环境####：
conda activate demo
####安装环境包####：
pip install huggingface-hub==0.17.3
pip install transformers==4.34 
pip install psutil==5.9.8
pip install accelerate==0.24.1
pip install streamlit==1.32.2 
pip install matplotlib==3.8.3 
pip install modelscope==1.9.5
pip install sentencepiece==0.1.99
