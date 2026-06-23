# CustomerOpsAgent_2｜Basjoo 开源 AI 客服系统 RAG 评估增强

## 项目定位

这是一个基于开源 AI 客服系统 [Basjoo](https://github.com/haoyiyin/basjoo)（MIT License）的二次开发与工程增强项目。项目重点是为现有 AI 客服系统补充 **RAG Evaluation Harness**、**Demo Data**、**Mock 可复现评估**和**评估报告**能力。

---

## 仓库结构

本项目使用两个仓库管理：

| 仓库 | 用途 | 地址 |
|---|---|---|
| **CustomerOpsAgent_2**（本仓库） | 项目管理、阶段计划、审查记录、工程文档 | [Strange-Men/CustomerOpsAgent_2](https://github.com/Strange-Men/CustomerOpsAgent_2) |
| **basjoo**（二开仓库） | 代码改动、测试、脚本、评估报告 | [Strange-Men/basjoo](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness) |

> **代码仓库**：https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness

---

## 已完成工程成果（Phase 1）

### RAG Evaluation Harness

- RAG 评估测试框架，覆盖检索精度、无答案回退、证据引用、幻觉风险等维度
- **Mock Embedding**：基于字符频率的 256 维向量生成，无需真实 API Key
- **Mock Retriever**：基于内存的余弦相似度搜索，无需 Qdrant
- **Mock RAG Pipeline**：从检索结果中提取答案，无需调用 LLM
- **15 个 eval cases**：覆盖正常命中、多文档检索、无答案 fallback、低相关度拒答、evidence/citation 校验、幻觉风险
- **37 个 pytest 测试**：全部通过
- 评估指标：Precision@k、Recall@k、MRR、No-Answer Accuracy、Citation Accuracy、Hallucination Risk

### SmartHome Demo Data

- 2 个 Demo Agent（通用客服 + 退换货专员），含 system prompt 和升级策略
- 3 篇知识库文档（产品 FAQ、退换货政策、故障排除指南）
- 15 个 Demo 问题（含分类标签和风险类型）
- 3 个对话场景 + 8 个 Bad Cases
- `seed_demo_data.py`：支持 `--validate-only`、`--dry-run`、`--mock` 三种模式

### 评估报告

- JSON + Markdown 双格式评估报告生成
- 包含执行摘要、指标表格、场景覆盖详情、失败用例分析
- 无 API Key / 无 Qdrant / 无 Docker 也能复现基础评估

---

## 当前结果

### Mock 模式（Phase 1）

| 指标 | 值 |
|---|---|
| `tests/rag_eval/` | **44 passed**（37 原有 + 7 新增） |
| `run_rag_eval.py --mock` | **15 passed, 0 failed** |
| No-Answer Accuracy | **100.0%** |
| Citation Accuracy | **88.9%** |
| Hallucination Risk | **0 cases** |
| Baseline 回归测试 | **267 passed, 36 failed, 1 skipped，无新增失败** |

### Real 模式（v2.0）

| 指标 | 值 |
|---|---|
| `run_rag_eval.py --real` | **5 passed, 0 failed** |
| Precision@3 | **0.733** |
| Recall@3 | **1.000** |
| MRR | **0.800** |
| Hit Rate | **0.800** |
| No-Answer Accuracy | **100.0%** |

---

## 当前工程边界

- **Phase 1**：Mock-friendly RAG Evaluation Harness Enhancement ✅
- **v2.0**：Real Qdrant Retrieval Evaluation Adapter ✅
- 不是完整 AI 客服系统
- 不是 LLM chat eval（只评估 retrieval，不评估回答生成）
- 不是部署版 SaaS

详细边界定义见 [ENGINEERING_BOUNDARY.md](./ENGINEERING_BOUNDARY.md)，后续工程计划见 [NEXT_ENGINEERING_PLAN.md](./NEXT_ENGINEERING_PLAN.md)。

---

## 如何查看代码成果

| 文件 | 说明 |
|---|---|
| [`backend/tests/rag_eval/`](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness/backend/tests/rag_eval) | 37 个 pytest 测试 |
| [`backend/scripts/run_rag_eval.py`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/scripts/run_rag_eval.py) | 独立评估 runner |
| [`backend/scripts/seed_demo_data.py`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/scripts/seed_demo_data.py) | Demo 数据种子脚本 |
| [`backend/scripts/demo_data/`](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness/backend/scripts/demo_data) | SmartHome Demo 数据集 |
| [`backend/reports/rag_eval_report.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/reports/rag_eval_report.md) | 评估报告 |
| [`backend/docs/rag-evaluation.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/backend/docs/rag-evaluation.md) | 使用文档 |
| [`ENHANCEMENT_SUMMARY.zh-CN.md`](https://github.com/Strange-Men/basjoo/blob/phase1-rag-eval-harness/ENHANCEMENT_SUMMARY.zh-CN.md) | 中文增强概述 |

---

## 如何运行

```powershell
# 从本仓库根目录进入 backend 目录（代码位于 selected\basjoo，GitHub 上请查看 Strange-Men/basjoo 的 phase1-rag-eval-harness 分支）
cd selected\basjoo\backend

# 运行 RAG eval 测试（37 个测试）
.\venv\Scripts\python.exe -m pytest tests\rag_eval -v

# 运行评估 runner（mock 模式，15 个 eval cases）
.\venv\Scripts\python.exe scripts\run_rag_eval.py --mock

# 验证 demo 数据格式
.\venv\Scripts\python.exe scripts\seed_demo_data.py --validate-only

# 预览 demo 数据摘要
.\venv\Scripts\python.exe scripts\seed_demo_data.py --dry-run

# 生成 mock 知识库 fixture
.\venv\Scripts\python.exe scripts\seed_demo_data.py --mock
```

---

## 当前阶段状态

- **Phase 1**：completed
- **Phase 2 readiness / environment setup**：completed
- **v2.0**：completed（真实 Qdrant 检索评估）
- **v2.0.1**：frozen（质量审计通过，已冻结）
- **当前状态**：v2.0.1-quality-freeze 已创建，v2.0 正式冻结

---

## 文档入口

- [ENGINEERING_BOUNDARY.md](./ENGINEERING_BOUNDARY.md) — 工程边界定义
- [NEXT_ENGINEERING_PLAN.md](./NEXT_ENGINEERING_PLAN.md) — 后续工程计划
- [PHASE2_READINESS_AUDIT.md](./PHASE2_READINESS_AUDIT.md) — Phase 2 就绪审计
- [PHASE2_CONDITION_DECISION.md](./PHASE2_CONDITION_DECISION.md) — Phase 2 条件决策
- [PHASE2_ENV_SETUP.md](./PHASE2_ENV_SETUP.md) — Phase 2 环境准备
- [PHASE2_ENV_VERIFICATION.md](./PHASE2_ENV_VERIFICATION.md) — Phase 2 最终环境验证

---

## 项目结构

```
CustomerOpsAgent_2/              ← 本仓库（项目管理 / 文档）
├── README.md                    ← 你正在看的文件
├── README_WORKSPACE.md          # 工作区说明与命名规范
├── ENGINEERING_BOUNDARY.md      # 工程边界定义
├── NEXT_ENGINEERING_PLAN.md     # 后续工程计划
├── OPEN_SOURCE_AUDIT.md         # 开源项目选型审查报告
├── PHASE0_PREP.md               # Phase 0 准备确认
├── PHASE1_PLAN.md               # Phase 1 实现计划与结果
├── PHASE2_READINESS_AUDIT.md    # Phase 2 就绪审计
├── PHASE2_CONDITION_DECISION.md # Phase 2 条件决策
├── PHASE2_ENV_SETUP.md          # Phase 2 环境准备
├── PHASE2_ENV_VERIFICATION.md   # Phase 2 环境验证
├── candidates/                  # 候选项目（gitignored，不提交）
└── selected/                    # 二开项目（gitignored，不提交）
    └── basjoo/                  # Basjoo fork，正式二开
```

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
*最后更新：2026-06-23 (v2.0.1-quality-freeze)*
