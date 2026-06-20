# CustomerOpsAgent_2｜Phase 2 Readiness Audit

> **Created**: 2026-06-20
> **Version**: v1.4-phase2-readiness-audit
> **Purpose**: Phase 2 准入审查，不是功能开发
> **Status**: Audit Complete

---

## 1. Audit Purpose

本文档是 Phase 2 准入审查报告。目标是判断是否值得从 Phase 1（mock-only RAG eval harness）进入 Phase 2（真实 RAG 集成评估）。

**本轮只做审查，不做功能开发。**

- ✅ 代码阅读和依赖映射
- ✅ 本机环境可行性检查
- ✅ 成本与风险分析
- ✅ Go / No-Go 决策
- ❌ 不写真实 Qdrant adapter
- ❌ 不接真实 API Key
- ❌ 不改核心源码
- ❌ 不修改 basjoo 代码

---

## 2. Current Phase 1 State

### 2.1 Phase 1 已完成内容

| 交付物 | 状态 | 详情 |
|---|---|---|
| RAG Evaluation Harness | ✅ | 15 eval cases, 6 类场景 |
| Mock Embedding | ✅ | 字符频率 256 维向量，无需 API Key |
| Mock Retriever | ✅ | 内存余弦相似度，无需 Qdrant |
| Mock RAG Pipeline | ✅ | 提取式答案，无需 LLM |
| pytest 测试 | ✅ | 37 passed |
| 独立 eval runner | ✅ | 15 passed, 0 failed |
| SmartHome Demo Data | ✅ | 2 Agent, 3 文档, 15 问题, 3 对话, 8 Bad Cases |
| seed_demo_data.py | ✅ | --validate-only / --dry-run / --mock 三种模式 |
| 评估报告 | ✅ | JSON + Markdown 双格式 |
| 基线回归 | ✅ | 267 passed, 36 failed, 1 skipped，无新增失败 |
| 文档 | ✅ | rag-evaluation.md, ENHANCEMENT_SUMMARY.md/.zh-CN.md |
| 版本标签 | ✅ | v1.0 → v1.3 全部推送 |

### 2.2 Phase 1 未完成内容

| 未完成项 | 原因 |
|---|---|
| 真实 Qdrant 检索评估 | 需要 Qdrant 服务 + Embedding API Key |
| 真实 Embedding API 评估 | 需要 Jina/SiliconFlow API Key |
| 真实 LLM 问答质量评估 | 需要 LLM API Key |
| Mock vs Real 对比报告 | 依赖以上三项 |
| 多轮对话评估 | Phase 1 只做单轮 |
| 延迟/性能指标 | Phase 1 只关注质量 |

### 2.3 Mock 评估的局限性

| 维度 | Mock 能验证 | Mock 不能验证 |
|---|---|---|
| 测试框架结构 | ✅ | — |
| 指标计算逻辑 | ✅ | — |
| 报告生成 | ✅ | — |
| 语义检索精度 | — | ❌ 字符频率向量不是语义向量 |
| 真实 LLM 回答质量 | — | ❌ 提取式不是生成式 |
| 真实 Qdrant 性能 | — | ❌ 内存搜索不等于向量库搜索 |
| 真实 citation 准确性 | — | ❌ Mock pipeline 的 citation 是模拟的 |

---

## 3. Phase 2 Candidate Options

### 方向 A：Real Qdrant Evaluation Adapter

**目标**：
- 启动真实 Qdrant
- 将 demo knowledge base 写入真实向量库
- 运行真实 retrieval eval
- 比较 mock retrieval 与 real Qdrant retrieval 的结果

**前置条件**：
- Qdrant 可用（Docker / 本地二进制 / Qdrant Cloud）
- Embedding API Key（Jina / SiliconFlow）
- Windows 本地运行方案确认

**工作量估计**：1-2 周

**收益**：
- 从 mock eval 升级到真实 retrieval eval
- 生成 mock vs real 对比报告
- 能更有说服力地证明 RAG eval harness 价值

### 方向 B：API-level RAG Eval Integration

**目标**：
- 通过 Basjoo 真实 chat / stream API 跑 eval cases
- 记录 response / citation / fallback
- 输出 API-level eval report

**前置条件**：
- Qdrant 运行
- Embedding API Key
- LLM API Key（DeepSeek / OpenAI）

**工作量估计**：2-3 周

**收益**：
- 端到端真实 RAG 质量评估
- 覆盖 retrieval + generation + citation 全链路

### 方向 C：冻结项目

**目标**：
- 不进入真实服务集成
- 保留 Phase 1 作为完整阶段成果
- 回到 CodePilot 或 Portfolio 主线

**前置条件**：无

**工作量估计**：0

**收益**：
- 零风险
- Phase 1 已经有完整交付物
- 可以把精力投入到其他项目

---

## 4. Basjoo Real RAG Dependency Map

### 4.1 Qdrant 依赖

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| Client 初始化 | `backend/services/qdrant_service.py` | 42-47 | `AsyncQdrantClient(settings.qdrant_url, settings.qdrant_api_key, settings.qdrant_timeout)` |
| Collection 创建 | `backend/services/qdrant_service.py` | 49-73 | `ensure_collection()` — idempotent, `VectorParams(size=dim, distance=COSINE)` |
| Collection 命名 | `backend/services/qdrant_service.py` | 35-38 | `kb_` + kb_id 前 12 位 hex |
| 向量搜索 | `backend/services/qdrant_service.py` | 122-155 | `search_kb()` — 双重隔离：tenant_id + kb_id payload filter |
| 维度映射 | `backend/services/qdrant_service.py` | 21-32 | bge-m3=1024, text-embedding-3-large=3072, 默认=1024 |

### 4.2 Embedding Provider 依赖

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| API Key 分发 | `backend/services/kb_document_processor.py` | 42-62 | `get_embedding_api_key(agent)` — jina/siliconflow/custom |
| Embedding 调用 | `backend/services/kb_document_processor.py` | 147-156 | `parser.embed_texts(texts, model, base_url, api_key)` |
| Query Embedding | `backend/services/kb_retrieval_service.py` | 77-81 | 检索时 embed query |

### 4.3 Knowledge Base 写入流程

| 步骤 | 文件 | 行号 | 功能 |
|---|---|---|---|
| 文档保存 | `backend/services/kb_document_processor.py` | 111-130 | 保存文件到磁盘 |
| 文档解析 | `backend/services/kb_document_processor.py` | 135 | `document_parser.parse_with_retry()` |
| 分块 | `backend/services/kb_document_processor.py` | 142 | `chunk_text()` |
| Embedding | `backend/services/kb_document_processor.py` | 156 | `embed_texts()` |
| Qdrant 写入 | `backend/services/kb_document_processor.py` | 194 | `qdrant.batch_upsert_points()` |
| DB 记录 | `backend/services/kb_document_processor.py` | 184-191 | 创建 `KbChunk` 记录 |
| 配置锁定 | `backend/services/kb_document_processor.py` | 201-202 | 首次成功后锁定 embedding 配置 |

### 4.4 Chat / Stream API 入口

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| 非流式聊天 | `backend/api/v1/endpoints.py` | 1106-1227 | `POST /api/v1/chat` |
| 流式聊天 | `backend/api/v1/endpoints.py` | 1230-1491 | `POST /api/v1/chat/stream` (SSE) |
| KB 检索调用 | `backend/api/v1/endpoints.py` | 867-911 | `KbRetrievalService().retrieve()` |
| 请求准备 | `backend/api/v1/endpoints.py` | 736-952 | `prepare_chat_request()` |

### 4.5 Citation / Source 字段生成

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| 检索结果字段 | `backend/services/kb_retrieval_service.py` | 114-126 | text, doc_id, chunk_index, score, filename, source_type, source_url, source_title |
| Chat 中的 sources | `backend/api/v1/endpoints.py` | 884-909 | 构建 kb_sources 列表：type, title, url, filename, snippet |
| 来源标签 | `backend/api/v1/endpoints.py` | 900-909 | `[来源N]` 标签 + URL |
| 占位符替换 | `backend/api/v1/endpoints.py` | 513-537 | `replace_source_placeholders()` |
| SSE sources 事件 | `backend/api/v1/endpoints.py` | 1316 | 流式模式下先发 sources 事件 |

### 4.6 No-Answer Fallback 逻辑

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| 相似度阈值 | `backend/services/kb_retrieval_service.py` | 99-108 | 优先级：显式参数 > agent.similarity_threshold > 默认 0.6 |
| 空 context fallback | `backend/api/v1/endpoints.py` | 921-925 | kb_context 为空时注入 "[No relevant information...]" 到 system prompt |
| LLM 失败 fallback | `backend/api/v1/endpoints.py` | 1169-1179, 1389-1405 | 返回 agent.restricted_reply 或 "service busy" |
| 空回答 fallback | `backend/api/v1/endpoints.py` | 1182-1186, 1408-1415 | LLM 返回空时触发 fallback |

### 4.7 测试基础设施

| 组件 | 文件 | 行号 | 功能 |
|---|---|---|---|
| Mock LLM | `backend/tests/conftest.py` | 42-55 | `mock_llm_service` (autouse) — monkeypatch `get_llm_service` |
| Mock Qdrant | — | — | **不存在** — 没有 mock Qdrant fixture |
| SQLite fixture | `backend/tests/conftest.py` | 118-158 | 每个测试独立 SQLite DB |
| 测试 Agent | `backend/tests/conftest.py` | 161-174 | 创建默认 Agent，含 `jina_api_key="test_jina_key"` |

### 4.8 环境变量

| 变量 | 文件 | 默认值 | 用途 |
|---|---|---|---|
| `QDRANT_URL` | `.env.example:21` | `http://localhost:6333` | Qdrant 服务地址 |
| `QDRANT_API_KEY` | `.env.example:22` | 空 | Qdrant API Key（可选） |
| `QDRANT_TIMEOUT` | `.env.example:23` | `30.0` | Qdrant 超时 |
| `JINA_API_KEY` | `.env.example:71` | 空 | Jina Embedding API Key |
| `SILICONFLOW_API_KEY` | `.env.example:71` | 空 | SiliconFlow API Key |
| `DEEPSEEK_API_KEY` | `.env.example:52` | 空 | DeepSeek LLM API Key |

### 4.9 如果接真实 Qdrant，需要改哪些最小文件

**不修改 basjoo 核心代码的前提下**，v2.0 最小闭环需要：

| 文件 | 操作 | 说明 |
|---|---|---|
| `backend/scripts/run_rag_eval.py` | 扩展 | 添加 `--real` 模式，使用真实 KbRetrievalService |
| `backend/scripts/seed_demo_data.py` | 扩展 | 实现 `--write-db` 模式，写入真实 DB + Qdrant |
| `backend/tests/rag_eval/conftest.py` | 扩展 | 添加 real retriever fixture（可选） |
| `.env` | 新增 | 配置 QDRANT_URL, JINA_API_KEY（不提交） |

**不需要修改的文件**：
- `backend/services/qdrant_service.py` — 直接使用
- `backend/services/kb_retrieval_service.py` — 直接使用
- `backend/services/kb_document_processor.py` — 直接使用
- `backend/api/v1/endpoints.py` — 直接使用

### 4.10 如果做 API-level eval，需要调用哪些 API 或 Service

| API / Service | 端点 / 调用方式 | 用途 |
|---|---|---|
| Chat Stream | `POST /api/v1/chat/stream` | 流式聊天，获取 answer + sources |
| Chat | `POST /api/v1/chat` | 非流式聊天 |
| KbRetrievalService | 内部服务调用 | 单独测试检索质量 |
| Agent Config | `GET /api/v1/agent:default` | 获取 agent 配置 |
| KB Status | 内部服务调用 | 检查 KB 是否就绪 |

---

## 5. Local Environment Check

### 5.1 检查结果

| 工具 | 版本 | 状态 | 说明 |
|---|---|---|---|
| Docker | — | ❌ 未安装 | `docker: command not found` |
| Docker Compose | — | ❌ 未安装 | `docker compose: command not found` |
| WSL | — | ❓ 状态不明 | 命令输出乱码，exit code 1，可能未正确配置 |
| Python | 3.11.5 | ✅ 可用 | 满足 Basjoo 要求 (3.11+) |
| Node.js | v24.15.0 | ✅ 可用 | 满足 Basjoo 要求 (18+) |
| npm | 11.12.1 | ✅ 可用 | 满足 Basjoo 要求 (9+) |

### 5.2 Docker Desktop 检查

| 检查项 | 结果 |
|---|---|
| `C:\Program Files\Docker\Docker\Docker Desktop.exe` | ❌ 不存在 |
| `winget list Docker.DockerDesktop` | ❌ 未安装 |
| `docker` 命令 | ❌ 不可用 |
| `docker compose` 命令 | ❌ 不可用 |

### 5.3 环境可行性判断

| 判断项 | 结论 |
|---|---|
| **Docker 是否可用** | ❌ 否 |
| **WSL 是否可用** | ❓ 需要手动验证 |
| **Windows 本地能否不靠 Docker 跑 Qdrant** | ⚠️ 理论上可以（Qdrant 提供 Linux 二进制），但需要 WSL 或编译 |
| **是否有 Qdrant 二进制 / 本地服务替代方案** | ⚠️ Qdrant Cloud 免费层可用（1GB 存储） |
| **是否更适合用远程 Qdrant Cloud** | ✅ 是 — Docker 不可用时的最简方案 |
| **是否需要真实 Embedding API Key** | ✅ 是 — Jina 或 SiliconFlow |
| **如果使用真实 API Key，应该如何避免泄漏** | 使用 `.env` 文件，加入 `.gitignore`，不提交到仓库 |

---

## 6. Feasibility Analysis

### 6.1 方向 A：Real Qdrant Evaluation Adapter

| 维度 | 评估 | 说明 |
|---|---|---|
| **技术可行性** | ⚠️ 有条件 | 需要解决 Qdrant 部署问题 |
| **Qdrant 部署** | ⚠️ 需要额外步骤 | Docker 不可用，需用 Qdrant Cloud 或 WSL |
| **Embedding API** | ✅ 可行 | Jina/SiliconFlow 有免费额度 |
| **代码改动量** | ✅ 小 | 只需扩展 run_rag_eval.py 和 seed_demo_data.py |
| **风险** | 中 | 环境配置可能遇到坑 |
| **收益** | 高 | mock vs real 对比报告，显著增强项目说服力 |

### 6.2 方向 B：API-level RAG Eval Integration

| 维度 | 评估 | 说明 |
|---|---|---|
| **技术可行性** | ⚠️ 有条件 | 依赖 A + LLM API Key |
| **额外依赖** | LLM API Key | DeepSeek / OpenAI |
| **代码改动量** | 中 | 需要实现 API 调用 + SSE 解析 |
| **风险** | 中-高 | 多个外部依赖，调试复杂 |
| **收益** | 高 | 端到端真实 RAG 评估 |

### 6.3 方向 C：冻结项目

| 维度 | 评估 | 说明 |
|---|---|---|
| **技术可行性** | ✅ 完全可行 | 无外部依赖 |
| **代码改动量** | 无 | — |
| **风险** | 无 | — |
| **收益** | 中 | Phase 1 已有完整交付物，但缺少真实 RAG 验证 |

---

## 7. Risk Analysis

### 7.1 技术风险

| 风险 | 级别 | 影响 | 缓解措施 |
|---|---|---|---|
| Docker 不可用 | **高** | 无法用 docker compose 启动 Qdrant | 用 Qdrant Cloud 免费层替代 |
| WSL 配置复杂 | 中 | Windows 环境下跑 Qdrant 困难 | 优先用远程 Qdrant Cloud |
| Qdrant Cloud 网络延迟 | 低 | 评估结果可能受网络影响 | 记录网络条件，对比本地结果 |
| Embedding API 不稳定 | 低 | API 调用可能失败 | 加重试机制，记录失败率 |
| Windows 环境差异 | 低 | 路径、编码等问题 | 已验证 Python/Node 环境正常 |
| 真实评估结果不稳定 | 中 | 评估结果可能每次不同 | 固定 seed，记录多次结果 |

### 7.2 成本风险

| 风险 | 级别 | 影响 | 缓解措施 |
|---|---|---|---|
| Embedding API 费用 | 低 | Jina/SiliconFlow 有免费额度 | 控制调用次数，用 demo data 规模 |
| LLM API 费用 | 低 | DeepSeek 价格低 | 只跑 15 个 eval cases |
| Qdrant Cloud 费用 | 低 | 免费层 1GB 足够 | demo data 规模很小 |
| 时间成本 | 中 | 环境配置可能耗时 | 设置时间上限（1 周） |

### 7.3 项目风险

| 风险 | 级别 | 影响 | 缓解措施 |
|---|---|---|---|
| 偏离 CodePilot 主线 | 中 | 在 Basjoo 上投入过多时间 | 设置明确的 v2.0 范围和时间上限 |
| 陷入环境坑 | 中 | Docker/WSL/网络问题消耗时间 | 优先用最简方案（Qdrant Cloud） |
| API Key 泄漏 | 高 | 密钥提交到 Git | .env + .gitignore，不提交 |
| 投入产出比不合理 | 中 | 花大量时间只换来小幅改进 | 设置 Go/No-Go 检查点 |

---

## 8. Go / No-Go Decision

### 结论

```
CONDITIONAL GO：只有满足以下条件才进入 v2.0-real-qdrant-eval-adapter
```

### 条件清单

| 序号 | 条件 | 当前状态 | 必须满足 |
|---|---|---|---|
| 1 | Qdrant 可用（Docker 或 Qdrant Cloud） | ❌ Docker 未安装 | ✅ 是 |
| 2 | Embedding API Key 可用 | ❌ 未申请 | ✅ 是 |
| 3 | Windows 本地运行方案确认 | ⚠️ 需要确认 Qdrant Cloud 可行性 | ✅ 是 |
| 4 | 成本可接受 | ✅ 免费额度足够 | ✅ 是 |
| 5 | 改造工作量可控（≤1 周） | ✅ 只需扩展 2 个脚本 | ✅ 是 |
| 6 | 有明确的时间上限 | ❌ 需要设定 | ✅ 是 |

### 决策理由

**为什么不是直接 GO**：
- Docker 未安装，Qdrant 部署需要额外步骤
- Embedding API Key 未申请
- WSL 状态不明
- 需要先验证 Qdrant Cloud 免费层是否可用

**为什么不是 NO-GO**：
- Phase 1 已有完整交付物，但缺少真实 RAG 验证
- 技术上可行（Qdrant Cloud + Jina/SiliconFlow 免费额度）
- 代码改动量小（只扩展 2 个脚本）
- 收益明确（mock vs real 对比报告）

**为什么是 CONDITIONAL GO**：
- 需要先解决 Qdrant 部署问题
- 需要先申请 Embedding API Key
- 需要设定时间上限（建议 1 周）
- 如果条件不满足，自动转为 NO-GO

---

## 9. Recommended Next Step

### 如果条件满足（进入 v2.0）

下一步应该是：

```
v2.0-real-qdrant-eval-adapter
```

**v2.0 最小开发边界**：

| 步骤 | 内容 | 文件 |
|---|---|---|
| 1 | 确认 Qdrant 可用 | 环境配置 |
| 2 | 配置 Embedding API Key | `.env`（不提交） |
| 3 | 扩展 seed_demo_data.py 实现 `--write-db` | `backend/scripts/seed_demo_data.py` |
| 4 | 扩展 run_rag_eval.py 添加 `--real` 模式 | `backend/scripts/run_rag_eval.py` |
| 5 | 运行 3-5 个 real retrieval eval cases | — |
| 6 | 生成 mock vs real 对比报告 | `backend/reports/` |
| 7 | 更新文档 | `backend/docs/rag-evaluation.md` |

**v2.0 不做什么**：
- 不改核心架构
- 不改前端 UI
- 不做 API-level eval（那是 v2.1 的事）
- 不做 Vercel 部署
- 不继续求职包装

**v2.0 时间上限**：1 周。如果 1 周内无法完成最小闭环，冻结项目。

### 如果条件不满足（冻结项目）

下一步应该是：

- 冻结 Basjoo 项目
- 回到 CodePilot 主线开发
- 保留 Phase 1 作为阶段性完成成果

---

## 10. Hard Boundaries

### 本轮审查边界

1. **不继续求职包装** — 开发中期不做简历话术
2. **不接真实 API Key** — 除非进入 v2.0 且有密钥安全方案
3. **不提交 .env** — API Key 永远不进 Git
4. **不改核心架构** — 不修改 backend/services/、backend/models.py、backend/api/ 等核心文件
5. **不重做 UI** — 不涉及 frontend-nextjs/
6. **不做 Vercel 部署** — 不做线上部署
7. **不夸大 mock 指标** — mock 评估结果不代表真实线上质量
8. **不为了包装而开发** — 工程目标优先于求职目标
9. **不修改 basjoo 核心源码** — 本轮原则上不修改 selected/basjoo
10. **不使用 git add . 或 git add *** — 精确提交

### v2.0 如果启动的边界

1. 只扩展 `run_rag_eval.py` 和 `seed_demo_data.py`，不改其他文件
2. API Key 只放在 `.env`，不提交
3. 1 周时间上限
4. 只做 3-5 个 real retrieval eval cases，不做全量
5. 生成 mock vs real 对比报告即为完成
6. 不继续求职包装，直到 v2.0 闭环

---

## Appendix: Basjoo RAG Architecture Reference

```
用户查询
  │
  ▼
┌─────────────────────────────────────────────────────────┐
│  endpoints.py: prepare_chat_request()                   │
│  1. 验证 agent + tenant                                  │
│  2. 调用 KbRetrievalService.retrieve()                   │
│  3. 构建 kb_context + kb_sources                         │
│  4. 如果 kb_context 为空 → 注入 fallback prompt          │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│  kb_retrieval_service.py: retrieve()                    │
│  1. 获取 embedding API key                               │
│  2. embed_texts([query]) → query_vector                  │
│  3. qdrant.search_kb(query_vector, top_k*2)              │
│  4. 按 similarity_threshold 过滤                         │
│  5. 截断到 top_k                                         │
│  6. 返回 [{text, doc_id, score, source_*}]               │
└──────────────────────┬──────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────┐
│  qdrant_service.py: search_kb()                         │
│  1. AsyncQdrantClient.search()                           │
│  2. payload filter: tenant_id + kb_id                    │
│  3. 返回 [{id, score, payload}]                          │
└─────────────────────────────────────────────────────────┘
```

---

*Document created: 2026-06-20*
*Audit version: v1.4-phase2-readiness-audit*
*Decision: CONDITIONAL GO*
*Next step: Verify Qdrant availability + Embedding API Key before proceeding to v2.0*
