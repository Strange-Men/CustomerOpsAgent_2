# CustomerOpsAgent_2｜下一阶段工程计划

> **Created**: 2026-06-20
> **Purpose**: 锁定后续开发路线，防止方向漂移
> **Status**: Active

---

## 1. Current State

### 项目真实状态

- Phase 1 已完成（v1.3-phase1-complete）
- 37 个 pytest 测试全部通过
- 15 个 eval cases 全部通过（mock 模式）
- baseline 无新增失败
- 所有评估基于 mock 组件，不涉及真实 Qdrant / Embedding / LLM
- CAREER_PACKAGE.md 已存在，但不是工程主线
- **v1.4-phase2-readiness-audit 已完成（2026-06-20）**
- **Go/No-Go 结论：CONDITIONAL GO**

### 仓库状态

- 外层仓库（CustomerOpsAgent_2）：main 分支，clean
- 二开仓库（basjoo）：phase1-rag-eval-harness 分支，clean
- 两个仓库均为 clean，无未提交更改

---

## 2. Why We Are Resetting Direction

### 原因

1. **项目开始出现简历包装漂移**：v1.3 的主要工作是中文包装文档，CAREER_PACKAGE.md 是最近的 commit
2. **需要重新回到工程目标**：后续开发必须以功能闭环为中心，而不是以包装为中心
3. **开发中期不做包装**：未闭环阶段只做工程记录，不做简历话术

### 什么是工程记录

- 阶段记录
- 开发记录
- 状态总结
- 工程计划

### 什么是包装（开发中期禁止）

- 简历包装
- 作品集包装
- 面试话术
- 面向 HR 的文档

---

## 3. Recommended Next Stage

**v1.4 已完成**：Phase 2 Readiness Audit（2026-06-20）

**v1.5 已完成**：Phase 2 Condition Resolution（2026-06-20）

**v1.6 已完成**：Phase 2 Environment Setup（2026-06-20）

**用户决策**：
- Qdrant 方案：Docker Desktop + 本地 Qdrant
- Embedding 方案：SiliconFlow
- 时间上限：1 周
- Phase 2 最小目标：真实 Qdrant retrieval eval

**v1.6.2 最终环境验证结果（2026-06-23）**：
- Docker：✅ v29.5.3
- Docker Compose：✅ v5.1.4
- WSL：✅ v2.7.8.0 (WSL2)
- Qdrant：✅ basjoo-qdrant 运行中，v1.18.2
- Qdrant API：✅ /health OK, /collections OK
- .env：✅ 存在且被 gitignore，所有 5 个变量已配置
- SILICONFLOW_API_KEY：✅ 已配置
- EMBEDDING_PROVIDER：✅ siliconflow
- SiliconFlow 连通性：✅ HTTP 200, Qwen/Qwen3-Embedding-0.6B, 维度 1024
- Qdrant CRUD：✅ 创建/插入/查询/删除 全部通过
- Basjoo 原生 SiliconFlow 支持：✅ 不需要修改核心源码
- 结论：READY FOR v2.0

**当前状态**：READY FOR v2.0

**下一步**：
1. 进入 v2.0-real-qdrant-eval-adapter
2. 扩展 `seed_demo_data.py`：`--write-db` 模式写入 Agent DB
3. 扩展 `run_rag_eval.py`：`--real` 模式使用真实 Qdrant + SiliconFlow
4. 运行 3-5 个 real retrieval eval cases
5. 生成 mock vs real 对比报告

详见 `PHASE2_ENV_VERIFICATION.md`。

详见 `PHASE2_ENV_SETUP.md`。

---

## 4. v1.4-phase2-readiness-audit Scope

> **Status**: ✅ 已完成（2026-06-20）
> **输出物**: `PHASE2_READINESS_AUDIT.md`
> **结论**: CONDITIONAL GO

### 审查完成内容

- ✅ 检查当前 Basjoo 的真实 RAG 依赖（Qdrant、Embedding API、LLM API）
- ✅ 检查 Qdrant 启动方式（Docker / 本地二进制 / 云服务）
- ✅ 检查 Docker 不可用时的替代方案
- ✅ 检查是否能用本地 Qdrant、远程 Qdrant 还是 mock-only
- ✅ 检查 Embedding API Key 是否必需（Jina / SiliconFlow / OpenAI）
- ✅ 检查真实 eval runner 改造点（需要改哪些文件、改多少）
- ✅ 检查 Windows 本地运行 Qdrant 的可行性
- ✅ 检查成本（API 调用费用、Qdrant 部署成本）
- ✅ 输出 Go / No-Go 结论：CONDITIONAL GO

### 关键发现

- Docker 未安装，无法用 docker compose 启动 Qdrant
- WSL 状态不明
- Python 3.11.5 / Node v24.15.0 / npm 11.12.1 均可用
- Qdrant Cloud 免费层是最简替代方案
- 代码改动量小：只需扩展 run_rag_eval.py 和 seed_demo_data.py

---

## 5. If Go

如果 v1.4 审查通过，下一步才是：

```
v2.0-real-qdrant-eval-adapter
```

### v2.0 可能的范围

- 实现真实 Qdrant 连接
- 实现真实 Embedding provider 集成
- 将 demo knowledge base 写入真实向量库
- 替换 MockRetriever 为真实 KbRetrievalService
- 运行真实 eval 并与 mock eval 结果对比
- 生成真实 RAG eval report

### v2.0 前置条件

- Qdrant 可用（Docker 或其他方式）
- Embedding API Key 可用
- Windows 本地运行方案确认
- 成本可接受

---

## 6. If No-Go

如果 v1.4 审查不通过，则：

- 冻结 Basjoo 项目
- 回到 CodePilot 或 Portfolio 主线
- 保留 Phase 1 作为阶段性完成成果
- 不再继续 Basjoo 开发

### 冻结后的处理

- Phase 1 代码和文档保持不变
- CAREER_PACKAGE.md 保留但不再扩展
- 可以在其他项目上继续开发

---

## 7. Timeline

| 阶段 | 时间 | 内容 | 状态 |
|---|---|---|---|
| v1.4-phase2-readiness-audit | 2026-06-20 | 审查、评估、Go/No-Go 决策 | ✅ 完成 |
| v1.5-phase2-condition-resolution | 2026-06-20 | 环境检查、方案比较、条件确认 | ✅ 完成 |
| v1.6-phase2-environment-setup | 2026-06-20 | Docker/Qdrant/SiliconFlow 环境准备 | ✅ 完成 |
| v1.6.2-phase2-env-final-verification | 2026-06-23 | 最终环境验证，全部通过 | ✅ 完成 |
| v2.0-real-qdrant-eval-adapter | 1 周上限 | 实现真实 RAG 集成 | ⬜ READY |

---

## 8. Decision Criteria

### v1.4 审查结论：CONDITIONAL GO

**条件清单**：

| 序号 | 条件 | 当前状态 | 必须满足 |
|---|---|---|---|
| 1 | Qdrant 可用（Docker 或 Qdrant Cloud） | ✅ Docker Desktop + basjoo-qdrant | ✅ 是 |
| 2 | Embedding API Key 可用 | ✅ SiliconFlow API Key 已配置 | ✅ 是 |
| 3 | Windows 本地运行方案确认 | ✅ Docker Desktop + WSL2 | ✅ 是 |
| 4 | 成本可接受 | ✅ 免费额度足够 | ✅ 是 |
| 5 | 改造工作量可控（≤1 周） | ✅ 只需扩展 2 个脚本 | ✅ 是 |
| 6 | 有明确的时间上限 | ✅ 1 周 | ✅ 是 |

### 如果条件不满足 → NO-GO

- 冻结 Basjoo 项目
- 回到 CodePilot 主线
- 保留 Phase 1 作为阶段性完成成果

---

*Document created: 2026-06-20*
*Purpose: Lock down next engineering steps after Phase 1 completion*
