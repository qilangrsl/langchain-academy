![LangChain Academy](https://cdn.prod.website-files.com/65b8cd72835ceeacd4449a53/66e9eba1020525eea7873f96_LCA-big-green%20(2).svg)

## 简介

欢迎使用 LangChain Academy 的 LangGraph 入门课程！
本课程是一套不断扩充的模块合集，聚焦 LangChain 生态中的基础概念。
Module 0 是基础环境配置，Module 1 - 5 逐步深入讲解 LangGraph 的构建，主题由浅入深；Module 6 讲解智能体的部署。
每个模块目录下都有一组 notebook，顶部附带对应 LangChain Academy 课程页面的链接，引导你完成该主题的学习。每个模块还有一个 `studio` 子目录，包含一组配套的图（graph），我们将借助 LangGraph API 和 Studio 进行探索。

## 环境配置

### Python 版本

请确保使用 Python 3.11、3.12 或 3.13。
```
python3 --version
```

### 克隆仓库
```
git clone https://github.com/langchain-ai/langchain-academy.git
$ cd langchain-academy
```
也可以直接下载 zip 包 [下载地址](https://github.com/langchain-ai/langchain-academy/archive/refs/heads/main.zip)。

### 创建环境并安装依赖（uv 管理）

本项目使用 [uv](https://docs.astral.sh/uv/) 管理依赖（`pyproject.toml` + `uv.lock`）。安装 uv 后在仓库根目录执行：
```
$ uv sync
```
首次执行会自动创建 `.venv` 虚拟环境并安装全部依赖。之后请用 `uv run` 前缀运行命令（如 `uv run jupyter notebook`），或手动激活：`source .venv/bin/activate`。

> 旧的 venv + pip 方式已废弃，根目录 `requirements.txt` 仅作参考。

### 运行 notebook
```
$ uv run jupyter notebook
```

### 配置环境变量
```
$ export API_ENV_VAR="你的 API Key"
```

### 配置商汤日日新（SenseNova）API Key

本项目统一使用 API 调用方式（OpenAI 兼容协议），LLM 改为商汤日日新模型，替代原 OpenAI 调用：
* 在 [商汤日日新控制台](https://platform.sensenova.cn) 注册并创建 API Key。
* 在环境中设置以下变量：
  - `SENSENOVA_API_KEY`：你的 API Key
  - `SENSENOVA_BASE_URL`：`https://token.sensenova.cn/v1`
  - `SENSENOVA_CHAT_MODEL`：对话模型，如 `sensenova-6.8-flash-lite`

### 配置硅基流动（SiliconFlow）API Key

向量嵌入（Embeddings）与重排（Rerank）统一改用硅基流动的模型：
* 在 [硅基流动控制台](https://cloud.siliconflow.cn) 注册并创建 API Key。
* 在环境中设置以下变量：
  - `SILICONFLOW_API_KEY`：你的 API Key
  - `SILICONFLOW_BASE_URL`：`https://api.siliconflow.cn/v1`
  - `SILICONFLOW_EMBEDDING_MODEL`：向量模型，如 `BAAI/bge-m3`
  - `SILICONFLOW_RERANK_MODEL`：重排模型，如 `BAAI/bge-reranker-v2-m3`

### 注册并配置 LangSmith API
* 在 [LangSmith](https://docs.langchain.com/langsmith/create-account-api-key#create-an-account-and-api-key) 注册，了解更多关于 LangSmith 及其在工作流中的使用方式 [见这里](https://www.langchain.com/langsmith)。
* 在环境中设置 `LANGSMITH_API_KEY`、`LANGSMITH_TRACING_V2="true"`、`LANGSMITH_PROJECT="langchain-academy"`。
* 如使用欧洲实例，还需设置 `LANGSMITH_ENDPOINT`="https://eu.api.smith.langchain.com"。

### 配置 Tavily API 用于网络搜索

* Tavily Search API 是专为 LLM 和 RAG 优化的搜索引擎，追求高效、快速、持续稳定的搜索结果。
* 在 [Tavily 官网](https://tavily.com/) 注册获取 API Key，注册简单且有非常宽松的免费额度。部分课程（Module 4）会用到 Tavily。

* 在环境中设置 `TAVILY_API_KEY`。

### 配置 Studio

* Studio 是一个用于查看和测试智能体的定制化 IDE。
* Studio 可以在本地运行，并在浏览器中打开，支持 Mac、Windows 和 Linux。
* 本地 Studio 开发服务器的文档见 [这里](https://docs.langchain.com/langsmith/studio#local-development-server)。
* LangGraph Studio 的图代码位于 `module-x/studio/` 目录（Module 1-5）。
* 启动本地开发服务器前，请确保虚拟环境已激活，然后在每个模块的 `/studio` 目录下运行：

```
langgraph dev
```

你会看到如下输出：
```
- 🚀 API: http://127.0.0.1:2024
- 🎨 Studio UI: https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024
- 📚 API Docs: http://127.0.0.1:2024/docs
```

打开浏览器访问 Studio UI：`https://smith.langchain.com/studio/?baseUrl=http://127.0.0.1:2024`。

* 使用 Studio 前需要创建包含相关 API Key 的 .env 文件。
* 在命令行运行以下脚本，为 Module 1 到 5 生成这些文件（示例）：
```
for i in {1..5}; do
  cp module-$i/studio/.env.example module-$i/studio/.env
  echo "SENSENOVA_API_KEY=\"$SENSENOVA_API_KEY\"" >> module-$i/studio/.env
  echo "SENSENOVA_BASE_URL=\"$SENSENOVA_BASE_URL\"" >> module-$i/studio/.env
  echo "SENSENOVA_CHAT_MODEL=\"$SENSENOVA_CHAT_MODEL\"" >> module-$i/studio/.env
  echo "SILICONFLOW_API_KEY=\"$SILICONFLOW_API_KEY\"" >> module-$i/studio/.env
  echo "SILICONFLOW_BASE_URL=\"$SILICONFLOW_BASE_URL\"" >> module-$i/studio/.env
  echo "SILICONFLOW_EMBEDDING_MODEL=\"$SILICONFLOW_EMBEDDING_MODEL\"" >> module-$i/studio/.env
  echo "SILICONFLOW_RERANK_MODEL=\"$SILICONFLOW_RERANK_MODEL\"" >> module-$i/studio/.env
done
echo "TAVILY_API_KEY=\"$TAVILY_API_KEY\"" >> module-4/studio/.env
```
