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

**推荐下一阶段为**：

```
v1.4-phase2-readiness-audit
```

**注意**：这不是 Phase 2 功能开发，而是 Phase 2 准入检查。

**目标**：评估是否值得进入 Phase 2，以及如何进入 Phase 2。

---

## 4. v1.4-phase2-readiness-audit Scope

### 允许做

- 检查当前 Basjoo 的真实 RAG 依赖（Qdrant、Embedding API、LLM API）
- 检查 Qdrant 启动方式（Docker / 本地二进制 / 云服务）
- 检查 Docker 不可用时的替代方案
- 检查是否能用本地 Qdrant、远程 Qdrant 还是 mock-only
- 检查 Embedding API Key 是否必需（Jina / SiliconFlow / OpenAI）
- 检查真实 eval runner 改造点（需要改哪些文件、改多少）
- 检查 Windows 本地运行 Qdrant 的可行性
- 检查成本（API 调用费用、Qdrant 部署成本）
- 输出是否进入 Phase 2 的 Go / No-Go 结论

### 不允许做

- 不写真实 Qdrant adapter
- 不接真实 API Key
- 不写新功能
- 不部署
- 不改 UI
- 不继续求职包装
- 不修改任何 Basjoo 源码

### 输出物

- 一份 Readiness Audit 报告（外层文档）
- Go / No-Go 结论
- 如果 Go：Phase 2 的具体工程计划
- 如果 No-Go：冻结方案

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

| 阶段 | 时间 | 内容 |
|---|---|---|
| v1.4-phase2-readiness-audit | 1-2 天 | 审查、评估、Go/No-Go 决策 |
| v2.0-real-qdrant-eval-adapter | 1-2 周 | 如果 Go，实现真实 RAG 集成 |
| 冻结 | 立即 | 如果 No-Go，冻结项目 |

---

## 8. Decision Criteria

### Go 条件

- Windows 本地可以运行 Qdrant（Docker 或其他方式）
- Embedding API Key 可以获取（免费额度或低成本）
- 改造工作量可控（不超过 1 周）
- 投入产出比合理

### No-Go 条件

- Windows 本地无法运行 Qdrant
- API Key 成本过高
- 改造工作量过大
- 投入产出比不合理
- 有更重要的项目需要投入

---

*Document created: 2026-06-20*
*Purpose: Lock down next engineering steps after Phase 1 completion*
