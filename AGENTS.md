# AGENTS.md — langchain-academy

## 项目性质
LangChain Academy 的 LangGraph 教学仓库，无传统构建/测试体系。内容由教学 Jupyter notebook（`module-0` 至 `module-6`）和配套的 LangGraph Studio 图代码（`module-x/studio/`）组成。没有 pytest/lint/CI 配置。

## 常用命令
```bash
# 依赖用 uv 管理（pyproject.toml + uv.lock），Python 3.11+；首次使用会自动创建 .venv
uv sync                                 # 安装/同步依赖
uv run jupyter notebook                 # 运行教学 notebook
cd module-N/studio && uv run langgraph dev   # 启动本地 LangGraph API/Studio（端口 2024）
```
> 旧版 venv 方式（`python3 -m venv lc-academy-env` + `pip install -r requirements.txt`）已废弃，根目录 `requirements.txt` 仅作参考，实际依赖以 `pyproject.toml` 为准。

## 验证方式
无自动化测试。改动 `studio/` 下的图代码后，验证手段是在对应 `module-N/studio/` 目录运行 `langgraph dev`，确认图能加载（或用 `langgraph.json` 中 `graphs` 指向的 `xxx.py:graph` 在 notebook 里直接调用 `graph.invoke(...)`）。

## 结构与约定
- `module-N/studio/langgraph.json` 定义该模块暴露的图（键 → `文件名:graph变量`）；新增图必须同时在 `graphs` 中注册。
- 每个 `studio/` 目录有自己的 `.env.example` 和 `requirements.txt`；`.env` 不入库，运行 Studio 前需从 `.env.example` 复制并填入 key。
- notebook 顶部第一个单元格链接到对应课程页面，不要删除。

## 必需的环境变量（统一走 OpenAI 兼容协议的 API 调用）
- 商汤日日新（LLM，所有模块）：`SENSENOVA_API_KEY`、`SENSENOVA_BASE_URL`（https://token.sensenova.cn/v1）、`SENSENOVA_CHAT_MODEL`（如 sensenova-6.8-flash-lite）
- 硅基流动（Embeddings/Rerank）：`SILICONFLOW_API_KEY`、`SILICONFLOW_BASE_URL`（https://api.siliconflow.cn/v1）、`SILICONFLOW_EMBEDDING_MODEL`（BAAI/bge-m3）、`SILICONFLOW_RERANK_MODEL`（BAAI/bge-reranker-v2-m3）
- `LANGSMITH_API_KEY`、`LANGSMITH_TRACING_V2="true"`、`LANGSMITH_PROJECT="langchain-academy"`
- `TAVILY_API_KEY`（Module 4 的搜索功能）
- 根目录 `.env` 已含全部配置，可直接复制到各 `module-N/studio/.env`

## 项目特有注意事项
- notebook 使用 LangGraph 的 checkpoint SQLite（`langgraph-checkpoint-sqlite`），模块目录下可能生成 `.db` 文件（如 `module-2/state_db/example.db`），属预期产物。
- `langgraph dev` 需在激活虚拟环境后、于对应 `studio/` 目录内运行，否则读不到该模块的 `langgraph.json` 和 `.env`。
- 教学代码刻意保持简单直白，不要引入抽象、类型标注或生产级错误处理。

## 维护规则
当项目结构、构建/测试命令、架构边界、开发约定或本文件记录的其他事实发生变化时，必须在同一次改动中同步更新本文件。
