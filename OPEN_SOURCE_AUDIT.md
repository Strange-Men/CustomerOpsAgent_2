# CustomerOpsAgent_2｜Open Source Customer Support Agent — 选型审查报告

> **Created**: 2026-06-19
> **Author**: Claude Code (automated audit)
> **Status**: Phase 1 Complete — 选型审查完成

---

## 1. Background

本次审查的目标是从 GitHub 上筛选适合二次开发的开源 AI 客服 / 售后客服 / RAG 客服 Agent 项目。

**核心需求**：
- AI 客服 / 售后服务场景明确
- 有 RAG / knowledge base
- 有 conversation history / memory
- 有 human handoff / agent assist
- 有测试 / harness / CI
- 项目规模适中，适合学生二开
- license 允许学习和二次开发
- 最近仍然维护
- 技术栈适合理解和修改

**排除条件**：
- 大型通用平台（如 Dify、RAGFlow）— 二开成本过高
- 已停止维护的项目
- License 不允许二开的项目
- 纯 demo 级项目，没有实际客服场景

---

## 2. Candidate Search Method

**搜索关键词**：
- AI customer support agent
- AI customer service platform
- RAG customer support
- helpdesk AI agent
- open source customer support AI
- 售后客服 AI / RAG 客服
- customer support LLM agent

**搜索渠道**：
- GitHub Search API（stargazers > 100）
- GitHub Topics: customer-support, customer-service, rag, llm, chatbot
- 人工推荐方向：TGO, Basjoo, Chatwoot AI/Captain

**筛选流程**：
1. 初筛 14 个项目
2. 通过 GitHub API 验证元数据（stars, license, update date）
3. 通过 gh CLI 获取 README 和目录结构
4. 深入审查 top 2 候选的架构、测试、RAG、前端

---

## 3. Candidate Comparison

### Tier 1: 客服场景专用项目

| 字段 | TGO | Basjoo | Agent-Desk | WiseEngage |
|---|---|---|---|---|
| **项目名** | TGO | Basjoo | Agent-Desk | WiseEngage |
| **GitHub URL** | [tgoai/tgo](https://github.com/tgoai/tgo) | [haoyiyin/basjoo](https://github.com/haoyiyin/basjoo) | [huabeitech/agent-desk](https://github.com/huabeitech/agent-desk) | [openimsdk/wiseengage](https://github.com/openimsdk/wiseengage) |
| **Star 数** | 499 | 143 | 142 | 156 |
| **最近更新** | 2026-06-19 | 2026-06-18 | 2026-06-19 | 2026-06-10 |
| **License** | Modified Apache 2.0 ¹ | **MIT** | Apache 2.0 | AGPL-3.0 |
| **技术栈** | Python/FastAPI + TS/React 19 + PostgreSQL + Redis | Python/FastAPI + Next.js 14 + PostgreSQL + Redis + Qdrant | Go + Web Frontend | Go |
| **客服场景明确** | ✅ 专用 | ✅ 专用 | ✅ 专用 | ✅ 专用 |
| **RAG** | ✅ 独立 tgo-rag 服务 | ✅ Qdrant 向量搜索 | ✅ (topics: rag, knowledge-base) | ❓ 待验证 |
| **Memory / History** | ✅ 多轮上下文 | ✅ 会话持久化 | ✅ (implied) | ✅ (IM 基础设施) |
| **Human Handoff** | ✅ Smart Handoff | ✅ 会话接管 | ✅ (stated) | ✅ (stated) |
| **测试 / Harness** | ✅ 13+ 测试文件 | ✅ 35+ 测试文件 + Playwright E2E | ❓ 待验证 | ❓ 待验证 |
| **前端后台** | ✅ React 19 Admin | ✅ Next.js 14 Admin | ❓ 待验证 | ✅ Dashboard |
| **本地运行** | ✅ Docker Compose | ✅ Docker Compose | ✅ Docker Compose | ✅ Docker Compose |
| **二开难度** | 中 | **低-中** | 中 | 高 (Go) |
| **作品集价值** | 高 | **高** | 中 | 中 |
| **风险点** | License 有商业限制；微服务架构较复杂 | 项目较新(2026-03)；社区较小 | 项目很新；Go 技术栈 | AGPL-3.0 license 限制 |

> ¹ TGO License: Modified Apache 2.0，禁止多租户商用和移除 Logo，学习二开无问题。

### Tier 2: 通用平台（可用但二开成本高）

| 字段 | FastGPT | Chatwoot | MaxKB | RAGFlow | Dify |
|---|---|---|---|---|---|
| **Stars** | 28,548 | 32,601 | 21,365 | 83,157 | 145,797 |
| **License** | Modified ² | Custom ³ | GPL-3.0 | Apache-2.0 | Apache-2.0 |
| **技术栈** | TS/Node + React + MongoDB | Ruby on Rails + Vue.js | Python + Vue.js + pgvector | Python + React + ES | Python + React + PG |
| **二开难度** | 高 | 高 | 高 | 高 | 极高 |
| **作品集价值** | 中 (太大) | 中 (非 AI 原生) | 中 | 中 | 低 (太通用) |
| **风险点** | 代码量巨大 | Ruby 技术栈不主流 | GPL-3.0 传染性 | 代码量巨大 | 定位太泛 |

> ² FastGPT License 显示为 NOASSERTION，需确认实际协议。
> ³ Chatwoot License 显示为 Other，需确认实际协议。

### 不推荐项目及原因

| 项目 | 原因 |
|---|---|
| **Dify** | 通用 AI 工作流平台，客服只是应用场景之一。代码量极大（145K stars），二开成本极高，不适合作品集展示。 |
| **RAGFlow** | RAG 引擎为主，非客服专用。83K stars 的大型项目，改一个功能需要理解整个系统。 |
| **Flowise** | 可视化 Agent 构建器，非客服专用。虽然灵活但需要自己搭建客服逻辑，工作量大。 |
| **AnythingLLM** | 本地 RAG 工具，非客服平台。缺少 human handoff、ticket 管理等客服核心功能。 |
| **LangGraph Multi-Agent CS** | 参考实现，非完整平台。缺少前端、部署、测试等工程化要素。 |

---

## 4. Selected Project

### 第一推荐：Basjoo

**GitHub**: https://github.com/haoyiyin/basjoo
**Stars**: 143
**License**: MIT
**Created**: 2026-03-28

### 第二备选：TGO

**GitHub**: https://github.com/tgoai/tgo
**Stars**: 499
**License**: Modified Apache 2.0
**Created**: 2025-11-18

---

## 5. Why This Project

### 为什么推荐 Basjoo

1. **MIT License** — 最宽松的开源协议，学习和二开完全无限制
2. **技术栈完美匹配** — Python/FastAPI + Next.js 14 + PostgreSQL + Qdrant，主流且易理解
3. **项目规模适中** — 6MB 代码库，单体架构，不像 TGO 那样有 15 个子仓库
4. **测试覆盖出色** — 35+ 后端测试文件 + Playwright E2E 测试 + 详细的测试报告
5. **文档质量极高** — 有 CLAUDE.md 和 AGENTS.md，专门为 AI 辅助开发优化
6. **客服场景专用** — 不是通用平台改装，而是从零构建的客服平台
7. **RAG 实现清晰** — Qdrant 向量搜索 + 文档解析 + chunking + embedding，流程完整
8. **Human Handoff 实现** — 会话接管功能已实现，有 E2E 测试验证
9. **Embeddable Widget** — 可嵌入的聊天组件，作品集展示效果好
10. **活跃维护** — 2026-06-18 还有更新，开发者响应迅速

### 为什么不首选 TGO

TGO 也是很好的项目，但：
- **License 有商业限制**（禁止多租户、禁止移除 Logo）
- **微服务架构较复杂**（15 个子仓库），二开需要理解多个服务间的交互
- **代码量大**（47MB），对学生来说学习曲线较陡
- **依赖 WuKongIM**，增加了部署和理解的复杂度

但 TGO 作为备选仍然有价值，特别是如果需要多渠道（微信、小程序）支持时。

---

## 6. Local Setup Findings

### Basjoo 本地运行

**方式 1: Docker Compose（推荐）**
```bash
cd candidates/basjoo
docker compose --profile dev up -d
```
- 前端: http://localhost:3000
- 后端: http://localhost:8000
- Qdrant: http://localhost:6333
- PostgreSQL: 127.0.0.1:5432
- Redis: 127.0.0.1:6379

**方式 2: 本地开发**
```bash
# 后端
cd backend
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python3 main.py

# 前端
cd frontend-nextjs
npm install
npm run dev

# Widget
cd widget
npm install
npm run dev
```

**需要的 API Key**（不填写也能启动，但 AI 功能不可用）：
- DeepSeek API Key（LLM 调用）
- Jina API Key（Embedding 调用）
- 或其他 OpenAI 兼容的 API Key

**Mock Mode**: 未发现内置 mock mode，但后端测试使用 monkeypatch 模拟 LLM 调用。

**Windows 兼容性**: 需要 Docker Desktop for Windows。本地开发需要 Python 3.11+ 和 Node.js 18+。

### TGO 本地运行

**方式: Docker Compose**
```bash
cd candidates/tgo
cp .env.dev.example .env.dev
make dev
```

**系统要求**: CPU >= 4 Core, RAM >= 8 GiB, macOS / Linux / WSL2
**注意**: TGO 官方不直接支持 Windows，需要 WSL2。

---

## 7. Architecture Summary

### Basjoo 架构

```
┌─────────────────────────────────────────────────────┐
│                    nginx (reverse proxy)              │
├─────────────────┬───────────────────────────────────┤
│   Frontend      │         Backend (FastAPI)          │
│   (Next.js 14)  │  ┌─────────────────────────────┐  │
│                 │  │  API Layer (thin routers)     │  │
│  - App Router   │  │  - /api/admin/*              │  │
│  - React 18     │  │  - /api/v1/*                 │  │
│  - TypeScript   │  └──────────┬──────────────────┘  │
│  - i18next      │             │                      │
│                 │  ┌──────────▼──────────────────┐  │
│                 │  │  Service Layer               │  │
│                 │  │  - llm_service.py            │  │
│                 │  │  - kb_service.py             │  │
│                 │  │  - qdrant_service.py         │  │
│                 │  │  - kb_document_processor.py  │  │
│                 │  │  - kb_retrieval_service.py   │  │
│                 │  │  - url_safety.py             │  │
│                 │  └──────────┬──────────────────┘  │
│                 │             │                      │
│                 │  ┌──────────▼──────────────────┐  │
│                 │  │  Data Layer                  │  │
│                 │  │  - PostgreSQL (app data)     │  │
│                 │  │  - SQLite (legacy)           │  │
│                 │  │  - Redis (cache/rate-limit)  │  │
│                 │  │  - Qdrant (vector search)    │  │
│                 │  └─────────────────────────────┘  │
├─────────────────┴───────────────────────────────────┤
│   Widget (TypeScript)           Scrapling Service    │
│   - esbuild bundle              - curl_cffi          │
│   - SSE streaming               - readability-lxml   │
│   - localStorage sessions       - Web content fetch  │
└─────────────────────────────────────────────────────┘
```

### TGO 架构（微服务）

```
┌────────────────────────────────────────────────────────────┐
│  tgo-web (React 19 Admin)    tgo-widget-js (Chat Widget)   │
├────────────────────────────────────────────────────────────┤
│                    tgo-api (FastAPI)                        │
│              Core business logic, user mgmt                 │
├──────────┬──────────┬──────────┬──────────┬────────────────┤
│ tgo-ai   │ tgo-rag  │ tgo-     │ tgo-     │ tgo-workflow   │
│ (Agent)  │ (RAG)    │ platform │ plugin-  │ (DAG Engine)   │
│          │          │ (Multi-  │ runtime  │                │
│          │          │ channel) │          │                │
├──────────┴──────────┴──────────┴──────────┴────────────────┤
│  PostgreSQL  │  Redis  │  WuKongIM  │  Celery             │
└────────────────────────────────────────────────────────────┘
```

---

## 8. RAG Summary

### Basjoo RAG 能力

| 维度 | 实现 |
|---|---|
| **文档导入** | 支持 PDF, TXT, CSV, Markdown, DOCX, XLSX；URL 爬取（Scrapling） |
| **Chunking** | `chunk_text` 函数，Recursive splitting，可配置 chunk 参数 |
| **Embedding** | OpenAI 兼容 API（Jina, SiliconFlow, 自定义） |
| **向量库** | Qdrant，per-tenant collection |
| **Retrieval** | 向量相似度搜索 + 混合搜索（hybrid_search） |
| **Citation** | 检索结果包含 source 信息 |
| **No-answer Fallback** | similarity_threshold 默认 0.01，低于阈值不返回 |
| **RAG Eval** | 无内置 RAG eval，但有相关性测试（test_chat_kb_retrieval.py） |

### TGO RAG 能力

| 维度 | 实现 |
|---|---|
| **文档导入** | Document KB, Q&A KB, Website KB |
| **Chunking** | 独立 tgo-rag 服务处理 |
| **Embedding** | 支持多种 embedding 模型 |
| **向量库** | 内置向量存储 |
| **Retrieval** | 混合语义/全文搜索 |
| **Citation** | 支持 |
| **No-answer Fallback** | 有 |
| **RAG Eval** | 有独立测试 |

---

## 9. Memory Summary

### Basjoo Memory 能力

| 维度 | 实现 |
|---|---|
| **Conversation History** | ✅ 保存在 PostgreSQL，chat sessions/messages |
| **Customer Profile** | ✅ Visitor info collection |
| **Ticket State** | ✅ Session 状态管理 |
| **Multi-turn Context** | ✅ SSE streaming + session persistence |
| **Memory Continuity Test** | ❌ 无专门测试，但 E2E 测试覆盖了会话流程 |

### TGO Memory 能力

| 维度 | 实现 |
|---|---|
| **Conversation History** | ✅ 多轮上下文 |
| **Customer Profile** | ✅ Visitor management |
| **Ticket State** | ✅ 会话状态 |
| **Multi-turn Context** | ✅ Context Memory |
| **Memory Continuity Test** | ✅ 有测试覆盖 |

---

## 10. Harness / Test Summary

### Basjoo 测试覆盖 ⭐⭐⭐⭐⭐

| 类型 | 文件数 | 覆盖范围 |
|---|---|---|
| **Unit Tests** | 10+ | models, auth, llm_service, edge_cases |
| **Integration Tests** | 5+ | kb_retrieval, chat_kb, file_upload_kb |
| **API Contract Tests** | 5+ | agents_api, v1_endpoints, auth_service |
| **Security Tests** | 3+ | security_robustness, rate_limit, url_safety |
| **Performance Tests** | 3+ | stress_load, extreme_stress, production_simulation |
| **E2E Tests** | 16 | Playwright, covering auth, chat, KB, sessions |
| **CI** | ❓ 待验证 | .github 目录存在 |

**亮点**：
- `conftest.py` 提供隔离的 SQLite 测试数据库
- `client` 和 `public_client` fixture 支持认证/非认证测试
- LLM 调用通过 monkeypatch 模拟，不需要真实 API Key
- E2E 测试报告详细记录了 BUG-001/002/003

### TGO 测试覆盖 ⭐⭐⭐⭐

| 类型 | 文件数 | 覆盖范围 |
|---|---|---|
| **Unit Tests** | 13+ | agent, chat, streaming, channels, exception |
| **Integration Tests** | ✅ | 有 tests 目录 |
| **E2E Tests** | ❓ 待验证 | - |
| **CI** | ✅ | .github 目录存在 |

**亮点**：
- AGENTS.md 定义了 `$code-change-verification` 和 `$functional-verification` 规范
- 支持 `make dev` 一键启动开发环境

---

## 11. Frontend UX Summary

### Basjoo 前端体验 ⭐⭐⭐⭐

| 维度 | 评分 | 说明 |
|---|---|---|
| **像真实客服后台** | ⭐⭐⭐⭐ | Admin Dashboard 设计专业 |
| **会话管理** | ⭐⭐⭐⭐ | Sessions 页面，支持人工接管 |
| **知识库管理** | ⭐⭐⭐⭐⭐ | URL + 文件上传，Qdrant 索引管理 |
| **Agent 配置** | ⭐⭐⭐⭐ | Playground 调试，模型/provider 配置 |
| **人工接管** | ⭐⭐⭐⭐ | 有 human takeover 功能 |
| **评估面板** | ⭐⭐ | 无专门的 eval/bad case 面板 |
| **AI Demo 感** | ⭐⭐⭐ | 有一定 demo 感，但接近产品级 |
| **作品集展示** | ⭐⭐⭐⭐ | Widget 可嵌入展示 |

### TGO 前端体验 ⭐⭐⭐⭐⭐

| 维度 | 评分 | 说明 |
|---|---|---|
| **像真实客服后台** | ⭐⭐⭐⭐⭐ | 专业级 Dashboard |
| **会话管理** | ⭐⭐⭐⭐⭐ | 完整的会话管理 |
| **知识库管理** | ⭐⭐⭐⭐⭐ | Document KB + Q&A KB + Website KB |
| **Agent 配置** | ⭐⭐⭐⭐⭐ | 多 Agent 编排 |
| **人工接管** | ⭐⭐⭐⭐⭐ | Smart Handoff |
| **评估面板** | ⭐⭐⭐ | 有使用统计 |
| **AI Demo 感** | ⭐⭐⭐⭐ | 接近产品级 |
| **作品集展示** | ⭐⭐⭐⭐⭐ | 多平台 Widget |

---

## 12. Optimization Opportunities

### 优化点 1: RAG Evaluation Harness

| 字段 | 内容 |
|---|---|
| **优化名称** | RAG Evaluation Harness |
| **当前问题** | Basjoo 有 RAG 检索功能，但没有系统化的 RAG 质量评估框架。无法量化回答质量、检索精度、幻觉率。 |
| **为什么值得做** | RAG 评估是 AI Agent 工程化的核心能力。作品集中展示"我建了一套 RAG eval harness"非常有说服力。 |
| **涉及模块** | 新增 `tests/rag_eval/` 目录；扩展 `backend/services/kb_retrieval_service.py` |
| **难度** | 中 |
| **风险** | 低（不修改核心代码） |
| **作品集亮点** | ✅ 非常适合 |
| **预计产出** | RAG eval 测试套件 + 评估报告 + 可视化 dashboard |
| **需要真实 API Key** | 需要（用于实际 RAG 调用） |
| **影响原项目主流程** | 否 |

### 优化点 2: Memory Continuity Test Suite

| 字段 | 内容 |
|---|---|
| **优化名称** | Memory Continuity Test Suite |
| **当前问题** | Basjoo 有会话持久化，但没有专门测试多轮对话的上下文连续性。 |
| **为什么值得做** | 多轮对话是客服 Agent 的核心场景。测试"用户第 3 轮提到的内容是否被记住"是工程化的重要体现。 |
| **涉及模块** | 新增 `tests/memory_continuity/` 目录 |
| **难度** | 低-中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | Memory continuity test suite + 测试报告 |
| **需要真实 API Key** | 需要 |
| **影响原项目主流程** | 否 |

### 优化点 3: Human Handoff Rules Engine

| 字段 | 内容 |
|---|---|
| **优化名称** | Human Handoff Rules Engine |
| **当前问题** | Basjoo 有 basic human takeover，但没有基于规则的自动转接（如"用户说'转人工'时自动转接"）。 |
| **为什么值得做** | 规则引擎是客服系统的标准功能，展示"我能设计规则引擎"体现系统设计能力。 |
| **涉及模块** | 新增 `backend/services/handoff_rules.py`；扩展 `backend/api/v1/endpoints.py` |
| **难度** | 中 |
| **风险** | 低-中（需要修改 chat 流程） |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | Handoff rules engine + 规则配置 UI + 测试 |
| **需要真实 API Key** | 否（可用 mock 测试） |
| **影响原项目主流程** | 轻微 |

### 优化点 4: Bad Case Dashboard

| 字段 | 内容 |
|---|---|
| **优化名称** | Bad Case Dashboard |
| **当前问题** | 没有专门的 bad case 收集和分析面板。无法系统性地发现和修复 AI 回答问题。 |
| **为什么值得做** | Bad case 管理是 AI 产品化的关键环节。展示"我能建 bad case 发现-修复闭环"非常有价值。 |
| **涉及模块** | 新增 `frontend-nextjs/src/views/BadCases/`；新增 `backend/api/v1/badcase_endpoints.py` |
| **难度** | 中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 非常适合 |
| **预计产出** | Bad case 收集 + 标注 + 分析 dashboard |
| **需要真实 API Key** | 否（可用 mock 数据） |
| **影响原项目主流程** | 否 |

### 优化点 5: Knowledge Base Quality Checker

| 字段 | 内容 |
|---|---|
| **优化名称** | Knowledge Base Quality Checker |
| **当前问题** | 知识库文档导入后没有质量检查（如重复检测、覆盖率分析、过期检测）。 |
| **为什么值得做** | KB 质量直接影响 RAG 效果。展示"我能做 KB 质量治理"体现数据工程能力。 |
| **涉及模块** | 新增 `backend/services/kb_quality.py` |
| **难度** | 中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | KB 质量检查工具 + 报告 |
| **需要真实 API Key** | 部分需要（embedding 用于相似度检测） |
| **影响原项目主流程** | 否 |

### 优化点 6: Reply Citation / Evidence Trace

| 字段 | 内容 |
|---|---|
| **优化名称** | Reply Citation / Evidence Trace |
| **当前问题** | Basjoo 的 RAG 检索返回 source 信息，但前端展示不够清晰，没有引用标注。 |
| **为什么值得做** | 引用溯源是 RAG 系统可信度的关键。展示"我能做 citation rendering"体现前端+RAG 综合能力。 |
| **涉及模块** | 修改 `widget/src/BasjooWidget.tsx`；新增 citation 组件 |
| **难度** | 低-中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | Citation rendering 组件 + 测试 |
| **需要真实 API Key** | 需要（用于实际 RAG 调用） |
| **影响原项目主流程** | 否 |

### 优化点 7: No-answer Fallback 增强

| 字段 | 内容 |
|---|---|
| **优化名称** | No-answer Fallback 增强 |
| **当前问题** | Basjoo 的 similarity_threshold 是静态配置，没有基于置信度的动态 fallback 策略。 |
| **为什么值得做** | 智能 fallback（如"我不确定，让我转接人工"）是客服 Agent 的必备能力。 |
| **涉及模块** | 修改 `backend/services/kb_retrieval_service.py`；新增 fallback 策略 |
| **难度** | 低-中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | 动态 fallback 策略 + 测试 |
| **需要真实 API Key** | 需要 |
| **影响原项目主流程** | 轻微 |

### 优化点 8: Demo Data / Mock Mode

| 字段 | 内容 |
|---|---|
| **优化名称** | Demo Data / Mock Mode |
| **当前问题** | 没有内置 demo 数据。新用户启动后需要手动配置 agent、上传文档才能体验。 |
| **为什么值得做** | 一键 demo 是作品集展示的关键。面试官点开就能看到完整功能，而不是空白页面。 |
| **涉及模块** | 新增 `scripts/seed_demo.py`；修改 `docker-compose.yml` |
| **难度** | 低 |
| **风险** | 低 |
| **作品集亮点** | ✅ 非常适合 |
| **预计产出** | Seed data 脚本 + demo 配置 |
| **需要真实 API Key** | 否（可用 mock 数据） |
| **影响原项目主流程** | 否 |

### 优化点 9: Frontend UX — Bad Case 标注面板

| 字段 | 内容 |
|---|---|
| **优化名称** | Bad Case 标注面板 |
| **当前问题** | 没有让用户标注"这个回答不好"的功能。无法收集用户反馈来改进 RAG。 |
| **为什么值得做** | 用户反馈闭环是 AI 产品化的关键。展示"我能做 feedback-driven RAG improvement"。 |
| **涉及模块** | 修改 `widget/src/BasjooWidget.tsx`；新增 `backend/models.py` feedback 模型 |
| **难度** | 低-中 |
| **风险** | 低 |
| **作品集亮点** | ✅ 适合 |
| **预计产出** | 👍/👎 反馈组件 + 反馈数据 dashboard |
| **需要真实 API Key** | 否 |
| **影响原项目主流程** | 否 |

### 优化点 10: Windows Dev Experience 修复

| 字段 | 内容 |
|---|---|
| **优化名称** | Windows Dev Experience 修复 |
| **当前问题** | Basjoo 的文档主要针对 Linux/macOS，Windows 开发体验没有专门优化。 |
| **为什么值得做** | 改善 Windows 开发者体验，扩大贡献者基础。 |
| **涉及模块** | 修改 `docker-compose.yml`、`CLAUDE.md`、`AGENTS.md` |
| **难度** | 低 |
| **风险** | 低 |
| **作品集亮点** | ⭐ 一般 |
| **预计产出** | Windows 开发指南 + Docker Compose 适配 |
| **需要真实 API Key** | 否 |
| **影响原项目主流程** | 否 |

---

## 13. Recommended Phase 1 Enhancement

### 🎯 Phase 1: RAG Evaluation Harness + Demo Data

**任务名称**: RAG Evaluation Harness with Demo Data

**为什么选它**:
1. **不破坏原项目** — 纯新增测试和脚本，不修改核心代码
2. **短期可展示** — 1-2 周可完成 MVP
3. **体现核心能力** — RAG eval 是 AI Agent 工程化的核心技能
4. **不依赖真实 API Key** — 可用 mock 数据 + 少量真实调用
5. **有测试验收方式** — eval 指标可量化
6. **能写进简历** — "Built RAG evaluation harness for open-source customer support platform"

**目标效果**:
- 一套 RAG eval 测试框架，包含：
  - 问答对测试集（golden QA pairs）
  - 检索精度评估（precision@k, recall@k, MRR）
  - 回答质量评估（faithfulness, relevance, hallucination rate）
  - 自动生成评估报告（JSON + Markdown）
- 一套 demo seed data：
  - 预配置的 agent（3 个不同场景）
  - 预上传的知识库文档（产品手册、FAQ）
  - 预设的对话示例

**修改范围**:
```
新增：
  tests/rag_eval/
    ├── conftest.py              # 测试配置
    ├── test_retrieval_quality.py # 检索质量测试
    ├── test_answer_quality.py   # 回答质量测试
    ├── test_hallucination.py    # 幻觉检测
    ├── golden_qa_pairs.json     # 测试数据集
    └── report_generator.py      # 报告生成

  scripts/
    ├── seed_demo.py             # Demo 数据种子脚本
    └── demo_data/               # Demo 数据文件

修改：
  无（或仅修改 README 添加使用说明）
```

**验收标准**:
1. `pytest tests/rag_eval/` 全部通过
2. 生成 RAG eval 报告（JSON + Markdown）
3. Demo seed data 脚本可一键运行
4. 报告包含：precision, recall, MRR, faithfulness, hallucination rate
5. 不修改任何原有代码

**风险**:
- 需要真实的 embedding API 来测试 RAG 检索质量
- 可以用 mock 模式先跑通框架，再用真实 API 验证

**后续扩展方向**:
1. Memory Continuity Test — 测试多轮对话的上下文连续性
2. Bad Case Dashboard — 可视化 bad case 分析
3. Human Handoff Rules — 规则引擎
4. Reply Citation — 引用渲染组件

---

## 14. License and Risk Notes

### Basjoo License: MIT ✅

```
MIT License
Copyright (c) 2026 haoyiyin
```

- ✅ 可以自由使用、修改、分发
- ✅ 可以用于商业项目
- ✅ 可以作为作品集展示
- ✅ 可以闭源二开
- ⚠️ 需要保留原始版权声明

### TGO License: Modified Apache 2.0 ⚠️

```
Modified Apache License 2.0 with additional conditions:
1a. Multi-tenant: 不允许多租户商用（除非获得书面授权）
1b. Logo: 不允许移除前端 Logo（不涉及前端的使用不受限）
2a. 贡献者同意协议可调整
2b. 贡献代码可被用于商业目的
```

- ✅ 学习和二开无问题
- ✅ 可以作为作品集展示
- ⚠️ 不能用于多租户 SaaS 服务
- ⚠️ 不能移除 Logo

### 风险总结

| 风险 | 级别 | 缓解措施 |
|---|---|---|
| Basjoo 项目较新，社区小 | 中 | 代码质量高，文档完善，风险可控 |
| 需要外部 API Key | 低 | 可用 mock 模式 + 少量真实调用 |
| Windows 开发体验 | 低 | Docker Desktop for Windows |
| 二开后上游更新 | 低 | Git fork + 定期 merge |

---

## 15. Next Step Prompt

### 下一步建议

1. **将 Basjoo 移到 `selected/` 目录**
   ```bash
   mv candidates/basjoo selected/basjoo
   ```

2. **尝试 Docker Compose 启动**
   ```bash
   cd selected/basjoo
   docker compose --profile dev up -d
   ```

3. **运行现有测试**
   ```bash
   cd selected/basjoo/backend
   pip install -r requirements.txt
   pytest tests/ --ignore=tests/integration/ -v
   ```

4. **开始 Phase 1: RAG Evaluation Harness**
   - 创建 `tests/rag_eval/` 目录
   - 编写 golden QA pairs
   - 实现 retrieval quality tests
   - 实现 answer quality tests
   - 创建 demo seed data

5. **文档更新**
   - 在 `OPEN_SOURCE_AUDIT.md` 中记录运行结果
   - 更新 `README_WORKSPACE.md` 记录选定项目

---

## 附录: 候选项目完整列表

| # | 项目 | Stars | License | 客服专用 | RAG | 二开难度 | 推荐 |
|---|---|---|---|---|---|---|---|
| 1 | **Basjoo** | 143 | MIT | ✅ | ✅ | 低-中 | ⭐ 第一推荐 |
| 2 | **TGO** | 499 | Mod Apache 2.0 | ✅ | ✅ | 中 | ⭐ 第二备选 |
| 3 | Agent-Desk | 142 | Apache 2.0 | ✅ | ✅ | 中 | 待验证 |
| 4 | WiseEngage | 156 | AGPL-3.0 | ✅ | ❓ | 高 | License 限制 |
| 5 | FastGPT | 28,548 | ❓ | 部分 | ✅ | 高 | 太大 |
| 6 | Chatwoot | 32,601 | ❓ | ✅ | ✅ | 高 | Ruby 栈 |
| 7 | MaxKB | 21,365 | GPL-3.0 | 部分 | ✅ | 高 | GPL 限制 |
| 8 | RAGFlow | 83,157 | Apache 2.0 | 部分 | ✅ | 高 | RAG 引擎 |
| 9 | Dify | 145,797 | Apache 2.0 | 部分 | ✅ | 极高 | 太通用 |
| 10 | Flowise | 53,716 | Apache 2.0 | 部分 | ✅ | 高 | 通用平台 |

---

## 16. Selected Project Deep Audit

> **Audit date**: 2026-06-19
> **Audited by**: Claude Code (deep code review)
> **Project**: Basjoo (haoyiyin/basjoo)
> **Clone type**: Shallow clone (1 commit visible)

### 16.1 Final Selected Project

| 字段 | 值 |
|---|---|
| **项目名** | Basjoo |
| **GitHub** | https://github.com/haoyiyin/basjoo |
| **License** | MIT (Copyright (c) 2026 haoyiyin) |
| **技术栈** | Python 3.11+ / FastAPI + TypeScript / Next.js 14 + PostgreSQL + Qdrant + Redis |
| **代码规模** | 7.2MB, 96 Python files, 109 TS/TSX files |
| **最近更新** | 2026-06-16 (shallow clone, commit 6939926) |
| **维护状态** | ✅ 活跃维护 |
| **二开适合度** | ⭐⭐⭐⭐⭐ 非常适合 |

### 16.2 Architecture Summary

```
basjoo/
├── backend/                    # FastAPI 后端 (96 .py files)
│   ├── api/v1/                 # API 路由层
│   │   ├── endpoints.py        # 主聊天/配置 API
│   │   ├── kb_document_endpoints.py  # 知识库文档 API
│   │   ├── schemas.py          # Pydantic 模型
│   │   └── sse_utils.py        # SSE 流式工具
│   ├── services/               # 业务逻辑层
│   │   ├── llm_service.py      # LLM 抽象 (889 行, 支持 OpenAI/DeepSeek/Anthropic/Gemini/Mock)
│   │   ├── kb_retrieval_service.py   # RAG 检索 (134 行)
│   │   ├── kb_document_processor.py  # 文档处理: parse→chunk→embed→Qdrant (258 行)
│   │   ├── kb_service.py       # 知识库管理 (442 行)
│   │   ├── qdrant_service.py   # Qdrant 向量操作 (155 行)
│   │   ├── document_parser.py  # 文档解析+chunking+embedding (232 行)
│   │   ├── url_safety.py       # SSRF 防护
│   │   └── auth_service.py     # JWT 认证
│   ├── models.py               # SQLAlchemy 模型
│   ├── tests/                  # 35+ 测试文件
│   └── main.py                 # FastAPI 入口
├── frontend-nextjs/            # Next.js 14 前端 (109 .ts/.tsx files)
│   ├── app/                    # App Router 路由
│   ├── src/views/              # 页面组件
│   │   ├── Dashboard.tsx       # 仪表盘
│   │   ├── Playground.tsx      # AI 调试台
│   │   ├── Sessions.tsx        # 会话管理 (含人工接管)
│   │   ├── URLManagement.tsx   # URL 知识管理
│   │   ├── FileUploadManagement.tsx  # 文件知识管理
│   │   ├── KnowledgeBaseSetup.tsx    # 知识库配置
│   │   ├── AgentSettings.tsx   # Agent 设置
│   │   └── AdminUsers.tsx      # 用户管理
│   └── src/services/api.ts     # API 客户端 + SSE 解析
├── widget/                     # 可嵌入聊天组件
│   └── src/BasjooWidget.tsx    # 自包含 Widget (2064 行)
├── scrapling-service/          # Web 内容抓取微服务
├── tests/e2e/                  # Playwright E2E 测试 (16 specs)
├── docker-compose.yml          # Docker 编排 (dev/prod profiles)
└── nginx/                      # 反向代理配置
```

### 16.3 RAG Deep Audit

| 维度 | 实现 | 代码位置 | 评分 |
|---|---|---|---|
| **文档导入** | PDF, TXT, CSV, Markdown, DOCX, XLSX + URL 爬取 | `kb_document_endpoints.py`, `document_parser.py` | ⭐⭐⭐⭐⭐ |
| **Chunking** | `chunk_text()`, recursive splitting, 可配置 chunk_size/chunk_overlap | `document_parser.py` | ⭐⭐⭐⭐ |
| **Embedding** | OpenAI 兼容 API (Jina, SiliconFlow, 自定义), 独立于 chat provider | `kb_document_processor.py:42-62` | ⭐⭐⭐⭐ |
| **Vector DB** | Qdrant, per-tenant collection 隔离, batch upsert | `qdrant_service.py` | ⭐⭐⭐⭐⭐ |
| **Retrieval** | 向量相似度搜索 + threshold 过滤 + top_k 截断 | `kb_retrieval_service.py:25-134` | ⭐⭐⭐⭐ |
| **Citation** | 检索结果含 source_type, source_url, source_title, filename | `kb_retrieval_service.py:119-125` | ⭐⭐⭐⭐ |
| **No-answer Fallback** | similarity_threshold 默认 0.6, agent 可配置, 低于阈值不返回 | `kb_retrieval_service.py:100-107` | ⭐⭐⭐ |
| **RAG Eval** | ❌ 无专门 RAG eval harness, 但有 `test_chat_kb_retrieval.py` 验证检索集成 | — | ⭐⭐ |
| **Hybrid Search** | Qdrant 支持 hybrid_search, 需在 search_settings 中启用 | `qdrant_service.py` | ⭐⭐⭐ |

**RAG 流程**:
```
文档上传 → document_parser.parse_with_retry() → chunk_text()
→ embed_texts() [Jina/SiliconFlow API] → Qdrant batch_upsert_points()
                                          ↓
用户查询 → embed_texts([query]) → Qdrant search_kb()
→ threshold 过滤 → top_k 截断 → 返回 [{text, doc_id, score, source}]
```

**关键发现**:
- RAG 管道完整且清晰: parse → chunk → embed → store → retrieve → filter
- 多租户隔离: per-tenant Qdrant collection + payload filter
- 无 RAG eval 框架: 缺少 precision/recall/hallucination 量化评估
- similarity_threshold 默认值较高 (0.6), 可能导致低相关性内容被过滤
- 无 dynamic fallback: 低于阈值时静默返回空, 没有 "我不确定" 提示

### 16.4 Memory Deep Audit

| 维度 | 实现 | 评分 |
|---|---|---|
| **Conversation History** | ✅ PostgreSQL/SQLite 持久化, ChatSession + ChatMessage 模型 | ⭐⭐⭐⭐ |
| **Multi-turn Context** | ✅ SSE streaming + session persistence, 支持连续对话 | ⭐⭐⭐⭐ |
| **Customer Profile** | ✅ Visitor info (country, city), visitor_id 持久化 | ⭐⭐⭐ |
| **Ticket State** | ✅ Session 状态: active, taken_over, closed | ⭐⭐⭐⭐ |
| **Memory Continuity Test** | ❌ 无专门的多轮上下文连续性测试 | ⭐⭐ |
| **Session Timeout** | ✅ 30 分钟不活动自动关闭 (APScheduler) | ⭐⭐⭐⭐ |

**Memory 模型**:
- `ChatSession`: 会话记录, 含 visitor_id, status, message_count
- `ChatMessage`: 消息记录, 含 role, content, sources (JSON)
- Widget 使用 localStorage 持久化 visitor_id 和 session_id
- 人工接管通过 WebSocket 推送消息

**关键发现**:
- Memory 是基础的会话持久化, 不是高级的 customer context memory
- 无 customer profile 聚合 (如购买历史、偏好)
- 无跨会话记忆 (新会话不记得之前的对话)
- 无 memory continuity test 验证多轮上下文是否正确传递

### 16.5 Harness / Test Deep Audit

| 类型 | 文件数 | 覆盖范围 | 评分 |
|---|---|---|---|
| **Unit Tests** | 10+ | models, auth, llm_service, edge_cases, migrations | ⭐⭐⭐⭐ |
| **Integration Tests** | 5+ | kb_retrieval, chat_kb, file_upload_kb, kb_data_layer | ⭐⭐⭐⭐ |
| **API Contract Tests** | 5+ | agents_api, v1_endpoints, auth_service, agent_membership | ⭐⭐⭐⭐ |
| **Security Tests** | 3+ | security_robustness, rate_limit, url_safety | ⭐⭐⭐⭐⭐ |
| **Performance Tests** | 3+ | stress_load, extreme_stress, production_simulation | ⭐⭐⭐⭐ |
| **E2E Tests** | 16 specs | Playwright: auth, chat, KB, sessions, widget | ⭐⭐⭐ |
| **Frontend Tests** | 1+ | Vitest: URLManagement.test.tsx | ⭐⭐ |
| **CI** | ❓ 未发现 .github/workflows | — | ⭐ |

**测试基础设施亮点**:
- `conftest.py` 提供隔离的 SQLite 测试数据库 (`.pytest_dbs/`)
- `client` / `public_client` / `support_client` / `readonly_client` fixture 支持多角色测试
- LLM 调用通过 `MockLLMService` monkeypatch, 不需要真实 API Key
- E2E 测试报告详细记录了 BUG-001 到 BUG-007

**E2E 测试问题** (来自 E2E_TEST_REPORT.md):
- 6/16 测试失败, 主要原因:
  - BUG-001/002: QA batch_import/list 端点不存在
  - BUG-003: 根路由重定向到 agent selector, E2E 导航失败
  - BUG-004/005/006: UI 选择器不匹配
- 这些问题是 E2E 测试与当前架构不同步, 不是核心功能缺陷

### 16.6 Frontend UX Audit

| 维度 | 评分 | 说明 |
|---|---|---|
| **像真实客服后台** | ⭐⭐⭐⭐ | Dashboard + Agent Panel 设计专业, 有 Admin/User 角色 |
| **会话管理** | ⭐⭐⭐⭐ | Sessions 页面支持状态过滤 (active/taken_over/closed) |
| **知识库管理** | ⭐⭐⭐⭐⭐ | URL + 文件上传, Qdrant 索引, chunk 参数可配置 |
| **Agent 配置** | ⭐⭐⭐⭐ | Playground 调试, 模型/provider/temperature 配置 |
| **人工接管** | ⭐⭐⭐⭐ | WebSocket 推送, 支持接管后发送消息 |
| **评估面板** | ⭐ | ❌ 无 bad case / eval 面板 |
| **AI Demo 感** | ⭐⭐⭐ | 有一定 demo 感, 但接近产品级 |
| **作品集展示** | ⭐⭐⭐⭐ | Widget 可嵌入, 有截图, 有中英文文档 |

**前端路由结构**:
```
/ → Agent Selector (Agents.tsx)
/login → Login
/register → Register
/agents/[agentId]/dashboard → Dashboard
/agents/[agentId]/playground → Playground
/agents/[agentId]/sessions → Sessions
/agents/[agentId]/urls → URL Management
/agents/[agentId]/files → File Upload
/agents/[agentId]/settings → Agent Settings
/users → Admin Users
```

**关键发现**:
- 前端路由需要 agent context, 根路由重定向到 agent selector
- 有 i18n 支持 (en-US, zh-CN)
- Widget 是自包含的 TypeScript 组件, 可独立嵌入
- 无 bad case 收集/标注/分析面板

### 16.7 Local Setup Risk

| 风险项 | 级别 | 说明 |
|---|---|---|
| **Docker Desktop 依赖** | 低 | Windows 需要 Docker Desktop, WSL2 |
| **API Key 需求** | 低 | 不填写也能启动, AI 功能不可用但不影响测试 |
| **端口冲突** | 低 | 默认端口 3000/8000/6333/5432/6379 |
| **Windows 兼容性** | 低-中 | 主要通过 Docker 运行, 本地开发需要 Python 3.11+ 和 Node.js 18+ |
| **依赖下载量** | 中 | 后端 ~55 个 pip 包, 前端 Next.js + Playwright |
| **缓存位置** | 低 | Docker volumes 在 Docker Desktop 管理, 不影响 C 盘 |

### 16.8 License Risk

**MIT License** — 无风险 ✅

```
MIT License
Copyright (c) 2026 haoyiyin
```

- ✅ 可以自由使用、修改、分发
- ✅ 可以用于商业项目
- ✅ 可以作为作品集展示
- ✅ 可以闭源二开
- ✅ 可以 fork 后修改并公开
- ⚠️ 需要保留原始版权声明 (在 fork 中)

---

## 17. Phase 1 Enhancement Plan

### 🎯 Recommended: RAG Evaluation Harness with Demo Data

**任务名称**: RAG Evaluation Harness + Seed Demo Data

**Why this task**:
1. **填补核心空白** — Basjoo 有完整的 RAG 管道, 但没有任何 RAG 质量评估框架
2. **不破坏原项目** — 纯新增 `tests/rag_eval/` 和 `scripts/seed_demo.py`, 不修改核心代码
3. **不依赖真实 API Key** — 可用 MockLLMService 跑通框架, 少量真实调用验证
4. **短期可完成** — 1-2 周可出 MVP
5. **体现核心能力** — RAG eval 是 AI Agent 工程化的核心技能
6. **作品集亮点** — "Built RAG evaluation harness for open-source customer support platform"

**当前问题**:
- Basjoo 的 RAG 检索有 `test_chat_kb_retrieval.py` 验证集成, 但没有量化评估
- 无法回答: "RAG 回答的准确率是多少? 幻觉率是多少? 检索精度是多少?"
- 新用户启动后是空白页面, 需要手动配置 agent/上传文档才能体验

**修改范围**:
```
新增 (不修改任何已有文件):
  tests/rag_eval/
    ├── conftest.py                 # RAG eval 测试配置
    ├── test_retrieval_precision.py # 检索精度: precision@k, recall@k, MRR
    ├── test_answer_faithfulness.py # 回答忠实度: answer 是否基于 retrieved context
    ├── test_hallucination.py       # 幻觉检测: answer 是否包含未检索到的信息
    ├── test_no_answer_fallback.py  # 无答案场景: 低相关性时是否正确返回空
    ├── golden_qa_pairs.json        # 测试数据集: 问答对 + expected sources
    └── report_generator.py         # 评估报告生成 (JSON + Markdown)

  scripts/
    ├── seed_demo.py                # Demo 数据种子脚本
    └── demo_data/
        ├── agents.json             # 3 个预配置 agent (不同场景)
        ├── knowledge/              # 示例知识库文档
        │   ├── product_faq.md      # 产品 FAQ
        │   ├── return_policy.md    # 退换货政策
        │   └── troubleshooting.md  # 故障排除
        └── conversations.json      # 预设对话示例
```

**实现思路**:
1. 利用已有的 `MockLLMService` 和 `conftest.py` fixture 搭建测试框架
2. 创建 `golden_qa_pairs.json` 包含测试问答对和预期检索来源
3. 实现 retrieval quality tests: 给定 query, 验证 retrieved chunks 是否包含 expected sources
4. 实现 answer faithfulness tests: 给定 query + retrieved context, 验证 answer 是否忠实于 context
5. 实现 hallucination tests: 验证 answer 不包含 retrieved context 之外的事实性声明
6. 实现 no-answer fallback tests: 验证低相关性查询返回空结果
7. 创建 report_generator 汇总评估指标
8. 创建 seed_demo.py 一键填充 demo 数据

**验收标准**:
1. `pytest tests/rag_eval/ -v` 全部通过 (mock mode)
2. 生成 RAG eval 报告 (`rag_eval_report.json` + `rag_eval_report.md`)
3. 报告包含: precision@k, recall@k, MRR, faithfulness score, hallucination rate, no-answer accuracy
4. `python scripts/seed_demo.py` 可一键运行, 填充 demo agent + knowledge + conversations
5. 不修改任何原有代码文件
6. 有 README 说明如何运行

**风险**:
| 风险 | 级别 | 缓解 |
|---|---|---|
| Mock mode 无法验证真实 RAG 质量 | 低 | 框架先跑通, 后续用真实 API 验证 |
| golden_qa_pairs 数据量不足 | 低 | 先做 10-20 对, 后续扩展 |
| 与现有测试框架集成 | 低 | 复用 conftest.py fixture |

**是否需要 fork**: ✅ 是 (见 Git Strategy)
**是否需要真实 API Key**: 否 (mock mode 可跑通框架)
**是否适合作品集**: ⭐⭐⭐⭐⭐ 非常适合

---

## 18. Git Strategy for Secondary Development

### 推荐方案: Fork + Branch

**方案 A: Fork 原项目, 在 fork 分支上做二开** ✅ 推荐

```
1. 在 GitHub fork haoyiyin/basjoo → Strange-Men/basjoo
2. clone fork 到 selected/basjoo:
   git clone https://github.com/Strange-Men/basjoo.git selected/basjoo
3. 设置 remote:
   cd selected/basjoo
   git remote add upstream https://github.com/haoyiyin/basjoo.git
4. 创建二开分支:
   git checkout -b phase1-rag-eval-harness
5. 在分支上开发, 完成后 PR 到自己的 fork main
```

**Remote 命名**:
- `origin` → `https://github.com/Strange-Men/basjoo` (你的 fork)
- `upstream` → `https://github.com/haoyiyin/basjoo` (原项目)

**Branch 命名**:
- `phase1-rag-eval-harness` — Phase 1 RAG 评估框架
- `phase1-seed-demo` — Phase 1 Demo 数据 (可合并到同一分支)
- 后续: `phase2-memory-continuity`, `phase3-human-handoff-rules`

**为什么不提交 candidates/selected 到外层仓库**:
- CustomerOpsAgent_2 外层仓库只管理审查文档和二开规划
- candidates/ 和 selected/ 包含外部项目源码, 不属于本项目
- 避免 license 混乱 (不同项目不同 license)
- 避免仓库膨胀 (Basjoo 7.2MB, TGO 更大)

**CustomerOpsAgent_2 外层仓库用途**:
- 记录选型审查文档 (OPEN_SOURCE_AUDIT.md)
- 记录工作区说明 (README_WORKSPACE.md)
- 记录二开计划和进度
- 不包含任何候选项目源码

**为什么不选方案 B (本地二开, 不推远程)**:
- 无法展示给面试官/他人
- 没有 Git 历史记录
- 不适合作品集展示
- 无法协作

---

## 19. Phase 0 Preparation Status

### 19.1 Source Code Status

| 项目 | 路径 | Remote | 状态 |
|---|---|---|---|
| **外层仓库** | `D:\Claude_workfile\CustomerOpsAgent_2` | https://github.com/Strange-Men/CustomerOpsAgent_2.git | ✅ 正常 |
| **原项目副本** | `D:\Claude_workfile\CustomerOpsAgent_2\candidates\basjoo` | https://github.com/haoyiyin/basjoo.git | ✅ 已下载 |
| **Fork 项目** | `D:\Claude_workfile\CustomerOpsAgent_2\selected\basjoo` | https://github.com/Strange-Men/basjoo.git | ✅ 已 clone (commit 6939926) |

### 19.2 Fork 状态

| 检查项 | 状态 | 说明 |
|---|---|---|
| **原项目可访问** | ✅ | https://github.com/haoyiyin/basjoo 可访问 |
| **Fork 存在** | ✅ | https://github.com/Strange-Men/basjoo 已创建 |
| **selected/basjoo 已 clone** | ✅ | 已 clone，commit 6939926 |
| **upstream 已设置** | ✅ | upstream → haoyiyin/basjoo |
| **v0.1-fork-baseline tag** | ✅ | 已创建 (local, not pushed) |

**Fork baseline 建立于 2026-06-19**:
- origin → Strange-Men/basjoo
- upstream → haoyiyin/basjoo
- 当前分支: main, commit 6939926

### 19.3 Phase 0 准备结论

**9-Step Preparation Checklist**: ✅ 全部通过

1. ✅ 明确原始想法 — 基于 Basjoo 做 RAG Eval Harness
2. ✅ 想法压力测试 — 真实有价值、短期可完成、适合简历
3. ✅ 想法判断结论 — 非常值得做
4. ✅ 初版二开 PRD — RAG Eval Harness + Demo Data
5. ✅ PRD 追问对齐 — 范围合理、不破坏原项目
6. ✅ 最终 PRD 固化 — Phase 1 范围明确
7. ✅ 设计基调确定 — 纯新增、不改 UI/架构
8. ✅ 文件结构规划 — tests/rag_eval/ + scripts/
9. ✅ 验收流程规划 — pytest + eval report + seed demo

**Fork baseline**: ✅ 已建立 (v0.1-fork-baseline, 2026-06-19)

### 19.4 版本规则

**外层 CustomerOpsAgent_2 文档仓库**:
- v0.1-audit：选型审查 ✅
- v0.2-deep-audit：深度审查 ✅
- v0.3-phase0-prep：二开准备完成 ✅
- 不进入 v1 (外层仓库不承载二开代码)

**Basjoo fork 仓库**:
- v0.1-fork-baseline：fork 后原始基线
- v0.2-setup-verified：本地运行和测试验证完成
- v0.3-phase1-plan：Phase 1 计划锁定
- v1.0-rag-eval-harness：RAG eval harness 实现完成
- v1.1-demo-data：demo data 完成
- v1.2-docs-and-report：文档与评估报告完成
- v1.3-phase1-complete：Phase 1 完整验收

**规则**:
- 准备阶段全部 v0
- 第一行代码二开开始进入 v1
- 不允许在 v0 阶段写功能代码
- v1 之后每个版本必须有可运行验收结果

---

*Report updated: 2026-06-19*
*Phase 0 preparation confirmed*
*Fork baseline: v0.1-fork-baseline (2026-06-19)*
*Deep audit by: Claude Code*
*Workspace: D:\Claude_workfile\CustomerOpsAgent_2*
