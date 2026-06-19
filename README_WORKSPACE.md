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
└── selected/                    # 最终选定的二开项目（待确定）
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

```
v0.1-audit       # 选型审查完成
v0.2-setup       # 环境搭建完成
v0.3-enhancement # 二开增强完成
v1.0-demo        # 作品集 demo 完成
```

当前阶段：`v0.1-audit`（本轮不打 tag，除非明确允许）

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
6. Fork 原项目到 Strange-Men/basjoo ⬜ (下一步)
7. 在 fork 分支 phase1-rag-eval-harness 上开始二开 ⬜

## 选定项目

**最终推荐**: [Basjoo](https://github.com/haoyiyin/basjoo)
- License: MIT
- 技术栈: Python/FastAPI + Next.js 14 + PostgreSQL + Qdrant + Redis
- 代码规模: 7.2MB, 96 Python + 109 TypeScript files
- 二开难度: 低-中

**第二备选**: [TGO](https://github.com/tgoai/tgo)
- License: Modified Apache 2.0
- 二开难度: 中 (微服务架构较复杂)

## Git 策略

### 外层仓库 (CustomerOpsAgent_2)

- 只管理审查文档和二开规划
- 不包含任何候选项目源码
- candidates/ 和 selected/ 被 .gitignore 忽略

### 二开仓库 (fork)

- Fork haoyiyin/basjoo → Strange-Men/basjoo
- origin: https://github.com/Strange-Men/basjoo
- upstream: https://github.com/haoyiyin/basjoo
- 分支: phase1-rag-eval-harness

## Phase 1 二开任务

**RAG Evaluation Harness + Demo Data**

- 新增 `tests/rag_eval/` — RAG 质量评估测试框架
- 新增 `scripts/seed_demo.py` — Demo 数据种子脚本
- 不修改原项目核心代码
- 不依赖真实 API Key (mock mode)

详见 OPEN_SOURCE_AUDIT.md 第 17 章。

## 技术栈偏好

- Python / FastAPI（后端）
- TypeScript / React / Next.js（前端）
- PostgreSQL + 向量数据库（数据层）
- Docker Compose（部署）
- MIT / Apache 2.0（license 优先）

---

*Created: 2026-06-19*
*Last updated: 2026-06-19 (deep audit complete)*
