# CustomerOpsAgent_2｜Phase 1 计划锁定

> **Created**: 2026-06-19
> **Author**: Claude Code
> **Status**: Phase 1 Plan Locked — v0.3-phase1-plan
> **Version**: v0.3-phase1-plan

---

## 1. Background

### 1.1 项目背景

CustomerOpsAgent_2 是一个基于开源 AI 客服平台的二次开发项目。经过 Phase 0 的选型审查和准备，已确定基于 [Basjoo](https://github.com/haoyiyin/basjoo) 进行二开。

**Basjoo 核心能力**：
- ✅ 完整的 RAG 管道：文档上传 → 解析 → 分块 → Embedding → Qdrant 存储 → 检索
- ✅ 支持多种 Embedding 提供商：Jina, SiliconFlow, Custom OpenAI Compatible
- ✅ 完善的测试基础设施：35+ 测试文件，MockLLMService，隔离的 SQLite 测试数据库
- ✅ 多租户隔离：per-tenant Qdrant collection + payload filter
- ✅ MIT License，技术栈主流，规模适中

**Phase 0 已完成**：
- ✅ Fork 已创建：Strange-Men/basjoo
- ✅ 已 clone 到 selected/basjoo
- ✅ origin → Strange-Men/basjoo, upstream → haoyiyin/basjoo
- ✅ v0.1-fork-baseline tag 已创建并推送
- ✅ v0.2-setup-verified tag 已创建并推送
- ✅ 后端测试：267 passed, 36 failed (Qdrant 依赖), 1 skipped
- ✅ 前端测试：125 passed (20 test files)

### 1.2 当前问题

1. **无 RAG 评估框架**：Basjoo 有完整的 RAG 管道，但没有任何系统化的 RAG 质量评估机制。无法量化回答质量、检索精度、幻觉率。
2. **无 Demo 数据**：新用户启动后是空白页面，需要手动配置 agent、上传文档才能体验。
3. **无评估报告**：无法生成可展示的 RAG 质量报告。

---

## 2. Phase 1 Goal

### 2.1 任务名称

**RAG Evaluation Harness + Demo Data**

### 2.2 核心目标

1. **构建 RAG 评估框架**：一套可复用的 RAG 质量评估测试套件
2. **创建 Demo 数据**：预配置的 agent + 知识库 + 问答对，一键启动体验
3. **生成评估报告**：自动生成 JSON + Markdown 格式的 RAG 质量报告

### 2.3 为什么选这个任务

| 理由 | 说明 |
|---|---|
| **填补核心空白** | Basjoo 有 RAG 管道但无评估框架 |
| **不破坏原项目** | 纯新增 tests/rag_eval/ 和 scripts/，不修改核心代码 |
| **不依赖真实 API Key** | 可用 MockLLMService 跑通框架 |
| **短期可完成** | 1-2 周可出 MVP |
| **体现核心能力** | RAG eval 是 AI Agent 工程化的核心技能 |
| **作品集亮点** | "Built RAG evaluation harness for open-source customer support platform" |

---

## 3. Current Basjoo Capability Summary

### 3.1 RAG 相关模块

| 模块 | 文件 | 行数 | 功能 |
|---|---|---|---|
| **RAG 检索** | `backend/services/kb_retrieval_service.py` | 134 | 向量相似度搜索 + threshold 过滤 + top_k 截断 |
| **知识库管理** | `backend/services/kb_service.py` | 442 | 知识库 CRUD，多租户隔离 |
| **Qdrant 操作** | `backend/services/qdrant_service.py` | 155 | Qdrant 向量操作，batch upsert |
| **文档处理** | `backend/services/kb_document_processor.py` | 258 | 文档解析 → 分块 → Embedding → Qdrant |
| **文档解析** | `backend/services/document_parser.py` | 232 | PDF, DOCX, XLSX, TXT, MD, HTML 解析 |
| **LLM 抽象** | `backend/services/llm_service.py` | 889 | OpenAI, DeepSeek, Anthropic, Gemini, Mock |

### 3.2 测试基础设施

| 组件 | 文件 | 功能 |
|---|---|---|
| **测试配置** | `backend/tests/conftest.py` | 隔离 SQLite 数据库，MockLLMService，fixture |
| **客户端 fixture** | `conftest.py` | `client` (认证), `public_client` (非认证), `support_client`, `readonly_client` |
| **Mock LLM** | `conftest.py` | `MockLLMService` monkeypatch，不需要真实 API Key |
| **测试数据库** | `backend/.pytest_dbs/` | 每个测试隔离的 SQLite 数据库 |

### 3.3 现有 RAG 测试

| 测试文件 | 覆盖范围 |
|---|---|
| `test_chat_kb_retrieval.py` | RAG 检索集成测试 |
| `test_kb_retrieval.py` | 知识库检索单元测试 |
| `test_kb_data_layer.py` | 知识库数据层测试 |
| `test_kb_document_pipeline.py` | 文档处理管道测试 |
| `test_file_upload_kb_ingestion.py` | 文件上传 + KB 集成测试 |

**关键发现**：
- 现有测试验证"功能是否正常工作"，但不评估"RAG 质量如何"
- 无 precision@k, recall@k, MRR 等量化指标
- 无 faithfulness, hallucination rate 评估
- 无 no-answer fallback 场景测试
- 无自动化评估报告生成

---

## 4. Phase 1 Current Scope

### 4.1 RAG Evaluation Harness

**目标**：构建一套可复用的 RAG 质量评估测试框架

**计划新增/扩展**：

| 组件 | 文件 | 功能 |
|---|---|---|
| **RAG eval 配置** | `tests/rag_eval/conftest.py` | RAG eval 测试专用配置和 fixture |
| **检索精度测试** | `tests/rag_eval/test_retrieval_precision.py` | precision@k, recall@k, MRR |
| **无答案 fallback 测试** | `tests/rag_eval/test_no_answer_fallback.py` | 低相关性时是否正确返回空 |
| **Evidence/Citation 测试** | `tests/rag_eval/test_evidence_citation.py` | 引用来源是否正确 |
| **幻觉风险测试** | `tests/rag_eval/test_hallucination_risk.py` | answer 是否包含未检索到的信息 |
| **评估用例数据** | `tests/rag_eval/rag_eval_cases.json` | 测试用例：问题 + 期望答案 + 期望来源 |
| **Demo 知识库数据** | `tests/rag_eval/demo_knowledge_base.json` | 测试用知识库文档 |

**评测入口**：
- **pytest**：主要入口，`pytest tests/rag_eval/ -v`
- **独立 script**：`backend/scripts/run_rag_eval.py`，可单独运行并生成报告

**每个测试用例输入**：
```json
{
  "query": "如何退货？",
  "expected_answer_keywords": ["退货", "退款", "7天"],
  "expected_sources": ["return_policy.md"],
  "expected_confidence": "high"
}
```

**每个测试用例期望输出**：
- 检索到的 chunks 是否包含 expected_sources
- answer 是否包含 expected_answer_keywords
- confidence score 是否符合预期
- 是否有 hallucination（answer 包含未检索到的事实性声明）

**是否需要调用真实 LLM**：
- **Mock mode**：不需要，用 MockLLMService 跑通框架
- **真实 mode**：可选，用于验证真实 RAG 质量

**是否需要真实 Embedding API Key**：
- **Mock mode**：不需要，用 mock embedding
- **真实 mode**：需要 Jina 或 SiliconFlow API Key

**没有 Qdrant 时如何处理**：
- 使用 `pytest.mark.skipif` 标记依赖 Qdrant 的测试
- 提供 mock Qdrant client 用于单元测试
- 提供 fallback 策略：跳过检索测试，只测试 answer 质量评估逻辑

### 4.2 Demo Data

**目标**：创建预配置的 demo 数据，一键启动体验

**计划新增**：

| 组件 | 文件 | 功能 |
|---|---|---|
| **Demo Agent 配置** | `scripts/demo_data/agents.json` | 3 个预配置 agent（不同场景） |
| **Demo 知识库文档** | `scripts/demo_data/knowledge/` | 产品 FAQ、退换货政策、故障排除 |
| **Demo 问答对** | `scripts/demo_data/demo_questions.json` | 预设问题 + 期望答案 |
| **Demo 对话示例** | `scripts/demo_data/conversations.json` | 预设对话流程 |
| **Demo 期望 Evidence** | `scripts/demo_data/expected_evidence.json` | 每个问题的期望引用来源 |
| **Demo Bad Cases** | `scripts/demo_data/bad_cases.json` | 幻觉、低相关性、无答案场景 |
| **Seed Script** | `scripts/seed_demo_data.py` | 一键填充 demo 数据到数据库 |

**Demo data 放在哪里**：
- `scripts/demo_data/` 目录下，JSON/YAML 格式
- 不直接修改数据库，通过 seed script 导入

**Seed script 是否修改数据库**：
- 是，但只在开发/演示环境
- 使用 `--dry-run` 参数可预览而不实际写入
- 使用 `--reset` 参数可清除 demo 数据

**是否需要真实 API Key**：
- **创建 demo agent**：不需要
- **创建 demo knowledge base**：需要 Embedding API Key（或用 mock）
- **运行 demo 对话**：需要 LLM API Key（或用 mock）

**如何保证 demo data 不污染生产配置**：
- 使用独立的 demo workspace/agent ID
- Seed script 检查环境变量，生产环境拒绝运行
- Demo 数据有明显标记（如 agent name 包含 "[Demo]"）

### 4.3 Eval Report

**目标**：自动生成 RAG 质量评估报告

**计划生成**：

| 报告格式 | 文件 | 内容 |
|---|---|---|
| **JSON Report** | `backend/reports/rag_eval_report.json` | 机器可读的评估结果 |
| **Markdown Report** | `backend/reports/rag_eval_report.md` | 人类可读的评估报告 |

**报告内容**：
- 测试用例通过/失败统计
- 检索精度指标：precision@k, recall@k, MRR
- 回答质量指标：faithfulness score, relevance score
- 幻觉风险指标：hallucination rate
- No-answer fallback 准确率
- 每个测试用例的详细结果
- 失败用例的分析和建议

**报告输出路径**：
- `backend/reports/rag_eval_report.json`
- `backend/reports/rag_eval_report.md`

**适合放进作品集展示**：
- ✅ Markdown 报告可直接展示
- ✅ 包含量化指标，体现工程化能力
- ✅ 包含 bad case 分析，体现问题发现能力

---

## 5. Non-goals (Explicit)

### 5.1 不做什么

| 不做 | 原因 |
|---|---|
| **不重写 Basjoo 核心架构** | 保持原项目完整性 |
| **不重做前端 UI** | Phase 1 不涉及前端 |
| **不接真实客服系统** | 专注 RAG eval，不接外部系统 |
| **不接真实生产 API Key** | 用 mock 或开发 key |
| **不引入 LangGraph / RAGAS / DeepEval / Mem0** | 避免重型依赖 |
| **不改变现有数据库主结构** | 除非必须且经过说明 |
| **不破坏原有测试** | 新增测试不影响已有测试 |
| **不删除原有功能** | 保持原项目功能完整 |
| **不把 CustomerOpsAgent_2 自研代码搬进 Basjoo** | 保持 Basjoo 独立性 |
| **不从 0 重写客服 Agent** | 基于 Basjoo 二开 |

### 5.2 不修改的文件

| 文件/目录 | 原因 |
|---|---|
| `backend/services/llm_service.py` | LLM 抽象层，不修改 |
| `backend/services/kb_retrieval_service.py` | RAG 检索核心，不修改 |
| `backend/services/kb_document_processor.py` | 文档处理核心，不修改 |
| `backend/models.py` | 数据模型，不修改 |
| `backend/main.py` | 应用入口，不修改 |
| `frontend-nextjs/` | 前端代码，Phase 1 不涉及 |
| `widget/` | Widget 代码，Phase 1 不涉及 |

---

## 6. Technical Strategy

### 6.1 核心原则

1. **优先复用 Basjoo 现有基础设施**
   - 复用 `MockLLMService` 和 `conftest.py` fixture
   - 复用现有测试框架和数据库隔离机制
   - 复用现有的 RAG 检索服务

2. **优先用 pytest 做 harness**
   - pytest 作为主要测试入口
   - 利用 pytest fixtures 管理测试数据
   - 利用 pytest markers 分类测试（mock/real, unit/integration）

3. **尽量不新增依赖**
   - 不引入 RAGAS、DeepEval 等重型评估框架
   - 用 Python 标准库 + pytest 实现评估逻辑
   - 如需 JSON/YAML 解析，用已有的 pydantic 或标准库

4. **数据分离**
   - 测试用例放在 JSON 文件中，不硬编码在测试逻辑里
   - Demo 数据放在独立目录，通过 seed script 导入
   - 评估报告输出到独立目录

5. **Mock / No-API-Key 策略**
   - 没有 API Key 时，测试使用 mock 或标记跳过
   - 没有 Qdrant 时，测试标记跳过或使用 mock client
   - 每个测试可单独运行，不依赖外部服务

### 6.2 技术选型

| 组件 | 选型 | 原因 |
|---|---|---|
| **测试框架** | pytest | 已有，复用 |
| **数据格式** | JSON | 标准库支持，易于维护 |
| **报告格式** | Markdown + JSON | 人类可读 + 机器可读 |
| **Mock 策略** | MockLLMService + pytest monkeypatch | 已有，复用 |
| **评估逻辑** | Python 自定义实现 | 避免重型依赖 |

---

## 7. Planned File Structure

### 7.1 基于 Basjoo 真实目录结构

```
backend/
├── tests/
│   └── rag_eval/                          # 新增：RAG 评估测试目录
│       ├── __init__.py                    # Python 包标识
│       ├── conftest.py                    # RAG eval 测试配置和 fixture
│       ├── test_retrieval_precision.py    # 检索精度测试
│       ├── test_no_answer_fallback.py     # 无答案 fallback 测试
│       ├── test_evidence_citation.py      # Evidence/Citation 测试
│       ├── test_hallucination_risk.py     # 幻觉风险测试
│       └── fixtures/                      # 测试数据目录
│           ├── rag_eval_cases.json        # 评估用例：问题 + 期望
│           └── demo_knowledge_base.json   # 测试用知识库文档
│
├── scripts/                               # 已有目录，新增文件
│   ├── seed_demo_data.py                  # 新增：Demo 数据种子脚本
│   ├── run_rag_eval.py                    # 新增：RAG 评估运行脚本
│   └── demo_data/                         # 新增：Demo 数据目录
│       ├── agents.json                    # 预配置 agent
│       ├── knowledge/                     # 示例知识库文档
│       │   ├── product_faq.md             # 产品 FAQ
│       │   ├── return_policy.md           # 退换货政策
│       │   └── troubleshooting.md         # 故障排除
│       ├── demo_questions.json            # 预设问题 + 期望答案
│       ├── conversations.json             # 预设对话示例
│       ├── expected_evidence.json         # 每个问题的期望引用来源
│       └── bad_cases.json                 # 幻觉、低相关性、无答案场景
│
├── reports/                               # 新增：评估报告目录
│   ├── rag_eval_report.json               # JSON 格式评估报告
│   └── rag_eval_report.md                 # Markdown 格式评估报告
│
└── docs/                                  # 新增：文档目录
    └── rag-evaluation.md                  # RAG 评估使用说明
```

### 7.2 为什么不修改已有目录结构

- `backend/tests/` 已有 30+ 测试文件，新增 `rag_eval/` 子目录不影响已有测试
- `backend/scripts/` 已存在，新增文件不影响已有脚本
- `backend/reports/` 和 `backend/docs/` 是新增目录，不影响已有代码

---

## 8. Eval Case Design

### 8.1 测试用例覆盖场景

| 场景类型 | 用例数 | 说明 |
|---|---|---|
| **正常命中知识库** | 3 | 问题与知识库高度相关，应返回正确答案和来源 |
| **多文档检索** | 2 | 问题需要综合多个文档才能回答 |
| **无答案 fallback** | 2 | 问题与知识库无关，应返回空或"我不确定" |
| **低相关度拒答** | 2 | 问题部分相关但不足以回答，应谨慎回答或拒答 |
| **Evidence/Citation 校验** | 2 | 验证返回的引用来源是否正确 |
| **幻觉风险样例** | 2 | 验证 answer 不包含未检索到的事实性声明 |
| **中文客服问题** | 2 | 中文场景测试 |
| **英文客服问题** | 2 | 英文场景测试 |

**总计**：17 个测试用例（至少 8-10 个必须通过）

### 8.2 测试用例格式

```json
{
  "test_id": "TC001",
  "scenario": "normal_hit",
  "query": "如何退货？",
  "language": "zh",
  "expected_answer_keywords": ["退货", "退款", "7天", "无理由"],
  "expected_sources": ["return_policy.md"],
  "expected_confidence": "high",
  "description": "正常退货政策查询，应返回退货流程和条件"
}
```

### 8.3 评估指标

| 指标 | 计算方式 | 说明 |
|---|---|---|
| **precision@k** | 检索到的 top-k 结果中相关文档的比例 | 检索精度 |
| **recall@k** | 相关文档中被检索到的比例 | 检索召回率 |
| **MRR** | 第一个相关结果的排名倒数的平均值 | 排名质量 |
| **faithfulness** | answer 中基于 retrieved context 的比例 | 回答忠实度 |
| **hallucination_rate** | answer 中包含未检索到信息的比例 | 幻觉率 |
| **no_answer_accuracy** | 无答案场景正确拒答的比例 | 拒答准确率 |

---

## 9. Demo Data Design

### 9.1 Demo Agent 配置

```json
{
  "agents": [
    {
      "name": "[Demo] 电商客服",
      "description": "演示电商客服场景",
      "agent_type": "website_support",
      "provider_type": "deepseek",
      "system_prompt": "你是一个专业的电商客服助手..."
    },
    {
      "name": "[Demo] 技术支持",
      "description": "演示技术支持场景",
      "agent_type": "website_support",
      "provider_type": "deepseek",
      "system_prompt": "你是一个技术支持助手..."
    },
    {
      "name": "[Demo] 产品咨询",
      "description": "演示产品咨询场景",
      "agent_type": "website_support",
      "provider_type": "deepseek",
      "system_prompt": "你是一个产品咨询助手..."
    }
  ]
}
```

### 9.2 Demo 知识库文档

| 文档 | 内容 | 用途 |
|---|---|---|
| `product_faq.md` | 产品常见问题 | 测试 FAQ 场景 |
| `return_policy.md` | 退换货政策 | 测试政策查询 |
| `troubleshooting.md` | 故障排除指南 | 测试技术支持场景 |

### 9.3 Demo 问答对

```json
{
  "questions": [
    {
      "id": "Q001",
      "question": "如何退货？",
      "expected_answer": "您可以在购买后7天内无理由退货...",
      "expected_sources": ["return_policy.md"],
      "category": "退货退款"
    },
    {
      "id": "Q002",
      "question": "产品保修多久？",
      "expected_answer": "我们的产品提供1年保修...",
      "expected_sources": ["product_faq.md"],
      "category": "产品信息"
    }
  ]
}
```

### 9.4 Demo Bad Cases

```json
{
  "bad_cases": [
    {
      "id": "BC001",
      "type": "hallucination",
      "query": "你们的公司地址在哪里？",
      "description": "知识库中没有公司地址信息，测试是否会产生幻觉",
      "expected_behavior": "应返回'我不确定'或拒答"
    },
    {
      "id": "BC002",
      "type": "low_relevance",
      "query": "今天天气怎么样？",
      "description": "问题与客服无关，测试低相关度拒答",
      "expected_behavior": "应返回'这个问题超出我的能力范围'"
    }
  ]
}
```

---

## 10. Report Design

### 10.1 JSON Report 结构

```json
{
  "metadata": {
    "timestamp": "2026-06-19T12:00:00Z",
    "basjoo_version": "6939926",
    "test_mode": "mock",
    "total_cases": 17,
    "passed_cases": 15,
    "failed_cases": 2
  },
  "metrics": {
    "precision_at_5": 0.85,
    "recall_at_5": 0.78,
    "mrr": 0.92,
    "faithfulness_score": 0.95,
    "hallucination_rate": 0.05,
    "no_answer_accuracy": 0.90
  },
  "test_results": [
    {
      "test_id": "TC001",
      "scenario": "normal_hit",
      "query": "如何退货？",
      "passed": true,
      "precision": 1.0,
      "recall": 1.0,
      "faithfulness": 1.0,
      "hallucination": false
    }
  ],
  "failed_cases": [
    {
      "test_id": "TC015",
      "reason": "answer missing expected keyword",
      "details": "..."
    }
  ]
}
```

### 10.2 Markdown Report 结构

```markdown
# RAG Evaluation Report

**Generated**: 2026-06-19 12:00:00
**Basjoo Version**: 6939926
**Test Mode**: Mock

## Summary

| Metric | Value |
|---|---|
| Total Cases | 17 |
| Passed | 15 (88%) |
| Failed | 2 (12%) |

## Metrics

| Metric | Value |
|---|---|
| Precision@5 | 0.85 |
| Recall@5 | 0.78 |
| MRR | 0.92 |
| Faithfulness | 0.95 |
| Hallucination Rate | 5% |
| No-Answer Accuracy | 90% |

## Test Results

### Passed Cases
- TC001: 如何退货？ ✅
- TC002: 产品保修多久？ ✅
...

### Failed Cases
- TC015: ❌ answer missing expected keyword
...

## Analysis

### Strengths
- 高 faithfulness score (0.95)
- 低 hallucination rate (5%)

### Weaknesses
- Recall@5 较低 (0.78)
- 部分 no-answer 场景未正确拒答

## Recommendations
1. 优化检索策略，提高 recall
2. 增强 no-answer fallback 策略
...
```

---

## 11. API / Service Touch Points

### 11.1 需要测试的 API

| API | 端点 | 功能 | 测试方式 |
|---|---|---|---|
| **Chat Stream** | `/api/v1/chat/stream` | 流式聊天 | Mock LLM，验证 answer 质量 |
| **KB Search** | 内部服务调用 | 知识库检索 | Mock/Real Qdrant，验证检索精度 |
| **Agent Config** | `/api/v1/agent:default` | 获取 agent 配置 | 使用测试 fixture |
| **KB Status** | 内部服务调用 | 知识库状态 | 使用测试 fixture |

### 11.2 需要测试的服务

| 服务 | 文件 | 测试方式 |
|---|---|---|
| **kb_retrieval_service** | `backend/services/kb_retrieval_service.py` | Mock/Real Qdrant |
| **kb_service** | `backend/services/kb_service.py` | 使用测试数据库 |
| **qdrant_service** | `backend/services/qdrant_service.py` | Mock Qdrant client |
| **llm_service** | `backend/services/llm_service.py` | MockLLMService |

### 11.3 不修改的文件

| 文件 | 原因 |
|---|---|
| `backend/services/llm_service.py` | 只使用，不修改 |
| `backend/services/kb_retrieval_service.py` | 只使用，不修改 |
| `backend/services/kb_document_processor.py` | 只使用，不修改 |
| `backend/models.py` | 只使用，不修改 |

---

## 12. Mock / No-API-Key Strategy

### 12.1 Mock 策略总览

| 组件 | Mock 方式 | 说明 |
|---|---|---|
| **LLM** | MockLLMService | 已有，直接复用 |
| **Embedding** | Mock Embedding | 返回固定向量，不调用真实 API |
| **Qdrant** | Mock Qdrant Client | 内存存储，不依赖真实 Qdrant |
| **数据库** | SQLite 测试数据库 | 已有，复用 conftest.py |

### 12.2 无 API Key 时的行为

| 场景 | 行为 |
|---|---|
| **无 LLM API Key** | 使用 MockLLMService，返回预设回答 |
| **无 Embedding API Key** | 使用 mock embedding，返回固定向量 |
| **无 Qdrant** | 跳过依赖 Qdrant 的测试，或使用 mock client |
| **无 Redis** | 测试自动 fallback 到内存 |
| **无 PostgreSQL** | 测试使用 SQLite |

### 12.3 测试标记

```python
# 依赖真实 API Key 的测试
@pytest.mark.requires_api_key
def test_real_llm_response():
    ...

# 依赖 Qdrant 的测试
@pytest.mark.requires_qdrant
def test_real_qdrant_search():
    ...

# Mock 模式测试（默认）
def test_mock_retrieval():
    ...
```

### 12.4 环境变量检查

```python
import os

def check_api_key_available():
    """检查是否有可用的 API Key"""
    return bool(os.getenv("DEEPSEEK_API_KEY") or os.getenv("OPENAI_API_KEY"))

def check_qdrant_available():
    """检查 Qdrant 是否可用"""
    import httpx
    try:
        resp = httpx.get("http://localhost:6333/healthz", timeout=2)
        return resp.status_code == 200
    except:
        return False
```

---

## 13. Qdrant / External Service Strategy

### 13.1 Qdrant 不可用时的处理

| 场景 | 处理方式 |
|---|---|
| **单元测试** | 使用 mock Qdrant client，不依赖真实服务 |
| **集成测试** | 使用 `pytest.mark.skipif` 标记跳过 |
| **评估报告** | 在报告中标注 "Qdrant not available, using mock" |

### 13.2 Mock Qdrant Client 实现

```python
class MockQdrantClient:
    """Mock Qdrant client for testing"""

    def __init__(self):
        self.collections = {}
        self.points = {}

    def search(self, collection_name, query_vector, limit=10):
        """返回预设的搜索结果"""
        return self.points.get(collection_name, [])[:limit]

    def upsert(self, collection_name, points):
        """存储到内存"""
        if collection_name not in self.points:
            self.points[collection_name] = []
        self.points[collection_name].extend(points)
```

### 13.3 外部服务依赖总结

| 服务 | 是否必需 | Mock 策略 |
|---|---|---|
| **Qdrant** | RAG 功能必需 | Mock client 或 skip |
| **Redis** | 可选 | 自动 fallback 到内存 |
| **PostgreSQL** | 可选 | 测试使用 SQLite |
| **LLM API** | 可选 | MockLLMService |
| **Embedding API** | 可选 | Mock embedding |
| **Scrapling** | URL 功能必需 | 不测试 URL 爬取 |

---

## 14. Acceptance Criteria

### 14.1 Phase 1 完成后必须满足

| 序号 | 验收标准 | 验证方式 |
|---|---|---|
| 1 | 不需要真实 API Key 也能运行基础 RAG eval harness | `pytest tests/rag_eval/ -v` 通过 |
| 2 | 至少包含 8-10 个 eval cases | 检查 `rag_eval_cases.json` |
| 3 | 覆盖：正常命中、多文档检索、无答案 fallback、低相关度拒答、evidence/citation 校验、幻觉风险、中文、英文 | 检查测试用例覆盖 |
| 4 | pytest 能运行新增 harness | `pytest tests/rag_eval/ -v` 输出正常 |
| 5 | 原有核心测试不被破坏 | `pytest tests/ --ignore=tests/rag_eval/ -v` 全部通过 |
| 6 | 能生成 eval report | `backend/reports/rag_eval_report.json` 和 `.md` 存在 |
| 7 | demo data 能被 seed 或作为 fixtures 使用 | `python scripts/seed_demo_data.py --dry-run` 成功 |
| 8 | 文档说明如何运行 | `backend/docs/rag-evaluation.md` 存在且内容完整 |
| 9 | 不提交 .env / API Key / 缓存 / node_modules | `git status` 检查 |
| 10 | selected/basjoo 工作区 clean | `git status` 检查 |
| 11 | v1.0 tag 只有在实现和验收通过后才能创建 | 所有验收标准通过后才打 tag |

### 14.2 验收命令

```bash
# 1. 运行 RAG eval harness (mock mode)
cd selected/basjoo/backend
pytest tests/rag_eval/ -v

# 2. 运行原有测试，确保不被破坏
pytest tests/ --ignore=tests/rag_eval/ -v

# 3. 生成评估报告
python scripts/run_rag_eval.py

# 4. 运行 demo seed (dry-run)
python scripts/seed_demo_data.py --dry-run

# 5. 检查报告
cat reports/rag_eval_report.md

# 6. 检查文档
cat docs/rag-evaluation.md
```

---

## 15. Risks

### 15.1 风险矩阵

| 风险 | 级别 | 影响 | 缓解措施 |
|---|---|---|---|
| **Mock mode 无法验证真实 RAG 质量** | 低 | 框架先跑通，后续用真实 API 验证 | 提供 mock + real 双模式 |
| **golden_qa_pairs 数据量不足** | 低 | 先做 10-20 对，后续扩展 | 用 JSON 文件管理，易于扩展 |
| **与现有测试框架集成** | 低 | 复用 conftest.py fixture | 已验证兼容性 |
| **Qdrant 不可用** | 中 | 部分测试无法运行 | 提供 mock client 和 skip 标记 |
| **API Key 不可用** | 低 | 无法测试真实 LLM | MockLLMService 可跑通框架 |
| **Windows 兼容性** | 低 | 路径分隔符问题 | 已验证 venv 正常工作 |

### 15.2 风险缓解详情

**Mock mode 无法验证真实 RAG 质量**：
- 框架先跑通，确保代码正确
- 后续用真实 API 验证，确保 RAG 质量
- 提供 `--mode=real` 参数切换真实模式

**Qdrant 不可用**：
- 提供 MockQdrantClient 用于单元测试
- 使用 `pytest.mark.skipif` 标记依赖 Qdrant 的测试
- 在评估报告中标注 "Qdrant not available"

**API Key 不可用**：
- MockLLMService 返回预设回答
- Mock embedding 返回固定向量
- 测试不依赖真实 API

---

## 16. v1.0 Implementation Rules

### 16.1 允许修改的文件

| 文件/目录 | 操作 | 说明 |
|---|---|---|
| `backend/tests/rag_eval/*` | 新增 | RAG 评估测试 |
| `backend/scripts/*` | 新增 | Seed script 和评估脚本 |
| `backend/reports/*` | 新增 | 评估报告（gitignore） |
| `backend/docs/*` | 新增 | 文档 |
| `backend/.gitignore` | 修改 | 添加 reports/ 到 gitignore |

### 16.2 不允许修改的文件

| 文件/目录 | 原因 |
|---|---|
| `backend/services/llm_service.py` | 核心服务，不修改 |
| `backend/services/kb_retrieval_service.py` | 核心服务，不修改 |
| `backend/services/kb_document_processor.py` | 核心服务，不修改 |
| `backend/services/kb_service.py` | 核心服务，不修改 |
| `backend/services/qdrant_service.py` | 核心服务，不修改 |
| `backend/models.py` | 数据模型，不修改 |
| `backend/main.py` | 应用入口，不修改 |
| `backend/api/*` | API 路由，不修改 |
| `frontend-nextjs/*` | 前端代码，Phase 1 不涉及 |
| `widget/*` | Widget 代码，Phase 1 不涉及 |
| `docker-compose.yml` | 部署配置，不修改 |
| `.env.example` | 环境配置示例，不修改 |

### 16.3 Commit Message 规范

```
feat:       新增 RAG eval harness
test:       新增测试用例
docs:       新增文档
chore:      构建/工具变更
```

### 16.4 分支策略

```bash
# 创建开发分支
git checkout -b phase1-rag-eval-harness

# 开发完成后 PR 到 main
git push origin phase1-rag-eval-harness
# 创建 PR: phase1-rag-eval-harness → main

# 合并后打 tag
git tag v1.0-rag-eval-harness
git push origin v1.0-rag-eval-harness
```

### 16.5 Tag 规则

| Tag | 条件 | 说明 |
|---|---|---|
| **v1.0-rag-eval-harness** | 所有验收标准通过 | Phase 1 实现完成 |
| **v1.1-demo-data** | Demo data 完成 | 可选 |
| **v1.2-docs-and-report** | 文档和报告完成 | 可选 |
| **v1.3-phase1-complete** | Phase 1 完整验收 | 最终 tag |

---

## 17. Next Step Prompt

### 17.1 Phase 1 计划锁定后

**下一步是 v1.0-rag-eval-harness**：

1. 创建分支：`git checkout -b phase1-rag-eval-harness`
2. 创建目录：`mkdir -p tests/rag_eval/fixtures scripts/demo_data reports docs`
3. 开始实现 RAG eval harness

### 17.2 v1.0 实现顺序

| 序号 | 任务 | 文件 | 预计时间 |
|---|---|---|---|
| 1 | 创建 conftest.py | `tests/rag_eval/conftest.py` | 0.5 天 |
| 2 | 创建测试用例数据 | `tests/rag_eval/fixtures/rag_eval_cases.json` | 0.5 天 |
| 3 | 实现检索精度测试 | `tests/rag_eval/test_retrieval_precision.py` | 1 天 |
| 4 | 实现无答案 fallback 测试 | `tests/rag_eval/test_no_answer_fallback.py` | 0.5 天 |
| 5 | 实现 evidence/citation 测试 | `tests/rag_eval/test_evidence_citation.py` | 0.5 天 |
| 6 | 实现幻觉风险测试 | `tests/rag_eval/test_hallucination_risk.py` | 0.5 天 |
| 7 | 实现评估报告生成 | `scripts/run_rag_eval.py` | 1 天 |
| 8 | 创建 demo 数据 | `scripts/demo_data/*.json` | 0.5 天 |
| 9 | 实现 seed script | `scripts/seed_demo_data.py` | 0.5 天 |
| 10 | 编写文档 | `docs/rag-evaluation.md` | 0.5 天 |
| 11 | 验收测试 | 运行所有测试 | 0.5 天 |

**预计总时间**：7 天

### 17.3 v1.0 完成后

**进入 v1.1-demo-data**（如果 demo data 未在 v1.0 完成）

**进入 v1.2-docs-and-report**（完善文档和报告）

**进入 v1.3-phase1-complete**（最终验收）

---

## 18. Appendix

### 18.1 参考资源

| 资源 | 链接 | 说明 |
|---|---|---|
| **Basjoo GitHub** | https://github.com/haoyiyin/basjoo | 原项目 |
| **Basjoo Fork** | https://github.com/Strange-Men/basjoo | 我的 fork |
| **Basjoo CLAUDE.md** | selected/basjoo/CLAUDE.md | 项目开发指南 |
| **Basjoo AGENTS.md** | selected/basjoo/AGENTS.md | AI 辅助开发指南 |
| **OPEN_SOURCE_AUDIT.md** | OPEN_SOURCE_AUDIT.md | 选型审查报告 |
| **PHASE0_PREP.md** | PHASE0_PREP.md | Phase 0 准备确认 |

### 18.2 关键代码位置

| 功能 | 文件 | 行数 |
|---|---|---|
| **RAG 检索** | `backend/services/kb_retrieval_service.py` | 25-134 |
| **LLM Mock** | `backend/tests/conftest.py` | MockLLMService 类 |
| **测试配置** | `backend/tests/conftest.py` | conftest 配置 |
| **数据库模型** | `backend/models.py` | SQLAlchemy 模型 |

### 18.3 环境变量

| 变量 | 用途 | 是否必需 |
|---|---|---|
| `DEEPSEEK_API_KEY` | LLM API | 可选（有 mock） |
| `JINA_API_KEY` | Embedding API | 可选（有 mock） |
| `QDRANT_URL` | Qdrant 服务地址 | 可选（有 mock） |
| `DATABASE_URL` | 数据库地址 | 可选（默认 SQLite） |
| `REDIS_URL` | Redis 地址 | 可选（自动 fallback） |

---

*Document created: 2026-06-19*
*Phase 1 plan locked: v0.3-phase1-plan*
*Next step: v1.0-rag-eval-harness — 实现 RAG eval harness*

---

## 19. v1.0 Implementation Results

> **Implemented**: 2026-06-19
> **Branch**: `phase1-rag-eval-harness`
> **Commit**: `4a40ae1`
> **Tag**: `v1.0-rag-eval-harness` ✅ pushed

### 19.1 New Files

| File | Description |
|---|---|
| `backend/tests/rag_eval/__init__.py` | Package marker |
| `backend/tests/rag_eval/conftest.py` | Mock embedding, retriever, RAG pipeline fixtures |
| `backend/tests/rag_eval/fixtures/rag_eval_cases.json` | 15 eval cases (EN + ZH) |
| `backend/tests/rag_eval/fixtures/demo_knowledge_base.json` | 3 demo documents, 13 chunks |
| `backend/tests/rag_eval/test_retrieval_precision.py` | Precision@k, recall@k, MRR tests |
| `backend/tests/rag_eval/test_no_answer_fallback.py` | No-answer and low-relevance tests |
| `backend/tests/rag_eval/test_evidence_citation.py` | Citation and source attribution tests |
| `backend/tests/rag_eval/test_hallucination_risk.py` | Hallucination detection tests |
| `backend/scripts/run_rag_eval.py` | Standalone eval runner (mock mode) |
| `backend/reports/rag_eval_report.json` | JSON evaluation report |
| `backend/reports/rag_eval_report.md` | Markdown evaluation report |
| `backend/docs/rag-evaluation.md` | Usage documentation |

### 19.2 Test Results

**RAG eval harness (23 tests)**:
```
tests/rag_eval/ — 23 passed in 0.20s
```

**Eval runner**:
```
Total cases:      15
Passed:           15
Failed:           0
Precision@3:      0.567
Recall@3:         0.978
Precision@5:      0.527
Recall@5:         1.000
MRR:              0.600
No-Answer Acc:    100.0%
Citation Acc:     88.9%
Hallucination:    0 cases
```

**Regression check (existing tests)**:
```
267 passed, 36 failed, 1 skipped — matches v0.2 baseline exactly
```

### 19.3 Mock / No-API-Key Approach

- **Mock embedding**: Word-level bag-of-words with deterministic hash (256-dim), no API key
- **Mock retriever**: Hybrid scoring (70% keyword overlap + 30% vector similarity), in-memory
- **Mock RAG pipeline**: Synthesises answers from retrieved chunks, no LLM call
- **No Qdrant**: All retrieval is in-memory
- **No Docker**: Pure Python, runs in existing venv

### 19.4 What Was Not Done (v1.0 scope)

- ❌ Demo data seed (deferred to v1.1)
- ❌ Frontend UI changes
- ❌ Core architecture changes
- ❌ Real API key integration
- ❌ Docker / Qdrant dependency
- ❌ Heavy eval frameworks (RAGAS, DeepEval, LangChain)
