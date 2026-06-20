# CustomerOpsAgent_2｜Phase 0 二开准备确认

> **Created**: 2026-06-19
> **Author**: Claude Code
> **Status**: Phase 0 Complete — 准备确认完成
> **Version**: v0.3-phase0-prep

---

## 1. Current Source Code Status

### 1.1 外层仓库 (CustomerOpsAgent_2)

| 字段 | 值 |
|---|---|
| **路径** | `D:\Claude_workfile\CustomerOpsAgent_2` |
| **分支** | main |
| **Remote** | https://github.com/Strange-Men/CustomerOpsAgent_2.git |
| **用途** | 管理选型、审查、二开规划文档 |
| **是否跟踪源码** | ❌ 不跟踪 candidates/ 或 selected/ |

### 1.2 候选项目 (candidates/basjoo)

| 字段 | 值 |
|---|---|
| **路径** | `D:\Claude_workfile\CustomerOpsAgent_2\candidates\basjoo` |
| **Remote** | https://github.com/haoyiyin/basjoo.git (原项目) |
| **分支** | main |
| **最新 commit** | 6939926 (chore: add SILICONFLOW_API_KEY to docker-compose services) |
| **用途** | 原项目审查副本，只读 |
| **是否有未提交修改** | ❌ 无 (working tree clean) |

### 1.3 二开项目 (selected/basjoo)

| 字段 | 值 |
|---|---|
| **路径** | `D:\Claude_workfile\CustomerOpsAgent_2\selected\basjoo` |
| **状态** | ✅ 已 clone，v1.0 开发完成 |
| **origin** | https://github.com/Strange-Men/basjoo.git |
| **upstream** | https://github.com/haoyiyin/basjoo.git |
| **主分支** | main (commit 6939926) |
| **开发分支** | phase1-rag-eval-harness (commit 4a40ae1) |
| **Working Tree** | ✅ clean |
| **Tag** | v0.1-fork-baseline ✅, v0.2-setup-verified ✅, v0.2.5-product-walkthrough ✅, v0.3-phase1-plan ✅, v1.0-rag-eval-harness ✅, v1.1-demo-data ✅ |

---

## 2. Selected Project

### 2.1 项目信息

| 字段 | 值 |
|---|---|
| **项目名** | Basjoo |
| **GitHub** | https://github.com/haoyiyin/basjoo |
| **License** | MIT (Copyright (c) 2026 haoyiyin) |
| **Stars** | 143 |
| **技术栈** | Python 3.11+ / FastAPI + TypeScript / Next.js 14 + PostgreSQL + Qdrant + Redis |
| **代码规模** | 7.2MB, 96 Python files, 109 TS/TSX files |
| **最近更新** | 2026-06-18 |
| **维护状态** | ✅ 活跃维护 |

### 2.2 为什么选择 Basjoo

1. **MIT License** — 最宽松的开源协议，学习和二开完全无限制
2. **技术栈完美匹配** — Python/FastAPI + Next.js 14 + PostgreSQL + Qdrant，主流且易理解
3. **项目规模适中** — 7.2MB 代码库，单体架构，二开难度低-中
4. **测试覆盖出色** — 35+ 后端测试文件 + Playwright E2E 测试
5. **文档质量极高** — 有 CLAUDE.md 和 AGENTS.md，专门为 AI 辅助开发优化
6. **客服场景专用** — 不是通用平台改装，而是从零构建的客服平台
7. **RAG 实现清晰** — Qdrant 向量搜索 + 文档解析 + chunking + embedding，流程完整
8. **Human Handoff 实现** — 会话接管功能已实现
9. **Embeddable Widget** — 可嵌入的聊天组件，作品集展示效果好
10. **活跃维护** — 开发者响应迅速

---

## 3. Fork / Remote Status

### 3.1 Fork 状态检查

| 检查项 | 状态 | 说明 |
|---|---|---|
| **原项目可访问** | ✅ | https://github.com/haoyiyin/basjoo 可访问 |
| **Fork 存在** | ✅ | https://github.com/Strange-Men/basjoo 已创建 |
| **selected/basjoo 已 clone** | ✅ | 已 clone 到 selected/basjoo |
| **upstream 已设置** | ✅ | upstream → haoyiyin/basjoo |
| **v0.1-fork-baseline tag** | ✅ | 已创建并推送到 origin |
| **v0.2-setup-verified tag** | ✅ | 已创建并推送到 origin |

### 3.2 当前 Remote 配置

| Remote | URL | 用途 |
|---|---|---|
| `origin` | https://github.com/Strange-Men/basjoo.git | 我的 fork，正式二开 |
| `upstream` | https://github.com/haoyiyin/basjoo.git | 原项目，定期 merge |

### 3.3 当前目录结构

```
D:\Claude_workfile\CustomerOpsAgent_2\
  candidates\
    basjoo\          # 原项目审查副本，只读 (已存在)
  selected\
    basjoo\          # 我的 fork，正式二开 (已 clone, main branch, commit 6939926)
```

---

## 4. 9-Step Preparation Checklist

### 4.1 明确原始想法 ✅

| 检查项 | 状态 | 说明 |
|---|---|---|
| **不从 0 自研** | ✅ | 基于开源项目二次开发 |
| **选定 Basjoo** | ✅ | MIT License，技术栈匹配，规模适中 |
| **二开方向明确** | ✅ | RAG Evaluation Harness + Demo Data |
| **作品集定位** | ✅ | "Built RAG evaluation harness for open-source customer support platform" |

### 4.2 想法压力测试 ✅

| 检查项 | 状态 | 说明 |
|---|---|---|
| **真实有价值** | ✅ | RAG eval 是 AI Agent 工程化的核心能力 |
| **避免重复造轮子** | ✅ | Basjoo 有完整 RAG 管道，只需加 eval |
| **不会改烂原项目** | ✅ | 纯新增 tests/rag_eval/ 和 scripts/ |
| **短期可完成** | ✅ | 1-2 周可出 MVP |
| **适合简历** | ✅ | RAG eval 是热门技能 |
| **不依赖真实 API Key** | ✅ | 可用 MockLLMService 跑通框架 |

### 4.3 想法判断结论 ✅

| 结论 | 说明 |
|---|---|
| **值不值得做** | ✅ 非常值得 |
| **为什么选 Basjoo** | MIT License + 技术栈匹配 + 规模适中 + 测试完善 |
| **为什么第一阶段选 RAG Eval** | 填补核心空白 + 不破坏原项目 + 短期可展示 |
| **是否需要收窄** | 否，当前范围合理 |

### 4.4 初版二开 PRD ✅

| 字段 | 内容 |
|---|---|
| **Phase 1 做什么** | RAG Evaluation Harness + Demo Data |
| **用户是谁** | 开发者 / 面试官 / 作品集访问者 |
| **输入输出** | 输入: golden QA pairs + knowledge docs → 输出: eval report (precision, recall, faithfulness, hallucination rate) |
| **核心功能** | RAG eval 测试框架 + demo seed data + 评估报告生成 |
| **验收标准** | pytest tests/rag_eval/ 全部通过 + 生成 eval report + seed demo 可运行 |
| **不做什么** | 不修改核心代码、不改 UI、不引入重型依赖 |

### 4.5 PRD 追问对齐 ✅

| 检查项 | 状态 | 说明 |
|---|---|---|
| **范围过大** | ❌ | 范围合理，1-2 周可完成 |
| **误入重构** | ❌ | 纯新增，不修改已有代码 |
| **需要真实 LLM** | ❌ | MockLLMService 可跑通框架 |
| **破坏原项目架构** | ❌ | 不修改任何核心文件 |
| **能用 MockLLMService** | ✅ | 已有 mock 基础设施 |
| **有测试闭环** | ✅ | pytest + eval report |

### 4.6 最终二开 PRD 固化 ✅

| 字段 | 内容 |
|---|---|
| **Current Phase 1 Scope** | RAG eval harness + demo seed data + eval report |
| **Future Possible Scope** | Memory continuity test, bad case dashboard, human handoff rules |
| **Permanent Safety Boundaries** | 不修改核心代码、不破坏原测试、不引入重型依赖 |
| **Explicit Non-goals** | 不改 UI、不改架构、不加新 LLM provider |
| **验收标准** | pytest 通过 + eval report 生成 + seed demo 可运行 |

### 4.7 产品 / 工程设计基调 ✅

| 检查项 | 状态 | 说明 |
|---|---|---|
| **开源项目增强** | ✅ | 不是另起炉灶 |
| **优先增加 harness / demo data** | ✅ | 核心目标 |
| **不大改 UI** | ✅ | 不涉及前端 |
| **不大改架构** | ✅ | 纯新增测试和脚本 |
| **不引入重型依赖** | ✅ | 复用已有测试基础设施 |
| **不破坏原测试** | ✅ | 新增目录，不影响已有测试 |

### 4.8 页面 / 文件结构规划 ✅

**Phase 1 涉及文件**:

```
新增 (不修改已有文件):
  tests/rag_eval/
    ├── conftest.py                 # RAG eval 测试配置
    ├── test_retrieval_precision.py # 检索精度测试
    ├── test_answer_faithfulness.py # 回答忠实度测试
    ├── test_hallucination.py       # 幻觉检测
    ├── test_no_answer_fallback.py  # 无答案场景测试
    ├── golden_qa_pairs.json        # 测试数据集
    └── report_generator.py         # 评估报告生成

  scripts/
    ├── seed_demo.py                # Demo 数据种子脚本
    └── demo_data/
        ├── agents.json             # 预配置 agent
        ├── knowledge/              # 示例知识库文档
        └── conversations.json      # 预设对话示例
```

**不碰或少碰的核心文件**:
- `backend/services/llm_service.py` — LLM 抽象
- `backend/services/kb_retrieval_service.py` — RAG 检索
- `backend/services/kb_document_processor.py` — 文档处理
- `backend/models.py` — 数据模型
- `frontend-nextjs/` — 前端代码

### 4.9 核心交互 / 验收流程规划 ✅

| 步骤 | 操作 | 预期结果 |
|---|---|---|
| 1 | 准备 demo knowledge base | `scripts/demo_data/knowledge/` 目录包含示例文档 |
| 2 | 运行 seed demo | `python scripts/seed_demo.py` 成功填充 demo 数据 |
| 3 | 运行 RAG eval tests | `pytest tests/rag_eval/ -v` 全部通过 |
| 4 | 生成评估报告 | `rag_eval_report.json` + `rag_eval_report.md` 生成 |
| 5 | 验证指标 | 报告包含 precision, recall, faithfulness, hallucination rate |
| 6 | 验证原项目未破坏 | `pytest tests/ --ignore=tests/rag_eval/ -v` 全部通过 |

---

## 5. 5-Key Development Rules

### 5.1 小步迭代

**版本规划**:

| 版本 | 任务 | 状态 |
|---|---|---|
| v0.1-fork-baseline | fork 后原始基线 | ✅ 已完成 (pushed) |
| v0.2-setup-verified | 本地运行和测试验证完成 | ✅ 已完成 (pushed) |
| v0.3-phase1-plan | Phase 1 计划锁定 | ✅ 已完成 |
| v1.0-rag-eval-harness | 实现 RAG eval harness | ✅ 已完成 (pushed) |
| v1.1-demo-data | 实现 demo seed | ✅ 已完成 (pushed) |
| v1.2-docs-and-report | 文档与评估报告完成 | ⬜ 待完成 |
| v1.3-phase1-complete | Phase 1 完整验收 | ⬜ 待完成 |

**规则**:
- 准备阶段全部 v0
- 第一行代码二开开始进入 v1
- 不允许在 v0 阶段写功能代码
- v1 之后每个版本必须有可运行验收结果

### 5.2 模块拆分

**禁止一把梭**。RAG eval、demo seed、report、docs、tests 要拆清楚。

| 模块 | 目录 | 职责 |
|---|---|---|
| RAG Eval Tests | `tests/rag_eval/` | 检索精度、回答忠实度、幻觉检测 |
| Demo Seed | `scripts/seed_demo.py` | 一键填充 demo 数据 |
| Demo Data | `scripts/demo_data/` | agent 配置、知识库文档、对话示例 |
| Eval Report | `tests/rag_eval/report_generator.py` | 生成 JSON + Markdown 报告 |
| Docs | `docs/rag-evaluation.md` | 使用说明 |

### 5.3 数据分离

**原则**: demo data / fixtures / eval cases 不要写死在测试逻辑里。

| 数据类型 | 位置 | 说明 |
|---|---|---|
| Golden QA Pairs | `tests/rag_eval/golden_qa_pairs.json` | 测试问答对 |
| Demo Agents | `scripts/demo_data/agents.json` | 预配置 agent |
| Demo Knowledge | `scripts/demo_data/knowledge/` | 示例知识库文档 |
| Demo Conversations | `scripts/demo_data/conversations.json` | 预设对话示例 |

### 5.4 状态摘要

**每轮 Claude 必须输出**:
- 本轮目标
- 修改文件
- 测试结果
- 没做什么
- 下一步

### 5.5 科学检查

**每轮至少运行**:
- 项目已有的安全测试
- lint 检查
- build 检查

**如果项目已有 CLAUDE.md / AGENTS.md，必须遵守**:
- ✅ Basjoo 有 CLAUDE.md 和 AGENTS.md
- ✅ 已读取并理解项目规范

---

## 6. Phase 1 PRD Summary

### 6.1 任务名称

**RAG Evaluation Harness with Demo Data**

### 6.2 核心功能

| 功能 | 说明 |
|---|---|
| **RAG Eval Harness** | 系统化的 RAG 质量评估框架 |
| **Golden QA Pairs** | 测试问答对数据集 |
| **Retrieval Precision Test** | 检索精度: precision@k, recall@k, MRR |
| **Answer Faithfulness Test** | 回答忠实度: answer 是否基于 retrieved context |
| **Hallucination Detection** | 幻觉检测: answer 是否包含未检索到的信息 |
| **No-answer Fallback Test** | 无答案场景: 低相关性时是否正确返回空 |
| **Eval Report Generator** | 自动生成 JSON + Markdown 评估报告 |
| **Demo Seed Script** | 一键填充 demo agent + knowledge + conversations |

### 6.3 验收标准

1. `pytest tests/rag_eval/ -v` 全部通过 (mock mode)
2. 生成 RAG eval 报告 (`rag_eval_report.json` + `rag_eval_report.md`)
3. 报告包含: precision@k, recall@k, MRR, faithfulness score, hallucination rate, no-answer accuracy
4. `python scripts/seed_demo.py` 可一键运行, 填充 demo agent + knowledge + conversations
5. 不修改任何原有代码文件
6. 有 README 说明如何运行

### 6.4 风险

| 风险 | 级别 | 缓解 |
|---|---|---|
| Mock mode 无法验证真实 RAG 质量 | 低 | 框架先跑通, 后续用真实 API 验证 |
| golden_qa_pairs 数据量不足 | 低 | 先做 10-20 对, 后续扩展 |
| 与现有测试框架集成 | 低 | 复用 conftest.py fixture |

---

## 7. Versioning Rules

### 7.1 外层 CustomerOpsAgent_2 文档仓库

**用途**: 管理选型、审查、二开规划文档

**版本**:
- v0.1-audit：选型审查 ✅
- v0.2-deep-audit：深度审查 ✅
- v0.3-phase0-prep：二开准备完成 ✅

**规则**: 不进入 v1，因为外层仓库不承载二开代码。

### 7.2 Basjoo fork 仓库

**用途**: 正式代码二开

**版本**:
- v0.1-fork-baseline：fork 后原始基线
- v0.2-setup-verified：本地运行和测试验证完成
- v0.3-phase1-plan：Phase 1 计划锁定
- v1.0-rag-eval-harness：第一阶段代码实现完成
- v1.1-demo-data：demo data 完成
- v1.2-docs-and-report：文档与评估报告完成
- v1.3-phase1-complete：Phase 1 完整验收

**规则**:
- 准备阶段全部 v0
- 第一行代码二开开始进入 v1
- 不允许在 v0 阶段写功能代码
- v1 之后每个版本必须有可运行验收结果

### 7.3 Commit Message 规范

```
docs:       文档变更
feat:       新功能
test:       测试
fix:        修复
chore:      构建/工具变更
```

---

## 8. Risks and Boundaries

### 8.1 安全边界

| 边界 | 说明 |
|---|---|
| **不修改核心代码** | 不改动 backend/services/, backend/models.py, backend/api/ 等核心文件 |
| **不破坏原测试** | 新增目录，不影响已有测试 |
| **不引入重型依赖** | 复用已有测试基础设施 |
| **不改 UI** | 不涉及 frontend-nextjs/ |
| **不改架构** | 纯新增测试和脚本 |
| **不使用真实 API Key** | MockLLMService 跑通框架 |

### 8.2 风险矩阵

| 风险 | 级别 | 缓解措施 |
|---|---|---|
| Basjoo 项目较新，社区小 | 中 | 代码质量高，文档完善，风险可控 |
| 需要外部 API Key | 低 | MockLLMService + 少量真实调用 |
| Windows 开发体验 | 低 | Docker Desktop for Windows |
| 二开后上游更新 | 低 | Git fork + 定期 merge |
| Fork 未创建 | **高** | 需要先手动 fork |

### 8.3 禁止事项

- ❌ 不开始写 Basjoo 二开代码
- ❌ 不修改 candidates/basjoo 核心源码
- ❌ 不修改 selected/basjoo 核心源码
- ❌ 不提交 candidates/ 或 selected/
- ❌ 不提交 node_modules / .env / 缓存
- ❌ 不使用真实 API Key
- ❌ 不大规模重构
- ❌ 不从 0 创建客服 Agent
- ❌ 不忽略 license
- ❌ 不打 tag (除非明确允许)

---

## 9. Setup 验证结果 (v0.2)

> **验证日期**: 2026-06-19
> **Basjoo commit**: 6939926

### 9.0.1 后端验证

| 项目 | 结果 |
|---|---|
| **Python** | 3.11.5 |
| **venv** | ✅ 创建成功 |
| **pip install** | ✅ 依赖安装成功 |
| **pytest** | ✅ 267 passed, 36 failed (Qdrant 依赖), 1 skipped |

### 9.0.2 前端验证

| 项目 | 结果 |
|---|---|
| **npm install** | ✅ 538 packages |
| **npm run build** | ✅ 17 pages generated |
| **npm run test** | ✅ 125 passed (20 test files) |

### 9.0.3 Docker / 服务审查

| 服务 | 本地测试 | 说明 |
|---|---|---|
| **Redis** | 可选 | 测试自动 fallback |
| **PostgreSQL** | 可选 | 测试使用 SQLite |
| **Qdrant** | 需要 | RAG 功能依赖 |
| **Scrapling** | 需要 | URL 爬取依赖 |
| **nginx** | 不需要 | 开发环境不需要 |

### 9.0.4 风险总结

| 风险 | 级别 | 缓解 |
|---|---|---|
| Qdrant 不可用导致 36 个测试失败 | 低 | Docker Compose 启动 Qdrant |
| Windows 路径兼容性 | 低 | 已验证 venv 正常工作 |
| 缓存位置 | 低 | 已配置 .cache/ 目录 |

---

## 10. Ready / Not Ready Verdict

### 10.1 准备状态总览 (更新于 v0.2)

| 准备项 | 状态 | 说明 |
|---|---|---|
| **原始想法明确** | ✅ | 基于 Basjoo 做 RAG Eval Harness |
| **想法压力测试通过** | ✅ | 真实有价值、短期可完成、适合简历 |
| **PRD 固化** | ✅ | Phase 1 范围明确、验收标准清晰 |
| **设计基调确定** | ✅ | 纯新增、不破坏原项目 |
| **文件结构规划** | ✅ | tests/rag_eval/ + scripts/ |
| **验收流程规划** | ✅ | pytest + eval report + seed demo |
| **版本规则锁定** | ✅ | v0 准备、v1 开发 |
| **Fork 已创建** | ✅ | Strange-Men/basjoo 已创建 |
| **selected/basjoo 已 clone** | ✅ | 已 clone，origin/upstream 已配置 |

### 10.2 结论

**✅ Fork baseline 已建立，Setup 验证已完成**

**已完成**:
1. ✅ Strange-Men/basjoo fork 已创建
2. ✅ 已 clone 到 selected/basjoo
3. ✅ origin → Strange-Men/basjoo, upstream → haoyiyin/basjoo
4. ✅ v0.1-fork-baseline tag 已创建并推送
5. ✅ 后端依赖安装 + 测试运行 (267 passed, 36 failed, 1 skipped)
6. ✅ 前端依赖安装 + build 成功 + 测试全通过 (125 tests)
7. ✅ v0.2-setup-verified tag 已创建并推送

**下一步**:
1. 进入 v0.3-phase1-plan — Phase 1 计划锁定
2. 然后进入 v1 开发阶段

---

## 10. Next Step Prompt

### 10.1 Fork Baseline + Setup Verified ✅ 已完成

- ✅ Fork 已创建: Strange-Men/basjoo
- ✅ 已 clone 到 selected/basjoo
- ✅ origin → Strange-Men/basjoo
- ✅ upstream → haoyiyin/basjoo
- ✅ v0.1-fork-baseline tag 已创建并推送
- ✅ v0.2-setup-verified tag 已创建并推送
- ✅ 后端测试: 267 passed, 36 failed (Qdrant 依赖), 1 skipped
- ✅ 前端 build: 成功
- ✅ 前端测试: 125 passed

### 10.2 下一步: v0.3-phase1-plan

**Phase 1 计划锁定**:
1. 确认 Phase 1 范围: RAG eval harness + demo data
2. 确认文件结构: tests/rag_eval/ + scripts/
3. 确认验收标准: pytest 通过 + eval report 生成

### 10.3 计划锁定后

**进入 v1 开发**:
1. 创建分支: `git checkout -b phase1-rag-eval-harness`
2. 创建目录: `mkdir -p tests/rag_eval scripts/demo_data`
3. 开始实现 RAG eval harness

---

## 11. Product Walkthrough (v0.2.5)

> **验证日期**: 2026-06-19
> **Basjoo commit**: 6939926
> **启动方式**: 本地 Python + Node.js (无 Docker)

### 11.1 启动方式

| 服务 | 启动命令 | 端口 | 状态 |
|---|---|---|---|
| **Backend** | `cd backend && PYTHONIOENCODING=utf-8 ./venv/Scripts/python.exe main.py` | 8000 | ✅ 运行成功 |
| **Frontend** | `cd frontend-nextjs && npm run dev` | 3000 | ✅ 运行成功 |
| **Redis** | 不可用 | 6379 | ⚠️ 自动 fallback 到内存限流 |
| **Qdrant** | 不可用 | 6333 | ⚠️ RAG 功能不可用 |
| **PostgreSQL** | 不可用 | 5432 | ⚠️ 使用 SQLite |

**关键发现**:
- Backend 可以在没有 Redis/Qdrant/PostgreSQL 的情况下启动
- Redis 不可用时自动 fallback 到内存限流
- 使用 SQLite 作为数据库
- Windows 需要设置 `PYTHONIOENCODING=utf-8` 解决中文编码问题

### 11.2 首页 / Dashboard

**访问地址**: http://localhost:3000

**页面流程**:
1. 访问 `/` → 重定向到 `/login` (需要登录)
2. 访问 `/login` → 显示登录页面
3. 首次访问 `/register` → 检查 `registration-settings` API
4. 如果 `bootstrap_required=true` → 显示注册页面
5. 如果 `bootstrap_required=false` → 重定向到 `/login`

**登录页面元素**:
- Logo + "Basjoo" 标题 (渐变色)
- "登录您的管理账号" 副标题
- 邮箱输入框
- 密码输入框
- 登录按钮
- 语言切换器 (中文/English)

**注册页面元素**:
- "初始设置" 标题
- "创建第一个超级管理员账号" 副标题
- 姓名、邮箱、密码、确认密码输入框
- "创建超级管理员" 按钮

### 11.3 Agent 管理

**路由**: `/agents`

**功能**:
- 创建 Agent (需要超级管理员权限)
- Agent 类型: website_support, ai_clone, sales_outreach, custom
- Agent 渠道: web_widget, whatsapp, email, custom
- Agent 名称、描述、类型、渠道配置
- Agent 激活/停用
- Agent 自动删除倒计时 (30天不活跃)

**创建流程**:
1. 点击 "创建 Agent"
2. 填写名称、描述
3. 选择 Agent 类型
4. 选择渠道模式
5. 创建成功后触发 KB Setup Wizard

### 11.4 Agent Settings

**路由**: `/agents/{agentId}/settings/agent`

**配置项**:
- Widget 标题、颜色、欢迎消息
- 历史记录保留天数 (默认30天)
- 允许的 Widget 来源白名单
- 每分钟对话限制
- 受限回复消息
- 嵌入代码生成

**嵌入代码示例**:
```html
<script src="http://localhost:8000/sdk.js"></script>
<script>
  new BasjooWidget({
    agentId: 'agt_xxxxxxxxxxxx',
    apiBase: 'http://localhost:8000',
    themeColor: '#06B6D4',
    title: 'AI客服助手',
    welcomeMessage: '您好！我是AI助手，有什么可以帮助您的吗？',
    language: 'zh-CN',
    position: 'right'
  }).init();
</script>
```

### 11.5 Knowledge Base

**路由**: `/agents/{agentId}/knowledge`

**功能**:
- KB 初始化向导
- Embedding 提供商选择: Jina, SiliconFlow, Custom
- Embedding API Key 配置
- KB 状态检查
- KB 重置

**初始化流程**:
1. 选择 Embedding 提供商
2. 输入 API Key
3. 测试连接
4. 初始化 KB
5. 配置锁定 (只能重置)

**关键发现**:
- 没有 Embedding API Key 时无法使用知识库功能
- KB 初始化后配置锁定，只能重置
- 支持 Jina, SiliconFlow, Custom OpenAI Compatible

### 11.6 URL 管理

**路由**: `/agents/{agentId}/urls`

**功能**:
- 添加 URL 知识源
- 单页面爬取
- 站点爬取 (深度、最大页面数)
- URL 列表管理
- 自动抓取设置
- 重新抓取
- 清除所有

**爬取流程**:
1. 输入 URL
2. 选择爬取方式 (单页面/站点)
3. 开始爬取
4. 爬取完成后需要重建索引

**关键发现**:
- 需要 Scrapling 微服务
- 支持静态 HTML 页面
- 自动提取主要内容
- 最多50个 URL 源

### 11.7 文件管理

**路由**: `/agents/{agentId}/files`

**功能**:
- 文件上传 (拖拽/点击)
- 支持格式: PDF, TXT, MD, HTML, DOCX, XLSX
- 文件列表管理
- 文件状态: pending, processing, ready, error
- 清除所有

**上传流程**:
1. 拖拽或选择文件
2. 文件上传到后端
3. 后端解析文件内容
4. 分块并生成 embedding
5. 存储到 Qdrant

**关键发现**:
- 需要 Embedding API Key
- 支持多种文档格式
- 最大20MB文件大小
- 最多5个文件同时上传

### 11.8 Playground

**路由**: `/agents/{agentId}/playground`

**功能**:
- AI 对话测试
- 设置预览
- 调试信息
- 重建索引
- 清除对话

**对话流程**:
1. 输入问题
2. 发送到后端
3. 后端检索知识库
4. 生成回答
5. 流式返回

**关键发现**:
- 需要配置 LLM API Key
- 需要配置 Embedding API Key (用于知识库检索)
- 支持流式响应
- 显示引用来源

### 11.9 Sessions / Human Handoff

**路由**: `/agents/{agentId}/sessions`

**功能**:
- 会话列表
- 会话状态: active, taken_over, closed
- 会话详情
- 人工接管
- 释放会话
- 搜索访客ID或消息内容

**会话流程**:
1. 访客通过 Widget 发起对话
2. AI 自动回复
3. 管理员查看会话列表
4. 管理员接管会话
5. 管理员手动回复
6. 释放会话恢复 AI 回复

**关键发现**:
- 支持 WebSocket 实时通信
- 支持人工接管
- 会话状态管理
- 访客信息 (国家、城市)

### 11.10 User Management

**路由**: `/agents/{agentId}/users`

**功能**:
- 用户列表
- 添加用户
- 编辑用户
- 删除用户
- 角色管理: super_admin, admin, support

**角色权限**:
- super_admin: 完全访问权限
- admin: Agent 管理权限
- support: 只能访问 sessions/chat

### 11.11 Widget

**访问地址**: http://localhost:8000/widget-demo

**功能**:
- 可嵌入的聊天组件
- 自动检测 API Base
- 流式响应
- 引用来源显示
- 多语言支持
- 响应式设计

**嵌入方式**:
```html
<script src="http://localhost:8000/sdk.js"></script>
<script>
  new BasjooWidget({
    agentId: 'agt_xxxxxxxxxxxx',
    apiBase: 'http://localhost:8000'
  }).init();
</script>
```

**关键发现**:
- 自动从 script src 检测 API Base
- 支持 localStorage 持久化会话
- 支持流式响应
- 支持引用来源显示
- 支持多语言

### 11.12 无 API Key 时的表现

| 功能 | 无 API Key 时 | 说明 |
|---|---|---|
| **登录/注册** | ✅ 可用 | 不需要 API Key |
| **Agent 创建** | ✅ 可用 | 不需要 API Key |
| **Agent Settings** | ✅ 可用 | 不需要 API Key |
| **User Management** | ✅ 可用 | 不需要 API Key |
| **Knowledge Base** | ❌ 不可用 | 需要 Embedding API Key |
| **URL 管理** | ⚠️ 部分可用 | 可以添加 URL，但爬取需要 Scrapling |
| **文件管理** | ❌ 不可用 | 需要 Embedding API Key |
| **Playground** | ❌ 不可用 | 需要 LLM API Key |
| **Sessions** | ⚠️ 部分可用 | 可以查看列表，但无法对话 |
| **Widget** | ❌ 不可用 | 需要 LLM API Key |

### 11.13 RAG / Knowledge Base 体验结论

**RAG 管道**:
1. URL/File → 内容提取
2. 内容 → 分块 (chunking)
3. 分块 → Embedding (Jina/SiliconFlow/Custom)
4. Embedding → Qdrant 存储
5. 查询 → Embedding → Qdrant 检索 → 相似度排序
6. 检索结果 + 查询 → LLM 生成回答

**关键发现**:
- RAG 管道完整，支持 URL 和文件
- 需要 Embedding API Key 才能使用
- 支持多种 Embedding 提供商
- 支持相似度阈值配置
- 支持 Top-K 结果数量配置

### 11.14 Sessions / Human Handoff 体验结论

**会话管理**:
- 会话状态: active, taken_over, closed
- 会话详情: 访客ID、国家、城市、消息数
- 会话操作: 接管、释放、查看历史

**人工接管**:
- 管理员可以接管会话
- 接管后 AI 停止回复
- 管理员可以手动回复
- 释放后恢复 AI 回复

**关键发现**:
- 支持 WebSocket 实时通信
- 支持人工接管
- 会话状态管理完善
- 访客信息丰富

### 11.15 后续 Harness 机会

| 机会 | 页面/API | 说明 |
|---|---|---|
| **RAG Eval Harness** | `/api/v1/chat/stream` | 测试检索精度、回答忠实度、幻觉检测 |
| **Demo Data** | Agent + KB + Sessions | 预配置 agent + 知识库 + 对话示例 |
| **No-API-Key Fallback** | 所有页面 | 测试无 API Key 时的表现 |
| **Bad Case Report** | Playground | 生成评估报告 |
| **截图展示** | Dashboard + Playground + Widget | 作品集展示 |

### 11.16 Phase 1 是否仍然推荐 RAG Eval Harness + Demo Data

**✅ 仍然推荐**

**理由**:
1. RAG 管道完整，适合做 eval harness
2. 需要 Embedding API Key 才能测试 RAG
3. Demo Data 可以预配置 agent + 知识库
4. 评估报告可以展示 RAG 质量
5. 适合作品集展示

**建议**:
1. Phase 1: RAG Eval Harness + Demo Data (MockLLMService)
2. Phase 2: 真实 API Key 验证
3. Phase 3: Bad Case Dashboard

---

## 12. Phase 1 Plan Lock (v0.3-phase1-plan)

> **Plan date**: 2026-06-19
> **Status**: ✅ Phase 1 plan locked

### 12.1 Phase 1 Plan Summary

| 字段 | 内容 |
|---|---|
| **Phase 1 名称** | RAG Evaluation Harness + Demo Data |
| **计划文档** | `PHASE1_PLAN.md` |
| **计划状态** | ✅ 已锁定 |
| **是否修改源码** | ❌ 否，纯计划 |
| **是否进入 v1** | ❌ 否，仍在 v0 阶段 |

### 12.2 Phase 1 范围确认

**RAG Evaluation Harness**：
- 新增 `tests/rag_eval/` — RAG 质量评估测试框架
- 覆盖：检索精度、无答案 fallback、evidence/citation、幻觉风险
- 至少 8-10 个 eval cases
- 支持 mock mode（不需要真实 API Key）

**Demo Data**：
- 新增 `scripts/demo_data/` — 预配置 demo 数据
- 包含：demo agent、知识库文档、问答对、bad cases
- 新增 `scripts/seed_demo_data.py` — 一键填充脚本

**Eval Report**：
- 新增 `backend/reports/` — 评估报告目录
- 生成 JSON + Markdown 格式报告
- 包含量化指标：precision@k, recall@k, MRR, faithfulness, hallucination rate

### 12.3 不做什么（Non-goals）

- ❌ 不重写 Basjoo 核心架构
- ❌ 不重做前端 UI
- ❌ 不接真实客服系统
- ❌ 不接真实生产 API Key
- ❌ 不引入 LangGraph / RAGAS / DeepEval / Mem0
- ❌ 不改变现有数据库主结构
- ❌ 不破坏原有测试
- ❌ 不删除原有功能

### 12.4 验收标准

1. ✅ 不需要真实 API Key 也能运行基础 RAG eval harness
2. ✅ 至少包含 8-10 个 eval cases
3. ✅ 覆盖：正常命中、多文档检索、无答案 fallback、低相关度拒答、evidence/citation、幻觉风险、中文、英文
4. ✅ pytest 能运行新增 harness
5. ✅ 原有核心测试不被破坏
6. ✅ 能生成 eval report
7. ✅ demo data 能被 seed 或作为 fixtures 使用
8. ✅ 文档说明如何运行

### 12.5 下一步

**进入 v1.0-rag-eval-harness**：
1. 创建分支：`git checkout -b phase1-rag-eval-harness`
2. 创建目录：`mkdir -p tests/rag_eval/fixtures scripts/demo_data reports docs`
3. 开始实现 RAG eval harness

**预计时间**：7 天

---

*Document created: 2026-06-19*
*Phase 0 preparation confirmed*
*Fork baseline: v0.1-fork-baseline (2026-06-19)*
*Setup verified: v0.2-setup-verified (2026-06-19)*
*Product walkthrough: v0.2.5-product-walkthrough (2026-06-19)*
*Phase 1 plan locked: v0.3-phase1-plan (2026-06-19)*
*Next step: v1.0-rag-eval-harness — 实现 RAG eval harness*
