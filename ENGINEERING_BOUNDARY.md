# CustomerOpsAgent_2｜工程边界定义

> **Created**: 2026-06-20
> **Purpose**: 恢复工程主线，设立项目边界，防止方向漂移
> **Status**: Active

---

## 1. Project Mission

**项目使命**：

不是重写客服系统，而是增强开源 AI 客服系统的 RAG 评估与工程可验证能力。

基于开源 AI 客服系统 Basjoo 的工程质量增强项目。核心方向是为现有 AI 客服系统补充 RAG Evaluation Harness、Demo Data、Mock 可复现评估、评估报告和后续真实 RAG 评估扩展能力。

---

## 2. What Phase 1 Has Completed

Phase 1 真实完成内容：

- RAG Evaluation Harness（15 eval cases, 6 类场景）
- Mock Embedding（字符频率 256 维向量，无需 API Key）
- Mock Retriever（内存余弦相似度，无需 Qdrant）
- Mock RAG Pipeline（提取式答案，无需 LLM）
- 37 个 pytest 测试全部通过
- SmartHome Demo Data（2 Agent, 3 文档, 15 问题, 3 对话, 8 Bad Cases）
- seed_demo_data.py（支持 --validate-only / --dry-run / --mock）
- JSON + Markdown 评估报告生成
- No-API-Key / No-Qdrant 可复现评估
- baseline 无新增失败（267 passed, 36 failed, 1 skipped）
- 独立 eval runner（run_rag_eval.py --mock: 15 passed, 0 failed）

---

## 3. What Phase 1 Has Not Completed

Phase 1 明确未完成的内容：

- 没有真实 Qdrant 检索评估
- 没有真实 Embedding API 评估
- 没有真实 LLM 问答质量评估
- 没有线上部署
- 没有前端 Eval Dashboard
- 没有把 Basjoo 变成自己的完整客服产品
- 没有多轮对话评估
- 没有延迟/性能指标
- 没有自动化的 eval case 生成

---

## 4. Project Completion Boundary

### 当前阶段可以称为

```
Phase 1 Complete：Mock-friendly RAG Evaluation Harness Enhancement
```

### 不能称为

```
完整 AI 客服系统完成
线上生产级 RAG 评估
真实 RAG 质量验证
```

### 后续决策

- 如果项目整体要继续，则进入 Phase 2
- 如果不继续，则以 Phase 1 作为阶段性完成产物冻结

---

## 5. Resume Packaging Freeze Rule

### 规则

1. **未闭环阶段不做简历包装**
2. **未闭环阶段只做工程记录**（阶段记录 / 开发记录 / 状态总结 / 工程计划）
3. CAREER_PACKAGE.md 保留为 Phase 1 完成后的附属材料，不是工程主线
4. 后续 Phase 2 开发期间，不继续更新简历话术
5. 只有 Phase 2 闭环后，才允许新增 Phase 2 的求职包装
6. 开发中期不写"简历包装 / 作品集包装 / 面试话术"

### 当前状态

Phase 1 已闭环，CAREER_PACKAGE.md 作为 Phase 1 的附属材料已存在。后续不再继续扩展，除非新阶段闭环。

---

## 6. Phase 2 Candidate Directions

只列工程方向，不写包装方向。

### 方向 A：Real Qdrant Evaluation Adapter

**目标**：
- 接入真实 Qdrant 服务
- 用 demo knowledge base 写入真实向量库
- 用真实或可配置 embedding provider
- 比较 mock eval 与 real retrieval eval 结果

**前置条件**：
- 确认可用的 Qdrant 部署方式（Docker / 本地二进制 / 云服务）
- 确认 Embedding API Key 来源和成本
- 确认 Windows 本地运行方案

### 方向 B：API-level RAG Eval Integration

**目标**：
- 通过 Basjoo 真实 chat / stream API 跑 eval cases
- 记录 API 响应、citation、fallback
- 输出真实 API-level eval report

**前置条件**：
- 需要 LLM API Key
- 需要 Qdrant 运行
- 需要 Embedding API Key

### 方向 C：Eval Report Hardening

**目标**：
- 不扩展系统功能
- 强化报告结构、失败案例解释、baseline comparison
- 保持离线可复现

**前置条件**：
- 无外部依赖
- 可以立即开始

### 方向 D：不继续 Phase 2，冻结项目

**目标**：
- 停止继续开发
- 把项目作为 Phase 1 完成产物
- 回到 CodePilot 或 Portfolio 主线

---

## 7. Recommended Next Engineering Step

**v1.4 已完成**（2026-06-20）：Phase 2 Readiness Audit

**审查结论**：CONDITIONAL GO

**关键发现**：
- Docker 未安装，无法用 docker compose 启动 Qdrant
- WSL 状态不明
- Qdrant Cloud 免费层是最简替代方案
- Embedding API Key（Jina/SiliconFlow）有免费额度
- 代码改动量小：只需扩展 2 个脚本
- 1 周时间上限

**下一步**：
- 如果 Qdrant + Embedding API Key 条件满足 → v2.0-real-qdrant-eval-adapter
- 如果条件不满足 → 冻结项目，回到 CodePilot 主线

详见 `PHASE2_READINESS_AUDIT.md`。

---

## 8. Hard Stop Rules

后续开发禁止事项：

1. **不再做求职包装**，直到新阶段闭环
2. **不接真实 API Key**，除非明确进入 Phase 2 且有安全方案
3. **不提交 .env**
4. **不改核心架构**（不修改 backend/services/、backend/models.py、backend/api/ 等核心文件）
5. **不重做前端 UI**
6. **不把 mock 指标说成真实线上指标**
7. **不为了包装而开发**
8. **不为了跑通而污染原项目**
9. **不提交 selected/ 或 candidates/ 到外层仓库**
10. **不使用 git add . 或 git add ***

---

*Document created: 2026-06-20*
*Purpose: Engineering boundary reset after Phase 1 completion*
