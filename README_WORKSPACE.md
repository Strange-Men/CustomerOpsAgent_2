# CustomerOpsAgent_2｜跨境电商客服 Agent 与 RAG 质量评估增强

## 目的

本工作区用于对开源 AI 客服系统 Basjoo 进行二次开发，面向跨境电商客服场景补充知识库、RAG 检索、自动回复工作流与质量评估能力。

**不从 0 自研**，只做开源项目的增强和定制。

> 最终产物定义与完整路线图见 [FINAL_PRODUCT_ROADMAP.md](./FINAL_PRODUCT_ROADMAP.md)

## 目录结构

```
CustomerOpsAgent_2/
├── README_WORKSPACE.md          # 本文件 — 工作区说明与命名规范
├── OPEN_SOURCE_AUDIT.md         # 候选项目选型审查报告
├── .gitignore                   # Git 忽略规则
├── candidates/                  # 候选开源项目（git clone）
│   ├── tgo/                     # TGO — AI Agent Customer Service Platform
│   └── basjoo/                  # Basjoo — AI Customer Support Platform
└── selected/                    # 二开项目（基于 fork）
```

## 命名规范

### 项目名统一

| 场景 | 正确 | 禁止 |
|---|---|---|
| 项目名 | `CustomerOpsAgent_2` | `CustomerOpsAgent2`、`customeropsagent2`、`CustomerOps Agent 2`、`CustomerOps-Agent-2`、`OpenSourceCustomerAgent` |
| 仓库名 | `CustomerOpsAgent_2` | 同上 |
| 目录名 | `D:\Claude_workfile\CustomerOpsAgent_2` | 任何变体 |
| 文档标题 | `CustomerOpsAgent_2｜Open-source AI Customer Support Agent Enhancement` | 任何变体 |

### Commit Message 规范

使用 Conventional Commits 风格：

```
docs:       文档变更
chore:      构建/工具变更
audit:      审查相关
setup:      环境搭建
fix:        修复
feat:       新功能
test:       测试
style:      格式调整
refactor:   重构
```

当前审查阶段：
```
docs: audit open source customer support agent projects
```

### 版本 Tag 规范

**外层 CustomerOpsAgent_2 文档仓库**:
```
v0.1-audit       # 选型审查完成 ✅
v0.2-deep-audit  # 深度审查完成 ✅
v0.3-phase0-prep # 二开准备完成 ✅
```
不进入 v1，因为外层仓库不承载二开代码。

**Basjoo fork 仓库**:
```
v0.1-fork-baseline   # fork 后原始基线
v0.2-setup-verified  # 本地运行和测试验证完成
v0.3-phase1-plan     # Phase 1 计划锁定
v1.0-rag-eval-harness # RAG eval harness 实现完成
v1.1-demo-data       # demo data 完成
v1.2-docs-and-report # 文档与评估报告完成
v1.3-phase1-complete # Phase 1 完整验收 ✅
```

**规则**:
- 准备阶段全部 v0
- 第一行代码二开开始进入 v1
- 不允许在 v0 阶段写功能代码
- v1 之后每个版本必须有可运行验收结果

当前阶段：`v2.2.0`（最终产物与路线图锁定）

**v2.0 版本 Tag**:
- `v2.0-real-qdrant-eval-adapter` — 功能交付点（c74a3c4）
- `v2.0.1-quality-freeze` — 质量审计冻结点（a53f608）

**v2.1 版本 Tag**:
- `v2.1.4-quality-freeze` — v2.1 质量审计冻结点（a69c074）

## Git 规范

### 分支策略

- `main` — 稳定版本
- `dev` — 开发分支
- `feat/*` — 功能分支
- `fix/*` — 修复分支

### 提交白名单

本轮只允许提交：
- `README_WORKSPACE.md`
- `OPEN_SOURCE_AUDIT.md`
- `PHASE0_PREP.md`
- `PHASE1_PLAN.md`
- `.gitignore`

禁止提交：
- `candidates/` 下的项目源码
- `selected/` 下的项目源码
- `node_modules/`、`.next/`、`dist/`、`build/`
- `.env`、`*.env`、API Key
- `__pycache__/`、`.pytest_cache/`、`.ruff_cache/`
- 任何缓存和依赖目录

## 注意事项

1. **不要修改原项目的核心代码** — 二开前需要先理解架构
2. **不要提交 API Key / .env / node_modules** — 安全第一
3. **不要忽略 license** — 每个项目都有不同的开源协议
4. **不要覆盖已有文件** — 操作前先确认
5. **candidates 目录下的项目是独立的 git repo** — 外层 .gitignore 已排除其 .git 目录
6. **开发中期不做简历包装** — 只有阶段闭环后才允许做阶段性求职材料

## 工作流

1. 搜索并筛选候选项目 ✅
2. Clone 候选项目到 `candidates/` ✅
3. 审查项目架构、RAG、Memory、测试、前端 ✅
4. 确定二开方向和第一阶段任务 ✅
5. 深度审查最终推荐项目 (Basjoo) ✅
6. Phase 0 准备确认 ✅
7. Fork 原项目到 Strange-Men/basjoo ✅
8. Clone fork 到 selected/basjoo ✅
9. Setup 验证完成 (v0.2-setup-verified) ✅
10. 在 fork 分支 phase1-rag-eval-harness 上开始二开 ✅
11. v1.0-rag-eval-harness：RAG eval harness 实现完成 ✅
12. v1.1-demo-data：demo data 实现完成 ✅

## 选定项目

**最终推荐**: [Basjoo](https://github.com/haoyiyin/basjoo)
- License: MIT
- 技术栈: Python/FastAPI + Next.js 14 + PostgreSQL + Qdrant + Redis
- 代码规模: 7.2MB, 96 Python + 109 TypeScript files
- 二开难度: 低-中
- Setup 验证: ✅ 通过 (v0.2-setup-verified)

**Setup 验证结果**:
- 后端测试: 267 passed, 36 failed (Qdrant 依赖), 1 skipped
- 前端 build: ✅ 成功
- 前端测试: 125 passed (20 test files)
- MockLLMService: ✅ 可用 (不需要真实 API Key)
- Windows 兼容性: ✅ 已验证

**第二备选**: [TGO](https://github.com/tgoai/tgo)
- License: Modified Apache 2.0
- 二开难度: 中 (微服务架构较复杂)

## Git 策略

### 外层仓库 (CustomerOpsAgent_2)

- 只管理审查文档和二开规划
- 不包含任何候选项目源码
- candidates/ 和 selected/ 被 .gitignore 忽略
- 版本: v0.1-audit → v0.2-deep-audit → v0.3-phase0-prep (不进入 v1)

### 二开仓库 (fork)

- Fork haoyiyin/basjoo → Strange-Men/basjoo ✅
- origin: https://github.com/Strange-Men/basjoo ✅
- upstream: https://github.com/haoyiyin/basjoo ✅
- 当前分支: main (commit 6939926)
- 开发分支: phase1-rag-eval-harness (commit 4a40ae1)
- 版本: v0.1-fork-baseline ✅ → v0.2-setup-verified ✅ → v0.2.5-product-walkthrough ✅ → v0.3-phase1-plan ✅ → v1.0-rag-eval-harness ✅ → v1.1-demo-data ✅ → v1.2-docs-and-report ✅ → v1.3-phase1-complete ✅ → v2.0-real-qdrant-eval-adapter ✅ → v2.0.1-quality-freeze ✅ → v2.1.1-real-eval-case-expansion ✅ → v2.1.2-real-eval-mismatch-analysis ✅ → v2.1.3-report-and-docs-polish ✅ → v2.1.4-quality-freeze ✅

### 目录结构说明

```
CustomerOpsAgent_2/
├── README_WORKSPACE.md     # 工作区说明
├── OPEN_SOURCE_AUDIT.md    # 选型审查报告
├── PHASE0_PREP.md          # Phase 0 准备确认
├── .gitignore              # Git 忽略规则
├── candidates/             # 候选开源项目（只读，不提交）
│   └── basjoo/             # 原项目审查副本 (commit 6939926)
└── selected/               # 二开项目（不提交）
    └── basjoo/             # 我的 fork，正式二开发 (origin: Strange-Men/basjoo, upstream: haoyiyin/basjoo, v0.2-setup-verified)
```

**重要**:
- candidates/ 只用于审查，不修改
- selected/ 用于正式二开，基于 fork
- 不提交 candidates/ 或 selected/ 到外层仓库

## Phase 1 二开任务

**RAG Evaluation Harness + Demo Data**

### v1.0 已完成 — RAG Eval Harness

- ✅ `backend/tests/rag_eval/` — 23 pytest tests 全部通过
- ✅ `backend/scripts/run_rag_eval.py` — eval runner，生成报告
- ✅ `backend/reports/rag_eval_report.json` + `.md` — 评估报告
- ✅ `backend/docs/rag-evaluation.md` — 使用文档
- ✅ 不修改原项目核心代码
- ✅ 不依赖真实 API Key (mock mode)
- ✅ 原有测试 baseline 不变：267 passed, 36 failed, 1 skipped

### v1.1 已完成 — Demo Data

- ✅ `scripts/demo_data/` — Demo 数据 (agents, knowledge, questions, conversations, bad cases)
- ✅ `scripts/seed_demo_data.py` — 支持 --validate-only / --dry-run / --mock
- ✅ `tests/rag_eval/test_demo_data_integrity.py` — 14 integrity tests 全部通过
- ✅ v1.0 tests/rag_eval/ 全部通过 (37 tests, 含 14 new)
- ✅ 原有测试 baseline 不变：267 passed, 36 failed, 1 skipped

详见 OPEN_SOURCE_AUDIT.md 和 PHASE1_PLAN.md。

## 技术栈偏好

- Python / FastAPI（后端）
- TypeScript / React / Next.js（前端）
- PostgreSQL + 向量数据库（数据层）
- Docker Compose（部署）
- MIT / Apache 2.0（license 优先）

---

## 当前状态

**当前阶段**：v2.2.0 planning（最终产物与路线图锁定）

**已完成**：
- ✅ v0.1-audit：选型审查完成
- ✅ v0.2-deep-audit：深度审查完成
- ✅ v0.3-phase0-prep：二开准备完成
- ✅ v0.1-fork-baseline：Fork 基线建立
- ✅ v0.2-setup-verified：Setup 验证完成
- ✅ v0.2.5-product-walkthrough：产品体验完成
- ✅ v0.3-phase1-plan：Phase 1 计划锁定
- ✅ v1.0-rag-eval-harness：RAG eval harness 实现完成
- ✅ v1.1-demo-data：demo data 实现完成
- ✅ v1.2-docs-and-report：文档与报告优化完成
- ✅ v1.3-phase1-complete：Phase 1 最终验收完成
- ✅ v1.4-phase2-readiness-audit：Phase 2 准入审查完成
- ✅ v1.5-phase2-condition-resolution：Phase 2 条件确认完成
- ✅ v1.6-phase2-environment-setup：Phase 2 环境准备完成
- ✅ v1.6.2-phase2-env-final-verification：Phase 2 最终环境验证完成
- ✅ v2.0-real-qdrant-eval-adapter：真实 Qdrant 检索评估完成
- ✅ v2.0.1-quality-freeze：v2.0 质量审计通过，已冻结
- ✅ v2.1 planning：v2.1 规划完成
- ✅ v2.1.1：real eval cases 5 → 10 完成
- ✅ v2.1.2：real eval mismatch analysis 完成
- ✅ v2.1.3：报告格式和文档收尾完成
- ✅ v2.1.4：v2.1 质量审计和 freeze tag 完成

**v2.0 完成内容（2026-06-23）**：
- `seed_demo_data.py --write-db`：写入 Qdrant（SiliconFlow embedding, 1024 dim）
- `run_rag_eval.py --real`：运行 5 个 real retrieval eval cases
- 报告：`rag_eval_real_report.json/md`、`rag_eval_mock_vs_real.md`
- 测试：44 个 pytest 全部通过（37 原有 + 7 新增）
- Tag：v2.0-real-qdrant-eval-adapter（功能交付点）
- Tag：v2.0.1-quality-freeze（质量审计冻结点）

**v2.1.1 完成内容（2026-06-23）**：
- Real eval cases 从 5 个扩展到 10 个
- 新增覆盖：multi_doc_retrieval、no_answer_fallback ZH、low_relevance_reject、evidence_citation ZH、hallucination_risk
- 44 个 pytest 测试全部通过
- Mock eval 继续 15 cases 全部通过
- Real eval 变成 10 cases，全部通过
- 报告文件已更新

**v2.1.2 完成内容（2026-06-23）**：
- 新增 `_compute_mismatch()` 函数，为每个 real eval case 计算 mismatch 分析
- 新增字段：language、returned_sources_top3、matched_sources、missing_sources、unexpected_sources、mismatch_type、analysis_note
- mismatch_type 枚举：none、missing_expected_source、unexpected_source_in_top3、low_rank_expected_source、no_answer_with_retrieval_noise、low_confidence_match
- 新增 6 个 pytest 测试（TestMismatchAnalysis），纯逻辑测试，不调用 API
- 50 个 pytest 测试全部通过
- Mock eval 继续 15 cases 全部通过
- Real eval 继续 10 cases 全部通过
- 报告新增 Mismatch Analysis 章节和 Real Retrieval Error Analysis 章节
- 关键发现：Precision@3=0.600 是 metric artifact（no-answer cases 拉低），实际 normal cases 为 1.000
- 关键发现：TC004 是唯一真实检索问题（return_policy.md 未进 top-5）

**v2.1.3 完成内容（2026-06-23）**：
- 报告格式和文档收尾
- 更新版本历史：添加 v2.1.1/v2.1.2 变更记录
- 添加指标解释：Precision@3=0.600 原因、Recall@3=0.950、MRR=0.600
- 添加限制说明：retrieval eval only、no chat eval、no answer generation eval、no frontend、no deployment
- 添加安全说明：.env 不提交、API Key 不写入文档
- 移除求职包装内容
- 50 个 pytest 测试全部通过
- 修改文件：ENHANCEMENT_SUMMARY.md、ENHANCEMENT_SUMMARY.zh-CN.md、rag-evaluation.md、reports/README.md、rag_eval_real_report.md、rag_eval_mock_vs_real.md

**工程边界重置（2026-06-20）**：
- 项目曾出现简历包装漂移，现已重置回工程主线
- 后续不再继续包装，除非新阶段闭环
- 详见 `ENGINEERING_BOUNDARY.md` 和 `NEXT_ENGINEERING_PLAN.md`

详见 `PHASE1_PLAN.md`。

---

*Created: 2026-06-19*
*Last updated: 2026-06-23 (v2.2.0-final-product-roadmap-lock)*
