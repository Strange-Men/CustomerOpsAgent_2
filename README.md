# CustomerOpsAgent_2｜开源 AI 客服系统二次开发与 RAG 评估增强

## 一句话介绍

这是一个基于开源 AI 客服系统 [Basjoo](https://github.com/haoyiyin/basjoo) 的**二次开发项目**，不是从 0 重写客服 Agent。项目重点是为现有 AI 客服系统补充 **RAG Evaluation Harness**、**Demo Data**、**评估报告**和**工程可验证能力**。

---

## 当前工程状态

本项目当前完成的是 **Phase 1：Mock-friendly RAG Evaluation Harness Enhancement**。
它不是完整 AI 客服系统，也不是线上生产级 RAG 评估。当前成果主要用于验证评估框架、测试基线、Demo Data 和报告生成能力。

后续如果继续开发，将先进入 v1.4 Phase 2 Readiness Audit，而不是直接继续做简历包装或盲目接入真实服务。

**v1.6.1 环境验证（2026-06-20）**：环境验证完成，结论为 NOT READY FOR v2.0。Docker Desktop 未安装，SiliconFlow API Key 未配置。详见 [PHASE2_ENV_VERIFICATION.md](./PHASE2_ENV_VERIFICATION.md)。

详细边界定义见 [ENGINEERING_BOUNDARY.md](./ENGINEERING_BOUNDARY.md)，后续工程计划见 [NEXT_ENGINEERING_PLAN.md](./NEXT_ENGINEERING_PLAN.md)。

---

## 如果你是 HR / 面试官

建议优先查看：

1. **本仓库 README.md** — 项目全貌和成果概览
2. **[CAREER_PACKAGE.md](./CAREER_PACKAGE.md)** — 求职包装、简历写法、面试话术（Phase 1 完成后的附属材料，不是工程主线）
3. **[basjoo 二开代码仓库 ENHANCEMENT_SUMMARY.zh-CN.md](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/ENHANCEMENT_SUMMARY.zh-CN.md)** — 中文增强说明
4. **[backend/reports/rag_eval_report.md](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/reports/rag_eval_report.md)** — 评估报告

---

## 为什么这个仓库看起来不放源码？

你可能注意到了：这个仓库（CustomerOpsAgent_2）里没有 Python 代码、没有前端代码、没有 `backend/` 目录。

**原因是：我用了两个仓库来管理这个项目。**

| 仓库 | 用途 | 地址 |
|---|---|---|
| **CustomerOpsAgent_2**（本仓库） | 项目管理、选型审查、阶段计划、进度文档 | [Strange-Men/CustomerOpsAgent_2](https://github.com/Strange-Men/CustomerOpsAgent_2) |
| **basjoo**（二开仓库） | 真正的代码改动、测试、脚本、报告 | [Strange-Men/basjoo](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness) |

**为什么要这样做？**

1. **保留 Basjoo fork 的原始 Git 历史** — basjoo 是从 `haoyiyin/basjoo` fork 来的，有自己的 commit 历史和版本管理
2. **避免把完整开源项目源码塞进管理仓库** — Basjoo 有 7.2MB 代码、96 个 Python 文件、109 个 TypeScript 文件，不适合放进另一个仓库
3. **外层仓库只放选型、审查、计划、阶段总结和项目说明** — 方便 HR / 面试官快速了解项目全貌
4. **真正代码改动在 basjoo fork 的 `phase1-rag-eval-harness` 分支** — 所有二开代码都在这里

**👉 真正的代码仓库在这里：https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness**

---

## 我具体做了什么

### RAG Evaluation Harness

- 设计并实现了 **RAG 评估测试框架**，覆盖检索精度、无答案回退、证据引用、幻觉风险等维度
- **Mock Embedding**：基于字符频率的向量生成（256 维），无需真实 API Key
- **Mock Retriever**：基于内存的余弦相似度搜索，无需 Qdrant
- **Mock RAG Pipeline**：从检索结果中提取答案，无需调用 LLM
- **15 个 eval cases**：覆盖正常命中、多文档检索、无答案 fallback、低相关度拒答、evidence/citation 校验、幻觉风险
- **37 个 pytest 测试**：全部通过
- 评估指标：Precision@k、Recall@k、MRR、No-Answer Accuracy、Citation Accuracy、Hallucination Risk

### SmartHome Support Demo Data

- **2 个 Demo Agent**：通用客服 + 退换货专员，含 system prompt 和升级策略
- **3 篇知识库文档**：产品 FAQ、退换货政策、故障排除指南
- **15 个 Demo 问题**：含分类标签和风险类型
- **3 个对话场景**：正常咨询、升级处理、无答案回复
- **8 个 Bad Cases**：幻觉陷阱、政策编造、跨文档混淆
- **seed_demo_data.py**：支持 `--validate-only`、`--dry-run`、`--mock` 三种模式

### 评估报告

- JSON + Markdown 双格式评估报告生成
- 包含执行摘要、指标表格、场景覆盖详情、失败用例分析
- **无 API Key / 无 Qdrant / 无 Docker 也能复现基础评估**

---

## 当前结果

> ⚠️ **重要说明**：以下指标来自 mock-friendly evaluation pipeline，**不代表真实线上 Qdrant / Embedding / LLM 效果**。当前价值是提供可复现的评估框架、测试基线和 demo 数据。

| 指标 | 值 |
|---|---|
| `tests/rag_eval/` | **37 passed** |
| `run_rag_eval.py --mock` | **15 passed, 0 failed** |
| No-Answer Accuracy | **100.0%** |
| Citation Accuracy | **88.9%** |
| Hallucination Risk | **0 cases** |
| Baseline 回归测试 | **267 passed, 36 failed, 1 skipped，无新增失败** |

---

## 如何查看成果

| 文件 | 说明 |
|---|---|
| [`backend/tests/rag_eval/`](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness/backend/tests/rag_eval) | 37 个 pytest 测试 |
| [`backend/scripts/run_rag_eval.py`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/scripts/run_rag_eval.py) | 独立评估 runner |
| [`backend/scripts/seed_demo_data.py`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/scripts/seed_demo_data.py) | Demo 数据种子脚本 |
| [`backend/scripts/demo_data/`](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness/backend/scripts/demo_data) | SmartHome Demo 数据集 |
| [`backend/reports/rag_eval_report.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/reports/rag_eval_report.md) | 评估报告 |
| [`backend/docs/rag-evaluation.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/docs/rag-evaluation.md) | 使用文档 |
| [`ENHANCEMENT_SUMMARY.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/ENHANCEMENT_SUMMARY.md) | 增强概述 |
| [`ENHANCEMENT_SUMMARY.zh-CN.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/ENHANCEMENT_SUMMARY.zh-CN.md) | 中文增强概述 |

---

## 如何运行

```powershell
# 进入 backend 目录
cd selected/basjoo\backend

# 运行 RAG eval 测试（37 个测试）
.\venv\Scripts\python.exe -m pytest tests/rag_eval/ -v

# 运行评估 runner（mock 模式，15 个 eval cases）
.\venv\Scripts\python.exe scripts/run_rag_eval.py --mock

# 验证 demo 数据格式
.\venv\Scripts\python.exe scripts/seed_demo_data.py --validate-only

# 预览 demo 数据摘要
.\venv\Scripts\python.exe scripts/seed_demo_data.py --dry-run

# 生成 mock 知识库 fixture
.\venv\Scripts\python.exe scripts/seed_demo_data.py --mock
```

---

## 项目价值

这个项目**不是**：
- ❌ 简单调用 LLM API 做一个聊天机器人
- ❌ 从 0 写一个玩具客服 Agent
- ❌ 做一个通用的 AI 工作流平台

这个项目**是**：
- ✅ 接手一个真实开源项目（Basjoo，MIT License，143 stars）
- ✅ 阅读并理解其 RAG 检索、文档处理、LLM 调用等核心模块
- ✅ 在不破坏原有架构的前提下，补充工程质量增强
- ✅ 能体现：代码阅读能力、测试设计能力、RAG 评估方法论、mock 策略、报告生成、开源二开流程

---

## 简历写法

### 中文版

> 基于开源 AI 客服系统 Basjoo 进行二次开发，构建 RAG Evaluation Harness 与 SmartHome Demo Data，覆盖检索命中、无答案回退、证据引用、幻觉风险等 15 类评估用例，并通过 pytest 与独立 runner 生成 JSON / Markdown 评估报告。设计 Mock Embedding / Retriever / Pipeline 实现无 API Key、无 Qdrant 环境下的可复现评估。

### English Version

> Enhanced an open-source AI customer support platform (Basjoo) by adding a mock-friendly RAG evaluation harness with 15 eval cases covering retrieval precision, no-answer fallback, citation accuracy, and hallucination risk. Built deterministic mock pipeline enabling reproducible evaluation without API keys or external services.

---

## 面试讲解重点

### 1. 为什么不从 0 写客服 Agent？

从 0 写一个客服 Agent 需要大量时间在基础设施上（认证、数据库、前端、部署），而真正有价值的是在已有系统上做工程质量增强。选择 Basjoo 是因为：MIT License、技术栈主流（Python/FastAPI + Next.js）、测试完善（35+ 测试文件）、RAG 管道完整。

### 2. 为什么选择 RAG Eval Harness？

RAG 评估是 AI Agent 工程化的核心能力。Basjoo 有完整的 RAG 管道（文档上传 → 解析 → 分块 → Embedding → Qdrant → 检索），但没有任何系统化的质量评估机制。补充 RAG eval 填补了这个核心空白。

### 3. 如何在无 API Key / 无 Qdrant 下做可复现评估？

设计了三层 mock：
- **Mock Embedding**：基于字符频率的 256 维向量，同一输入永远产生同一向量
- **Mock Retriever**：70% 关键词重叠 + 30% 向量余弦相似度的混合评分
- **Mock Pipeline**：从检索结果中提取句子作为答案，不调用 LLM

这样测试是确定性的、可复现的、CI 友好的。

### 4. 如何避免破坏原项目？

- 所有新增代码都在独立目录：`tests/rag_eval/`、`scripts/`、`reports/`、`docs/`
- 不修改任何 `backend/services/`、`backend/models.py`、`backend/api/` 等核心文件
- 不引入重型依赖（不使用 RAGAS、DeepEval、LangChain）
- 原有测试 baseline 不变：267 passed, 36 failed, 1 skipped

### 5. 当前 mock 评估和真实线上评估的边界

- **mock 评估的价值**：验证评估框架本身是否正确（测试结构、指标计算、报告生成）
- **mock 评估的局限**：字符频率向量不是语义向量，提取式回答不是 LLM 生成
- **真实评估需要**：Jina/OpenAI Embedding API + Qdrant + LLM API
- **框架已预留**：真实模式只需替换 embedding 和 retriever 实现，测试结构不变

---

## 项目结构

```
CustomerOpsAgent_2/              ← 本仓库（项目管理 / 文档）
├── README.md                    ← 你正在看的文件
├── README_WORKSPACE.md          # 工作区说明与命名规范
├── PHASE0_PREP.md               # Phase 0 准备确认
├── PHASE1_PLAN.md               # Phase 1 实现计划与结果
├── OPEN_SOURCE_AUDIT.md         # 开源项目选型审查报告
├── candidates/                  # 候选项目（gitignored，不提交）
└── selected/                    # 二开项目（gitignored，不提交）
    └── basjoo/                  # Basjoo fork，正式二开
```

**代码仓库**：https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness

---

## 版本历史

### 外层管理仓库（本仓库）

| 版本 | 状态 | 说明 |
|---|---|---|
| v0.1-audit | ✅ | 选型审查完成 |
| v0.2-deep-audit | ✅ | 深度审查完成 |
| v0.3-phase0-prep | ✅ | Phase 0 准备完成 |
| v1.6-phase2-environment-setup | ✅ | Phase 2 环境准备 |
| v1.6.1-phase2-env-verification | ✅ | Phase 2 环境验证（NOT READY FOR v2.0） |

### 代码仓库（basjoo fork）

| 版本 | 状态 | 说明 |
|---|---|---|
| v0.1-fork-baseline | ✅ | Fork 基线建立 |
| v0.2-setup-verified | ✅ | 本地运行和测试验证完成 |
| v0.2.5-product-walkthrough | ✅ | 产品体验完成 |
| v0.3-phase1-plan | ✅ | Phase 1 计划锁定 |
| v1.0-rag-eval-harness | ✅ | RAG 评估框架实现 |
| v1.1-demo-data | ✅ | SmartHome Demo Data 实现 |
| v1.2-docs-and-report | ✅ | 文档与报告优化 |
| v1.3-phase1-complete | ✅ | Phase 1 最终验收 |

---

## 技术栈

- **后端**：Python 3.11+ / FastAPI
- **前端**：TypeScript / Next.js 14
- **数据层**：PostgreSQL + Qdrant（向量搜索）
- **部署**：Docker Compose
- **License**：MIT（基于 [Basjoo](https://github.com/haoyiyin/basjoo)）

---

*基于 [Basjoo](https://github.com/haoyiyin/basjoo)（MIT License）进行二次开发*
*创建日期：2026-06-19*
*最后更新：2026-06-20 (v1.6.1-phase2-env-verification)*
