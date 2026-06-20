# CustomerOpsAgent_2｜Open-source AI Customer Support Agent Enhancement

## Overview

This is a **project management and documentation repository** for enhancing an open-source AI customer support platform.

**Not building from scratch** — instead, performing secondary development on [Basjoo](https://github.com/haoyiyin/basjoo), focusing on RAG quality evaluation, demo data, and engineering verifiability.

## Repository Structure

```
CustomerOpsAgent_2/          ← You are here (project management / docs)
├── README.md                # This file
├── README_WORKSPACE.md      # Workspace conventions
├── PHASE0_PREP.md           # Phase 0 preparation checklist
├── PHASE1_PLAN.md           # Phase 1 implementation plan
├── OPEN_SOURCE_AUDIT.md     # Open-source project selection audit
├── candidates/              # Candidate projects (gitignored, not committed)
└── selected/                # Active development (gitignored, not committed)
    └── basjoo/              # Fork for secondary development
```

**Code repository**: [Strange-Men/basjoo](https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness)

## Why Two Repositories?

| Repository | Purpose |
|---|---|
| **CustomerOpsAgent_2** (this) | Project management, selection audit, planning, progress tracking |
| **Strange-Men/basjoo** | Actual code development (fork of haoyiyin/basjoo) |

This separation:
- Keeps the management repo clean (no source code)
- Preserves original project Git history
- Allows independent versioning of docs vs code

## Version History

### Management Repository (this)

| Version | Status | Description |
|---|---|---|
| v0.1-audit | ✅ | Open-source project selection complete |
| v0.2-deep-audit | ✅ | Deep audit of Basjoo |
| v0.3-phase0-prep | ✅ | Phase 0 preparation complete |

### Code Repository (basjoo fork)

| Version | Status | Description |
|---|---|---|
| v0.1-fork-baseline | ✅ | Fork baseline established |
| v0.2-setup-verified | ✅ | Local setup and testing verified |
| v0.2.5-product-walkthrough | ✅ | Product experience complete |
| v0.3-phase1-plan | ✅ | Phase 1 plan locked |
| v1.0-rag-eval-harness | ✅ | RAG evaluation harness implemented |
| v1.1-demo-data | ✅ | SmartHome demo data implemented |
| v1.2-docs-and-report | 🔄 | Documentation and report polish (current) |

## Core Achievements

### RAG Evaluation Harness

- **15 eval cases** covering: normal hit, multi-doc retrieval, no-answer fallback, low-relevance reject, evidence citation, hallucination risk
- **37 pytest tests** passed
- **Mock pipeline**: mock retriever, mock embedding, mock RAG — no real API keys needed
- **Metrics**: Precision@k, Recall@k, MRR, No-Answer Accuracy, Citation Accuracy, Hallucination Risk

### SmartHome Demo Data

- **2 demo agents** with system prompts and escalation policies
- **3 knowledge documents**: product FAQ, return policy, troubleshooting
- **15 demo questions** with categories and risk types
- **3 conversation scenarios**: normal, escalation, no-answer
- **8 bad cases**: hallucination traps, policy fabrication, cross-doc confusion

### Evaluation Report

- JSON + Markdown report generation
- Reproducible results without external dependencies
- Baseline safety check (no regression to existing tests)

## How to View the Code

Go to the **basjoo repository** on the `phase1-rag-eval-harness` branch:

```
https://github.com/Strange-Men/basjoo/tree/phase1-rag-eval-harness
```

Key files:

| Path | Description |
|---|---|
| `backend/tests/rag_eval/` | pytest test suite (37 tests) |
| `backend/scripts/run_rag_eval.py` | Standalone eval runner |
| `backend/scripts/seed_demo_data.py` | Demo data seeder |
| `backend/scripts/demo_data/` | Demo dataset (JSON + Markdown) |
| `backend/reports/rag_eval_report.md` | Evaluation report |
| `backend/docs/rag-evaluation.md` | Usage documentation |
| `ENHANCEMENT_SUMMARY.md` | Enhancement overview |

## How to Run

```bash
cd backend

# Run RAG eval tests
.\venv\Scripts\python.exe -m pytest tests/rag_eval/ -v

# Run eval runner (mock mode)
.\venv\Scripts\python.exe scripts/run_rag_eval.py --mock

# Validate demo data
.\venv\Scripts\python.exe scripts/seed_demo_data.py --validate-only

# Preview demo data
.\venv\Scripts\python.exe scripts/seed_demo_data.py --dry-run

# Generate mock fixtures
.\venv\Scripts\python.exe scripts/seed_demo_data.py --mock
```

## Results Summary

| Metric | Value |
|---|---|
| RAG eval tests | 37 passed |
| Eval runner (mock) | 15 passed, 0 failed |
| Baseline tests | 267 passed, 36 failed, 1 skipped |
| New regression | 0 |
| Precision@3 | 0.567 |
| Recall@3 | 0.978 |
| No-Answer Accuracy | 100% |
| Citation Accuracy | 88.9% |
| Hallucination cases | 0 |

## Resume Description

### Chinese

基于开源 AI 客服系统 Basjoo 进行二次开发，构建 RAG Evaluation Harness 与 SmartHome Demo Data，支持检索精度、无答案回退、证据引用与幻觉风险等评估场景，在无真实 API Key / 无 Qdrant 环境下通过 Mock pipeline 完成可复现评估，并生成 JSON / Markdown 评估报告。

### English

Enhanced an open-source AI customer support platform by adding a mock-friendly RAG evaluation harness, demo dataset, no-answer fallback checks, citation validation, hallucination risk tests, and reproducible JSON/Markdown evaluation reports.

## Tech Stack

- Python / FastAPI (backend)
- TypeScript / Next.js 14 (frontend)
- PostgreSQL + Qdrant (data layer)
- Docker Compose (deployment)

## License

Based on [Basjoo](https://github.com/haoyiyin/basjoo) (MIT License).

---

*Created: 2026-06-19*
*Last updated: 2026-06-20*
