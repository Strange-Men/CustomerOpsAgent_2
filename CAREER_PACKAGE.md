# CustomerOpsAgent_2｜求职包装与作品集整理

> **面向**：简历、作品集网站、面试准备
> **版本**：Phase 1 Complete (v1.3-phase1-complete)
> **日期**：2026-06-20

---

## 1. 项目定位

这是一个基于开源 AI 客服系统 [Basjoo](https://github.com/haoyiyin/basjoo) 的**二次开发项目**，不是从 0 重写客服 Agent。我的增强重点是 **RAG Evaluation Harness**（RAG 评估测试框架）、**SmartHome Demo Data**（智能家居客服演示数据）、**评估报告**和**工程可验证性**。项目适合 AI 应用开发 / Agent 开发 / Python 后端实习方向，体现了开源项目阅读、RAG 工程、测试设计、mock 策略和工程边界意识。

---

## 2. 简历项目名称建议

### 保守版
```
开源 AI 客服系统二次开发｜RAG 评估框架与 Demo Data 增强
```

### 技术版
```
Basjoo AI 客服平台二开｜RAG Evaluation Harness + SmartHome Demo Data
```

### 面试亮点版
```
开源 AI 客服系统 RAG 质量评估工程｜Mock Pipeline + 可复现测试框架
```

---

## 3. 简历项目经历 Bullet

### 精简版（适合简历）

**项目简介**：
基于开源 AI 客服系统 Basjoo 进行二次开发，补充 RAG Evaluation Harness、SmartHome Demo Data 与评估报告能力，用于验证客服 Agent 在检索命中、无答案回退、证据引用和幻觉风险等场景下的表现。

**技术亮点**：
设计 15 个 RAG eval cases 与 37 个 pytest 测试，基于 Mock Embedding / Mock Retriever / Mock RAG Pipeline 实现无 API Key、无 Qdrant 环境下的可复现评估，并生成 JSON / Markdown 评估报告。

### 详细版（适合作品集）

- 基于开源 AI 客服平台 Basjoo（MIT License, Python/FastAPI + Next.js 14）进行二次开发
- 设计并实现 RAG Evaluation Harness，覆盖 6 类场景：正常命中、多文档检索、无答案回退、低相关度拒答、evidence/citation 校验、幻觉风险
- 构建 Mock Embedding（字符频率 256 维向量）、Mock Retriever（内存余弦相似度）、Mock Pipeline（提取式答案），实现无外部依赖的确定性测试
- 实现标准 IR 评估指标：Precision@k、Recall@k、MRR、No-Answer Accuracy、Citation Accuracy、Hallucination Risk
- 创建 SmartHome Support Demo Data：2 个 Agent、3 篇知识库文档、15 个问题、3 个对话场景、8 个 Bad Cases
- 达成 37 个 pytest 测试全部通过，15/15 eval cases 全部通过，原有 267 个测试无回归
- 生成 JSON + Markdown 评估报告，含执行摘要、指标表格、场景覆盖详情

### 英文版（备用）

> Enhanced an open-source AI customer support platform (Basjoo) by adding a mock-friendly RAG evaluation harness with 15 eval cases covering retrieval precision, no-answer fallback, citation accuracy, and hallucination risk. Built deterministic mock pipeline (character-frequency embeddings, in-memory cosine similarity) enabling reproducible evaluation without API keys or external services. Created SmartHome demo dataset with 2 agents, 3 knowledge docs, 15 questions, 3 conversations, and 8 adversarial cases.

---

## 4. 作品集项目卡片内容

### 项目标题
```
开源 AI 客服系统 RAG 评估工程
```

### 一句话介绍
```
基于 Basjoo 开源 AI 客服平台，构建 RAG Evaluation Harness 与 Demo Data，实现无 API Key 的可复现评估。
```

### 技术栈
```
Python / FastAPI / pytest / Qdrant / Next.js 14 / PostgreSQL
```

### 我做了什么
```
- 设计 15 个 RAG eval cases，覆盖 6 类场景
- 实现 Mock Embedding / Retriever / Pipeline
- 创建 SmartHome Demo Data（2 Agent + 3 文档 + 15 问题 + 8 Bad Cases）
- 生成 JSON + Markdown 评估报告
```

### 核心成果
```
37 pytest 测试全通过 | 15/15 eval cases 全通过 | 0 回归
```

### 测试结果
```
Precision@3: 0.567 | Recall@3: 0.978 | MRR: 0.600
No-Answer Accuracy: 100% | Citation Accuracy: 88.9% | Hallucination: 0
```

### GitHub 链接说明
```
管理仓库：github.com/Strange-Men/CustomerOpsAgent_2
代码仓库：github.com/Strange-Men/basjoo (branch: phase1-rag-eval-harness)
```

### 适合展示的文件入口
```
- ENHANCEMENT_SUMMARY.zh-CN.md — 中文增强说明
- backend/reports/rag_eval_report.md — 评估报告
- backend/tests/rag_eval/ — 测试代码
- backend/scripts/run_rag_eval.py — 评估 runner
```

---

## 5. 面试讲解话术

### Q1：你为什么不从 0 写客服 Agent？

> 真实工作中经常是接手已有系统，而不是从 0 重写。基于成熟开源项目二开更接近实际工程场景。我选择 Basjoo 是因为它 MIT License、技术栈主流（Python/FastAPI + Next.js）、测试完善（35+ 测试文件）、RAG 管道完整。我的重点是补工程质量能力（RAG 评估、Demo 数据、测试验证），而不是重复造轮子。

### Q2：你具体做了什么？

> 主要做了一套 RAG Evaluation Harness。具体包括：
> - 15 个 eval cases，覆盖正常命中、无答案回退、幻觉风险等 6 类场景
> - Mock Embedding（字符频率向量）、Mock Retriever（内存相似度搜索）、Mock Pipeline（提取式答案）
> - 37 个 pytest 测试，全部通过
> - 独立 eval runner，生成 JSON + Markdown 评估报告
> - SmartHome Demo Data：2 个 Agent、3 篇知识文档、15 个问题、8 个 Bad Cases
> - seed_demo_data.py 脚本，支持 validate-only、dry-run、mock 三种模式

### Q3：没有真实 API Key / Qdrant，这个项目还有价值吗？

> 有价值。当前目标是验证评估框架本身是否正确，不是验证真实 RAG 质量。Mock 模式的价值在于：
> - 测试是确定性的、可复现的、CI 友好的
> - 不需要付 API 调用费用
> - 可以快速迭代测试结构和指标计算逻辑
> 真实 Qdrant / Embedding 集成是 Phase 2 的扩展方向，框架已预留接口。我不会把 mock 指标夸大成线上效果。

### Q4：你怎么证明没有破坏原项目？

> - 原有测试 baseline 不变：267 passed, 36 failed, 1 skipped
> - 新增 tests/rag_eval/ 全部通过：37 passed
> - 所有新增代码都在独立目录：tests/rag_eval/、scripts/、reports/、docs/
> - 不修改任何 backend/services/、backend/models.py、backend/api/ 等核心文件
> - 分支管理：phase1-rag-eval-harness 独立分支，tag 版本清晰（v1.0 → v1.3）

### Q5：这个项目体现了哪些能力？

> - **开源项目阅读**：理解 Basjoo 的 RAG 管道、文档处理、LLM 调用等核心模块
> - **FastAPI / Python 后端**：在已有 FastAPI 项目上进行开发
> - **RAG 工程**：理解 embedding、retrieval、citation、hallucination 等概念
> - **pytest 测试**：设计测试用例、使用 fixture、mock 策略
> - **Mock 策略**：Mock Embedding / Retriever / Pipeline 的设计与实现
> - **评估报告**：JSON + Markdown 双格式报告生成
> - **Git 工作流**：fork、branch、tag、PR 流程
> - **工程边界意识**：明确 mock 指标的局限，不夸大效果

---

## 6. 项目边界说明

### 当前不是什么

- ❌ 不是完整生产级 AI 客服系统
- ❌ 不是线上真实 Qdrant / Embedding 评测
- ❌ 不是从 0 开发的客服 Agent
- ❌ mock 指标不代表真实线上 RAG 效果

### 当前是什么

- ✅ 基于开源项目的工程质量增强
- ✅ 可复现的 RAG 评估框架
- ✅ 完整的 SmartHome Demo 数据集
- ✅ 验证评估逻辑正确性的测试基线

### Phase 2 才考虑

- 真实 Qdrant 集成
- 真实 Embedding API（Jina / OpenAI）
- 真实 LLM 生成回答
- 延迟/性能指标
- 多轮对话评估

---

## 7. 推荐放简历的位置

### 排序建议

1. **CodePilot** 作为主项目（自研 AI 代码审查 Agent）
2. **CustomerOpsAgent_2 / Basjoo 二开**作为第二项目（开源 AI 客服系统工程增强）

### 两个项目差异

| 维度 | CodePilot | CustomerOpsAgent_2 |
|---|---|---|
| **定位** | 自研 AI 代码审查 Agent | 开源 AI 客服系统二次开发 |
| **技术栈** | Python / LLM API / Git | Python/FastAPI / pytest / RAG |
| **亮点** | 从 0 设计 Agent 架构 | 开源项目阅读 + 工程增强 |
| **能力体现** | Agent 设计、Prompt 工程 | RAG 工程、测试设计、mock 策略 |

---

## 8. 最终推荐写法

### 推荐简历版本

```
项目名：开源 AI 客服系统二次开发｜RAG 评估框架增强

项目简介：
基于开源 AI 客服系统 Basjoo 进行二次开发，构建 RAG Evaluation Harness 与 SmartHome Demo Data，覆盖检索命中、无答案回退、证据引用、幻觉风险等评估场景。

技术亮点：
设计 15 个 RAG eval cases 与 37 个 pytest 测试，基于 Mock Embedding / Retriever / Pipeline 实现无 API Key、无 Qdrant 环境下的可复现评估，生成 JSON / Markdown 评估报告。
```

### 一句话版本（适合简历空间紧张时）

```
基于开源 AI 客服系统 Basjoo 二开，构建 RAG Evaluation Harness（15 eval cases, 37 pytest tests）与 SmartHome Demo Data，实现无 API Key 的可复现 RAG 质量评估。
```

---

## 9. 项目链接

| 资源 | 链接 |
|---|---|
| 管理仓库 | https://github.com/Strange-Men/CustomerOpsAgent_2 |
| 代码仓库 | https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness |
| 中文增强说明 | [ENHANCEMENT_SUMMARY.zh-CN.md](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/ENHANCEMENT_SUMMARY.zh-CN.md) |
| 评估报告 | [rag_eval_report.md](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/reports/rag_eval_report.md) |
| 作品集摘要 | [portfolio-summary.md](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/docs/portfolio-summary.md) |

---

*版本：Phase 1 Complete (v1.3-phase1-complete)*
*最后更新：2026-06-20*
