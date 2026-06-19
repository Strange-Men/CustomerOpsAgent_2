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
| **状态** | ❌ 不存在 |
| **原因** | Strange-Men/basjoo fork 尚未创建 |

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
| **Fork 存在** | ❌ | https://github.com/Strange-Men/basjoo 不存在 |
| **selected/basjoo 已 clone** | ❌ | 目录不存在 |
| **upstream 已设置** | ❌ | 未设置 |

### 3.2 ⚠️ 需要手动 Fork

**下一步操作**：请在 GitHub 上手动 fork 原项目。

1. 访问 https://github.com/haoyiyin/basjoo
2. 点击右上角 "Fork" 按钮
3. 选择你的 GitHub 账号 (Strange-Men)
4. 保持默认设置，点击 "Create fork"
5. 等待 fork 完成

### 3.3 Fork 后的预期结构

```
D:\Claude_workfile\CustomerOpsAgent_2\
  candidates\
    basjoo\          # 原项目审查副本，只读 (已存在)
  selected\
    basjoo\          # 我的 fork，正式二开 (待创建)
```

**Remote 配置**：
- `origin` → https://github.com/Strange-Men/basjoo (你的 fork)
- `upstream` → https://github.com/haoyiyin/basjoo (原项目)

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
| v0.1-fork-baseline | fork 后原始基线 | ⬜ 待完成 |
| v0.2-setup-verified | 本地运行和测试验证完成 | ⬜ 待完成 |
| v0.3-phase1-plan | Phase 1 计划锁定 | ✅ 已完成 |
| v1.0-rag-eval-harness | 实现 RAG eval harness | ⬜ 待完成 |
| v1.1-demo-data | 实现 demo seed | ⬜ 待完成 |
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

## 9. Ready / Not Ready Verdict

### 9.1 准备状态总览

| 准备项 | 状态 | 说明 |
|---|---|---|
| **原始想法明确** | ✅ | 基于 Basjoo 做 RAG Eval Harness |
| **想法压力测试通过** | ✅ | 真实有价值、短期可完成、适合简历 |
| **PRD 固化** | ✅ | Phase 1 范围明确、验收标准清晰 |
| **设计基调确定** | ✅ | 纯新增、不破坏原项目 |
| **文件结构规划** | ✅ | tests/rag_eval/ + scripts/ |
| **验收流程规划** | ✅ | pytest + eval report + seed demo |
| **版本规则锁定** | ✅ | v0 准备、v1 开发 |
| **Fork 已创建** | ❌ | **需要先手动 fork** |
| **selected/basjoo 已 clone** | ❌ | **需要 fork 后 clone** |

### 9.2 结论

**⚠️ 准备基本完成，但 Fork 未创建**

**阻塞项**:
1. Strange-Men/basjoo fork 不存在
2. selected/basjoo 目录不存在

**下一步**:
1. 在 GitHub 上手动 fork haoyiyin/basjoo → Strange-Men/basjoo
2. Clone fork 到 selected/basjoo
3. 设置 upstream
4. 运行本地测试验证
5. 进入 v1 开发阶段

---

## 10. Next Step Prompt

### 10.1 立即执行

**手动 Fork**:
1. 访问 https://github.com/haoyiyin/basjoo
2. 点击 "Fork" 按钮
3. 选择 Strange-Men 账号
4. 等待 fork 完成

### 10.2 Fork 后执行

**Clone 并设置**:
```bash
# Clone fork
git clone https://github.com/Strange-Men/basjoo.git "D:\Claude_workfile\CustomerOpsAgent_2\selected\basjoo"

# 进入目录
cd "D:\Claude_workfile\CustomerOpsAgent_2\selected\basjoo"

# 设置 upstream
git remote add upstream https://github.com/haoyiyin/basjoo.git

# 验证 remote
git remote -v
```

### 10.3 验证后执行

**本地测试验证**:
```bash
# 进入后端目录
cd backend

# 创建虚拟环境
python3 -m venv venv
source venv/Scripts/activate  # Windows

# 安装依赖
pip install -r requirements.txt

# 运行测试
pytest tests/ --ignore=tests/integration/ -v
```

### 10.4 验证通过后

**进入 v1 开发**:
1. 创建分支: `git checkout -b phase1-rag-eval-harness`
2. 创建目录: `mkdir -p tests/rag_eval scripts/demo_data`
3. 开始实现 RAG eval harness

---

*Document created: 2026-06-19*
*Phase 0 preparation confirmed*
*Next step: Manual fork on GitHub*
