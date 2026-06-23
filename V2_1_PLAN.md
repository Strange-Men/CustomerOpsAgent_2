# CustomerOpsAgent_2｜v2.1 Real Eval Case Expansion Plan

> **Created**: 2026-06-23
> **Purpose**: 规划 v2.1 阶段，扩展 real retrieval eval cases
> **Status**: Completed & Frozen (v2.1.4-quality-freeze)
> **Frozen Baseline**: v2.0.1-quality-freeze
> **Next**: v2.2.0 Cross-border E-commerce Scenarioization（见 [FINAL_PRODUCT_ROADMAP.md](./FINAL_PRODUCT_ROADMAP.md)）

---

## 1. Background

v2.0 已通过质量审计并冻结（tag: v2.0.1-quality-freeze）。当前 real retrieval eval 仅运行 5 个 selected cases，而 mock eval 有完整的 15 个 cases。v2.1 的目标是扩展 real eval case 覆盖范围，从 5 个扩展到 8-10 个，并补充 bad case / mismatch 分析能力。

### v2.0 冻结状态确认

- **外层仓库**：main 分支，clean，最新 commit 75a2d40
- **basjoo 仓库**：phase1-rag-eval-harness 分支，clean，tag v2.0.1-quality-freeze
- **pytest**：44 passed（37 原有 + 7 新增）
- **real eval**：5 cases，100% passed
- **mock eval**：15 cases，100% passed

---

## 2. Why v2.1

### 为什么不继续做前端 / chat eval

1. **检索评估是基础**：在扩展到 LLM 回答生成评估之前，需要先确保检索层面的覆盖足够全面
2. **当前 real eval 覆盖不足**：5 个 cases 只覆盖了 3 种 scenario 类型（normal_hit、no_answer_fallback、evidence_citation），缺少 multi_doc_retrieval、low_relevance_reject、hallucination_risk 的 real eval 验证
3. **小步迭代原则**：v2.0 已冻结，后续开发必须走小步迭代，不能直接跳到大版本
4. **成本可控**：扩展 3-5 个 cases 的 API 成本极低（SiliconFlow 免费额度足够）

### v2.1 的价值

- 验证 real embedding 在更多场景下的表现
- 发现 mock eval 无法覆盖的 edge cases
- 为后续 v3.0（LLM chat eval）打下更坚实的基础
- 补充 bad case / mismatch 分析能力，提升评估报告质量

---

## 3. Current v2.0 Baseline

### 评估指标

| 维度 | Mock Eval | Real Eval |
|---|---|---|
| Cases | 15 | 5 |
| Passed | 15 (100%) | 5 (100%) |
| Precision@3 | 0.567 | 0.800 |
| Recall@3 | 0.978 | 1.000 |
| MRR | 0.600 | 0.800 |
| No-Answer Accuracy | 100.0% | 100.0% |

### 现有 Real Eval Cases（5 个）

| ID | Language | Scenario | Expected Sources | Status |
|---|---|---|---|---|
| TC001 | EN | normal_hit | return_policy.md | ✅ PASS |
| TC002 | ZH | normal_hit | product_faq.md | ✅ PASS |
| TC006 | EN | no_answer_fallback | (none) | ✅ PASS |
| TC010 | EN | evidence_citation | return_policy.md | ✅ PASS |
| TC014 | ZH | normal_hit | troubleshooting.md | ✅ PASS |

### 现有 Mock Eval Cases（15 个）

| ID | Language | Scenario | Expected Sources | Real Eval? |
|---|---|---|---|---|
| TC001 | EN | normal_hit | return_policy.md | ✅ 已有 |
| TC002 | ZH | normal_hit | product_faq.md | ✅ 已有 |
| TC003 | ZH | normal_hit | troubleshooting.md | ⬜ 候选 |
| TC004 | EN | multi_doc_retrieval | product_faq.md, return_policy.md | ⬜ 候选 |
| TC005 | ZH | multi_doc_retrieval | product_faq.md, return_policy.md, troubleshooting.md | ⬜ 候选 |
| TC006 | EN | no_answer_fallback | (none) | ✅ 已有 |
| TC007 | ZH | no_answer_fallback | (none) | ⬜ 候选 |
| TC008 | EN | low_relevance_reject | (none) | ⬜ 候选 |
| TC009 | ZH | low_relevance_reject | (none) | ⬜ 候选 |
| TC010 | EN | evidence_citation | return_policy.md | ✅ 已有 |
| TC011 | ZH | evidence_citation | product_faq.md | ⬜ 候选 |
| TC012 | EN | hallucination_risk | (none) | ⬜ 候选 |
| TC013 | ZH | hallucination_risk | (none) | ⬜ 候选 |
| TC014 | ZH | normal_hit | troubleshooting.md | ✅ 已有 |
| TC015 | EN | normal_hit | product_faq.md | ⬜ 候选 |

### 知识库

- 3 个文档：return_policy.md、product_faq.md、troubleshooting.md
- 11 个 chunks（demo_knowledge_base.json）
- Qdrant collection：customerops_demo_real_eval
- Embedding model：Qwen/Qwen3-Embedding-0.6B (1024 dim)

---

## 4. Proposed v2.1 Scope

### 目标

**Real retrieval eval cases: 5 → 8-10**

### 候选 Cases 分析

#### 优先级 1：补充缺失的 scenario 类型

| ID | Scenario | Language | 理由 |
|---|---|---|---|
| TC004 | multi_doc_retrieval | EN | 验证跨文档检索能力 |
| TC007 | no_answer_fallback | ZH | 补充中文无答案场景 |
| TC011 | evidence_citation | ZH | 补充中文引用验证 |

#### 优先级 2：补充 bad case 覆盖

| ID | Scenario | Language | 理由 |
|---|---|---|---|
| TC008 | low_relevance_reject | EN | 验证低相关度拒答 |
| TC012 | hallucination_risk | EN | 验证幻觉风险检测 |

#### 优先级 3：补充更多 normal_hit

| ID | Scenario | Language | 理由 |
|---|---|---|---|
| TC003 | normal_hit | ZH | 补充中文正常命中 |
| TC015 | normal_hit | EN | 补充英文正常命中 |

### 建议选择（8-10 个）

**方案 A：8 个 cases（最小扩展）**
- 保留原有 5 个：TC001, TC002, TC006, TC010, TC014
- 新增 3 个：TC004, TC007, TC011
- 覆盖 scenario：normal_hit(3), multi_doc_retrieval(1), no_answer_fallback(2), evidence_citation(2)

**方案 B：10 个 cases（推荐）**
- 保留原有 5 个：TC001, TC002, TC006, TC010, TC014
- 新增 5 个：TC004, TC007, TC008, TC011, TC012
- 覆盖 scenario：normal_hit(3), multi_doc_retrieval(1), no_answer_fallback(2), low_relevance_reject(1), evidence_citation(2), hallucination_risk(1)

**推荐方案 B**：覆盖所有 6 种 scenario 类型，API 成本增加可控（约 5 次额外 API 调用）。

---

## 5. Out of Scope

### v2.1 明确禁止

- ❌ 前端 UI
- ❌ API-level chat eval
- ❌ LLM 回答生成评估
- ❌ 完整客服系统
- ❌ 部署
- ❌ 求职包装
- ❌ 恢复 CAREER_PACKAGE.md
- ❌ 面试话术
- ❌ 修改 run_rag_eval.py 核心逻辑（只改 REAL_EVAL_CASE_IDS 配置）
- ❌ 修改 seed_demo_data.py
- ❌ 新增知识库文档
- ❌ 修改 Qdrant collection 写入逻辑

### 为什么不做 LLM 回答生成评估

1. 检索评估是基础，需要先确保检索层覆盖足够
2. LLM 回答评估需要额外的 API 调用（成本更高）
3. 需要设计新的评估指标（答案质量、幻觉检测等）
4. 这是 v3.0 的范围，不是 v2.1

---

## 6. Step Breakdown

### v2.1.1：扩展 real eval cases（5 → 8-10）

**目标**：将 real eval cases 从 5 个扩展到 8-10 个

**允许修改文件**：
- `backend/scripts/run_rag_eval.py`（只改 REAL_EVAL_CASE_IDS 配置）
- `backend/reports/rag_eval_real_report.json`（自动生成）
- `backend/reports/rag_eval_real_report.md`（自动生成）
- `backend/reports/rag_eval_mock_vs_real.md`（自动生成）

**禁止事项**：
- 不修改 run_rag_eval.py 的核心逻辑
- 不修改 seed_demo_data.py
- 不修改 rag_eval_cases.json
- 不修改 demo_knowledge_base.json
- 不新增知识库文档
- 不修改 Qdrant collection 写入逻辑

**验收命令**：
```powershell
cd selected\basjoo\backend
.\venv\Scripts\python.exe scripts\run_rag_eval.py --real
.\venv\Scripts\python.exe -m pytest tests\rag_eval -v
```

**完成条件**：
- REAL_EVAL_CASE_IDS 包含 8-10 个 case ID
- `run_rag_eval.py --real` 运行成功，所有 cases 通过
- 生成新的 rag_eval_real_report.json/md
- 生成新的 rag_eval_mock_vs_real.md
- pytest 44 个测试全部通过

**是否需要真实 API**：是（SiliconFlow embedding + Qdrant search）

**是否需要 Qdrant**：是（customerops_demo_real_eval collection）

---

### v2.1.2：补充 bad case / mismatch 分析输出

**目标**：在评估报告中增加 bad case / mismatch 分析能力

**允许修改文件**：
- `backend/scripts/run_rag_eval.py`（增加 mismatch 分析逻辑）
- `backend/reports/rag_eval_real_report.md`（增加 mismatch 分析章节）
- `backend/reports/rag_eval_mock_vs_real.md`（增加 mismatch 分析章节）

**禁止事项**：
- 不修改 seed_demo_data.py
- 不修改 rag_eval_cases.json
- 不修改 demo_knowledge_base.json
- 不新增知识库文档

**验收命令**：
```powershell
cd selected\basjoo\backend
.\venv\Scripts\python.exe scripts\run_rag_eval.py --real
# 检查报告中是否包含 mismatch 分析章节
```

**完成条件**：
- 评估报告包含 "Mismatch Analysis" 章节
- 列出 mock 和 real 结果不一致的 cases
- 分析 mismatch 原因（embedding 差异、阈值差异等）

**是否需要真实 API**：是

**是否需要 Qdrant**：是

---

### v2.1.3：更新报告格式和文档

**目标**：更新报告格式，补充 v2.1 相关文档

**允许修改文件**：
- `backend/docs/rag-evaluation.md`（更新 real eval cases 列表）
- `backend/reports/rag_eval_real_report.md`（格式优化）
- `backend/reports/rag_eval_mock_vs_real.md`（格式优化）

**禁止事项**：
- 不修改 run_rag_eval.py
- 不修改 seed_demo_data.py
- 不修改 rag_eval_cases.json
- 不修改 demo_knowledge_base.json

**验收命令**：
```powershell
# 检查文档是否更新
cat backend\docs\rag-evaluation.md | grep "v2.1"
cat backend\reports\rag_eval_real_report.md | grep "Mismatch"
```

**完成条件**：
- rag-evaluation.md 包含 v2.1 更新说明
- real eval cases 列表更新为 8-10 个
- 报告格式优化完成

**是否需要真实 API**：否

**是否需要 Qdrant**：否

---

### v2.1.4：质量审计和 freeze tag

**目标**：进行质量审计，创建 freeze tag

**允许修改文件**：
- 无（只读审查）

**禁止事项**：
- 不修改任何代码文件
- 不修改任何文档文件

**验收命令**：
```powershell
cd selected\basjoo
git status
git log --oneline -5
git tag -l "v2.1*"
```

**完成条件**：
- 所有测试通过
- 所有报告生成正确
- 创建 tag v2.1.x-xxx
- 两个仓库 clean

**是否需要真实 API**：否

**是否需要 Qdrant**：否

---

## 7. Acceptance Criteria

### v2.1.1 验收标准

1. `REAL_EVAL_CASE_IDS` 包含 8-10 个 case ID
2. `run_rag_eval.py --real` 运行成功
3. 所有 cases 通过（passed）
4. 生成 rag_eval_real_report.json 和 rag_eval_real_report.md
5. 生成 rag_eval_mock_vs_real.md
6. pytest 44 个测试全部通过
7. 报告中 case 数量正确（8-10 个）

### v2.1.2 验收标准

1. 评估报告包含 "Mismatch Analysis" 章节
2. 列出 mock 和 real 结果不一致的 cases
3. 分析 mismatch 原因
4. 报告格式清晰、可读

### v2.1.3 验收标准

1. rag-evaluation.md 包含 v2.1 更新说明
2. real eval cases 列表更新为 8-10 个
3. 文档格式一致、无错误

### v2.1.4 验收标准

1. 所有测试通过
2. 所有报告正确
3. tag 创建成功
4. 两个仓库 clean

---

## 8. Risk Control

### API 成本风险

- **风险**：扩展 cases 增加 API 调用次数
- **控制**：SiliconFlow 免费额度足够，每次 eval 只需 8-10 次 embedding 调用
- **缓解**：使用 `--top-k 3` 减少返回结果数量

### Qdrant 数据污染风险

- **风险**：修改 Qdrant collection 可能影响现有数据
- **控制**：v2.1 不修改 Qdrant collection 写入逻辑，只修改 eval case 配置
- **缓解**：使用 `--reset` 参数重建 collection（如果需要）

### 真实 Key 安全风险

- **风险**：API Key 泄露
- **控制**：.env 文件已被 gitignore，不打印 API Key
- **缓解**：代码中使用 `os.environ.get()` 读取，不硬编码

### 报告夸大风险

- **风险**：报告过度美化结果
- **控制**：如实记录所有 cases 结果，包括失败和 mismatch
- **缓解**：增加 mismatch 分析章节，公开讨论局限性

### 知识库覆盖不足风险

- **风险**：当前知识库只有 3 个文档，可能无法覆盖所有测试场景
- **控制**：v2.1 不新增知识库文档，只扩展现有 cases
- **缓解**：在报告中记录知识库覆盖局限性

---

## 9. Next Prompt Recommendation

下一轮建议 prompt 名称：

```
v2.1.1-real-eval-case-expansion
```

**目标**：实现 v2.1.1，将 real eval cases 从 5 个扩展到 8-10 个

**预计工作量**：
- 修改 1 个文件（run_rag_eval.py 的 REAL_EVAL_CASE_IDS 配置）
- 运行 1 次 real eval（需要 API + Qdrant）
- 生成 3 个报告文件

**前置条件**：
- Qdrant 运行中（docker compose up -d qdrant）
- .env 配置正确（SILICONFLOW_API_KEY）
- basjoo 仓库 clean

---

## 10. Timeline

| 阶段 | 目标 | 预计时间 | 状态 |
|---|---|---|---|
| v2.1.1 | 扩展 real eval cases（5 → 8-10） | 1 小时 | ⬜ 待开始 |
| v2.1.2 | 补充 bad case / mismatch 分析 | 1 小时 | ⬜ 待开始 |
| v2.1.3 | 更新报告格式和文档 | 30 分钟 | ⬜ 待开始 |
| v2.1.4 | 质量审计和 freeze tag | 30 分钟 | ⬜ 待开始 |

**总计**：约 3 小时

---

## 11. Decision Log

| 日期 | 决策 | 理由 |
|---|---|---|
| 2026-06-23 | v2.1 只扩展 real eval cases，不做 LLM chat eval | 小步迭代原则，检索评估是基础 |
| 2026-06-23 | 推荐方案 B（10 个 cases） | 覆盖所有 6 种 scenario 类型 |
| 2026-06-23 | 不新增知识库文档 | 控制范围，避免过度扩展 |
| 2026-06-23 | 不修改 seed_demo_data.py | 保持现有写入逻辑不变 |

---

*Document created: 2026-06-23*
*Purpose: Plan v2.1 real eval case expansion*
*Status: Planning*
