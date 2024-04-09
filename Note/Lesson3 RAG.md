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

## 2. 使用茴香豆搭建RAG助手

## 3. 茴香豆进阶
