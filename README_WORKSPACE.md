# CustomerOpsAgent_2｜Open-source AI Customer Support Agent Enhancement

## 目的

本工作区用于筛选、审查、并最终对开源 AI 客服 Agent 项目进行二次开发。

**不从 0 自研**，只做开源项目的增强和定制。

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
v1.3-phase1-complete # Phase 1 完整验收
```

**规则**:
- 准备阶段全部 v0
- 第一行代码二开开始进入 v1
- 不允许在 v0 阶段写功能代码
- v1 之后每个版本必须有可运行验收结果

当前阶段：`v0.2-setup-verified`（setup 验证完成，tag 已推送到 origin）

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
10. 在 fork 分支 phase1-rag-eval-harness 上开始二开 ⬜

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
- 分支: phase1-rag-eval-harness (待创建)
- 版本: v0.1-fork-baseline ✅ → v0.2-setup-verified ✅ → v1.0-rag-eval-harness → v1.3-phase1-complete

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

- 新增 `tests/rag_eval/` — RAG 质量评估测试框架
- 新增 `scripts/seed_demo.py` — Demo 数据种子脚本
- 不修改原项目核心代码
- 不依赖真实 API Key (mock mode)

**Setup 验证确认**:
- ✅ MockLLMService 可用，不需要真实 API Key
- ✅ 后端测试框架 (conftest.py) 支持隔离测试
- ✅ 前端测试框架 (Vitest) 正常工作
- ✅ Windows 环境兼容

详见 OPEN_SOURCE_AUDIT.md 第 17 章和第 20 章。

## 技术栈偏好

- Python / FastAPI（后端）
- TypeScript / React / Next.js（前端）
- PostgreSQL + 向量数据库（数据层）
- Docker Compose（部署）
- MIT / Apache 2.0（license 优先）

---

## 当前状态

**当前阶段**：v0.3-phase1-plan（Phase 1 计划锁定）

**已完成**：
- ✅ v0.1-audit：选型审查完成
- ✅ v0.2-deep-audit：深度审查完成
- ✅ v0.3-phase0-prep：二开准备完成
- ✅ v0.1-fork-baseline：Fork 基线建立
- ✅ v0.2-setup-verified：Setup 验证完成
- ✅ v0.2.5-product-walkthrough：产品体验完成
- ✅ v0.3-phase1-plan：Phase 1 计划锁定

**下一步**：
- ⬜ v1.0-rag-eval-harness：实现 RAG eval harness
- ⬜ v1.1-demo-data：实现 demo data
- ⬜ v1.2-docs-and-report：文档与评估报告
- ⬜ v1.3-phase1-complete：Phase 1 完整验收

**Phase 1 任务**：RAG Evaluation Harness + Demo Data

详见 `PHASE1_PLAN.md`。

---

*Created: 2026-06-19*
*Last updated: 2026-06-19 (phase1 plan locked: v0.3-phase1-plan)*
