# CustomerOpsAgent_2｜最终产物与路线图锁定

> **Created**: 2026-06-23
> **Purpose**: 锁定最终产物定义与后续开发路线，防止项目继续缩小为单纯 RAG Eval Harness
> **Status**: Active — 后续所有开发必须以此为准

---

## 1. Final Product Name

```text
Basjoo 二开项目｜跨境电商客服 Agent 与 RAG 质量评估增强
```

## 2. Final Product Definition

本项目不是单纯 RAG Evaluation Harness，也不是从 0 自研完整客服系统。

本项目是在开源 AI 客服系统 [Basjoo](https://github.com/haoyiyin/basjoo)（MIT License）的基础上进行二次开发，面向跨境电商客服场景补充知识库、RAG 检索、自动回复工作流与质量评估能力。

一句话定义：

```text
基于开源 AI 客服系统 Basjoo，二开成一个面向跨境电商客服场景的 Agent Demo，并补充可复现的 RAG 检索与回答质量评估能力。
```

## 3. Final Product Capabilities

最终产物应包含 4 块能力：

### 3.1 跨境电商客服知识库

商品问答、物流时效、退换货政策、支付问题、清关/关税、售后保修等。

### 3.2 RAG 检索与证据引用

用户问题可以检索知识库，并返回来源依据。

### 3.3 自动回复 / Agent Workflow

工单进入 → 意图识别 → 检索知识 → 生成客服回复 → 证据预览 → 人工复核。

### 3.4 RAG Evaluation Harness

评估检索准确性、无答案拒答、证据引用、回答一致性和幻觉风险。

## 4. Current Completed Work

当前已完成的是质量评估底座，不是最终产品全部。

### 已完成版本

| 版本 | 内容 | 状态 |
|---|---|---|
| v1.0 | Mock RAG Evaluation Harness | ✅ frozen |
| v1.1 | Demo Data (SmartHome) | ✅ frozen |
| v1.2 | Docs & Report | ✅ frozen |
| v1.3 | Phase 1 Complete | ✅ frozen |
| v2.0 | Real Qdrant + SiliconFlow Retrieval Eval | ✅ frozen |
| v2.1 | Real eval cases 10 个 + mismatch analysis + 文档冻结 | ✅ frozen |

### 已完成能力定位

```text
这些不是最终产品全部，而是最终客服 Agent 的质量评估与回归测试模块。
```

### 已完成技术组件

- `run_rag_eval.py`：RAG 评估 runner（mock + real 模式）
- `seed_demo_data.py`：Demo 数据种子脚本
- `rag_eval_cases.json`：15 个评估用例
- `demo_knowledge_base.json`：知识库文档
- 50 个 pytest 测试
- Mock + Real 双模评估报告

## 5. Future Roadmap

### v2.2：Cross-border E-commerce Scenarioization

**目标**：把当前 SmartHome demo data 逐步改造成跨境电商客服场景数据。

**范围**：
- 知识库文档从 SmartHome 改为跨境电商（商品 FAQ、物流、退换货、支付、清关）
- Eval cases 从智能家居改为跨境电商客服问题
- Agent prompt 从通用客服改为跨境电商客服
- 保留现有评估框架不变

**不做**：
- 不新增评估维度
- 不改评估框架代码
- 不做 LLM 回答生成

### v3.0：Full RAG Answer Evaluation

**目标**：检索 → 拼接 context → LLM 生成回答 → 检查引用、拒答、幻觉和回答质量。

**范围**：
- 在 retrieval eval 基础上增加 answer generation 评估
- 评估 LLM 回答的准确性、完整性、幻觉风险
- 评估无答案场景的拒答能力
- 评估证据引用的准确性

**不做**：
- 不做前端 UI
- 不做完整 Agent workflow
- 不做部署

### v4.0：Minimal Agent Workflow Demo

**目标**：完成一个最小客服工单流程。

**流程**：
```text
Ticket Intake → Intent Recognition → Retrieval → Draft Reply → Evidence Preview → Human Review
```

**不做**：
- 不做复杂多租户
- 不做大规模前端 dashboard
- 不做线上部署

### vFinal：Final Quality Audit and Freeze

**目标**：质量审计、tag、README、最终工程边界冻结。

**范围**：
- 全量测试通过
- 评估报告完整
- README 更新
- Tag 创建
- 工程边界冻结

## 6. Explicit Non-goals

以下内容明确不做：

- ❌ 从 0 自研完整客服 SaaS
- ❌ 复杂多租户商业系统
- ❌ 大规模前端 dashboard
- ❌ 真实企业数据
- ❌ 线上部署
- ❌ 无限扩 case
- ❌ 求职包装
- ❌ 恢复 CAREER_PACKAGE.md
- ❌ 面试话术

## 7. Development Rules

后续开发必须遵守以下规则：

### 7.1 单轮范围控制

- 每轮只做一个可验证能力
- 一个 prompt 最多改 1–2 个核心文件
- 文档和 tag 单独收尾
- 真实 API / Docker / Qdrant 验证单独一轮
- 不得把功能开发、测试、文档、提交、包装混在一轮

### 7.2 大任务拆分

- 如果任务变大，必须拆成 vX.Y.1 / vX.Y.2 / vX.Y.3
- 每个子版本必须有独立验收标准

### 7.3 安全规则

- 不提交 .env
- 不打印 API Key
- 不使用 git add .
- 不使用 git add *
- 不提交 selected/ 或 candidates/ 到外层仓库

### 7.4 边界规则

- 不修改 Basjoo 核心源码（除非明确允许）
- 不做求职包装
- 不恢复 CAREER_PACKAGE.md
- 不写面试话术

## 8. Version Tag Roadmap

```text
v0.x  — 准备阶段（审查、fork、setup）         ✅ 完成
v1.x  — Mock RAG Eval Harness                 ✅ 完成
v2.0  — Real Qdrant Retrieval Eval             ✅ 完成
v2.1  — Real eval expansion + mismatch         ✅ 完成
v2.2  — Cross-border E-commerce Scenarioization ⬜ 下一阶段
v3.0  — Full RAG Answer Evaluation             ⬜ 待规划
v4.0  — Minimal Agent Workflow Demo            ⬜ 待规划
vFinal — Final Quality Audit and Freeze        ⬜ 待规划
```

## 9. Decision Log

| 日期 | 决策 | 理由 |
|---|---|---|
| 2026-06-23 | 锁定最终产物为"跨境电商客服 Agent 与 RAG 质量评估增强" | 防止项目继续缩小为单纯 RAG Eval Harness |
| 2026-06-23 | 明确 v1.x/v2.0/v2.1 是质量评估底座 | 当前完成的只是最终产品的一个模块 |
| 2026-06-23 | 下一阶段为 v2.2 scenarioization | 在做 LLM 回答评估之前，先把 demo data 改造成跨境电商场景 |
| 2026-06-23 | 不做求职包装 | 开发中期不做包装，只有最终闭环后才允许 |

---

*Document created: 2026-06-23*
*Purpose: Lock final product definition and roadmap for basjoo enhancement*
*Status: Active*
