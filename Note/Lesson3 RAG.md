# 使用茴香豆搭建 RAG 智能助理

## LLM优化方法比较
![image](https://github.com/Hajime1235/InternLM/assets/165744158/9239cdb0-9683-4dfe-97f5-46179cfe4c23)


## RAG 概述
#### 定义：
RAG（Retrieval Augmented Generation，增强型检索生成）技术，通过检索与用户输入相关的信息片段，并结合外部知识库来生成更准确、更丰富的回答。20年最初提出（感觉Perplexity像这个）

#### 用途：
解决 LLMs 在处理知识密集型任务时可能遇到的挑战, 如幻觉、知识过时；缺乏透明、可追溯的推理过程等。

#### 工作原理：
![image](https://github.com/Hajime1235/InternLM/assets/165744158/2660b8b7-288e-48c5-9f68-128b4d8f9b4d)
索引模块会将输入的知识源切割成小块，编码成向量，储存在向量数据库中。

模型检索时，会将问题也编码成向量，并在向量数据库找寻与问题最相关的文档块（根据相似度得分）。

模型生成时，将检索到的文档块与原始问题一起输入到 LLM 中给出回答。

#### RAG优化方法：
![image](https://github.com/Hajime1235/InternLM/assets/165744158/7e5c2138-6f73-49b3-86b2-50307380793e)

#### RAG 适用范围：
适合需要结合最新信息和实时数据的任务

#### RAG 评估框架和基准测试
![image](https://github.com/Hajime1235/InternLM/assets/165744158/ee18fc30-13a3-4f95-9240-43c61dab6b55)


## 1. 环境配置

#### 茴香豆工作原理
![image](https://github.com/Hajime1235/InternLM/assets/165744158/9bd72d2a-808b-4cd3-906f-c72ebf2be9c3)
![image](https://github.com/Hajime1235/InternLM/assets/165744158/6f5faad2-6202-4229-af61-b7a3500cdb32)
![image](https://github.com/Hajime1235/InternLM/assets/165744158/3e9147ce-eaf0-48fc-8b03-a5d532f3cf24)

#### 1.1 茴香豆Web版演示
![image](https://github.com/Hajime1235/InternLM/assets/165744158/4e683a02-30af-439e-891f-72a636ce5ea3)

#### 1.2 把茴香豆部署到服务器上

按往常进入开发机
按教程配置环境，下载茴香豆
![image](https://github.com/Hajime1235/InternLM/assets/165744158/7b561cb9-3840-4e94-a83e-532fd27fe264)

## 2. 使用茴香豆搭建RAG助手

#### 2.1 修改配置文件
![image](https://github.com/Hajime1235/InternLM/assets/165744158/19fc18d1-827a-48ac-bb15-040701b9bc0a)

#### 2.2 创建知识库

将茴香豆的相关语料下载至开发机
![image](https://github.com/Hajime1235/InternLM/assets/165744158/966cc643-890b-47a4-be25-495b7ef7876f)
加入接受问题事例
![image](https://github.com/Hajime1235/InternLM/assets/165744158/a8499704-3e3c-466d-bcbe-65119d3c22bf)
加入测试用的问询列表，测试拒答流程是否正常
![image](https://github.com/Hajime1235/InternLM/assets/165744158/2beb3042-1191-4dd2-9bc6-dd8adde9a2b8)
创建RAG检索过程中使用的向量数据库，对事例问题进行向量化
![image](https://github.com/Hajime1235/InternLM/assets/165744158/a417d59a-e46d-4f7b-973f-63b7bb131811)

#### 2.3 运行茴香豆
![image](https://github.com/Hajime1235/InternLM/assets/165744158/bd0e10eb-5985-4112-a134-56833b0d6f98)

## 3. 茴香豆进阶：待学习
