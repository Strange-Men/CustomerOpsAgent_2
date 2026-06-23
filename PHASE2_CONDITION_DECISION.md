# CustomerOpsAgent_2｜Phase 2 条件确认决策

> **Created**: 2026-06-20
> **Version**: v1.5-phase2-condition-resolution
> **Purpose**: Phase 2 条件确认，不是功能开发
> **Status**: READY FOR v2.0 (v1.6.2 验证结果)

---

## 1. Purpose

本文档是 Phase 2 条件确认的最终决策记录。目标是确认是否满足进入 v2.0-real-qdrant-eval-adapter 的前置条件。

**本轮只做条件确认和方案比较，不做功能开发。**

- ✅ 环境检查
- ✅ 方案比较
- ✅ 成本风险评估
- ✅ Go / No-Go 决策
- ❌ 不写真实 Qdrant adapter
- ❌ 不接真实 API Key
- ❌ 不修改 basjoo 源码
- ❌ 不安装新软件（除非用户确认）

---

## 2. Current Blockers

| 阻塞项 | 当前状态 | 影响 |
|---|---|---|
| Docker | ✅ v29.5.3 | Docker Desktop 已安装，daemon 可用 |
| Docker Compose | ✅ v5.1.4 | 已安装 |
| WSL | ✅ v2.7.8.0 (WSL2) | 已安装，docker-desktop Running |
| Qdrant 运行方案 | ✅ Docker Desktop + 本地 Qdrant | basjoo-qdrant 容器运行中，v1.18.2 |
| Embedding API Key | ✅ SiliconFlow API Key 已配置 | Qwen/Qwen3-Embedding-0.6B, 维度 1024 |
| API Key 安全策略 | ✅ 已实施 | .env + .gitignore，不提交 |
| 时间成本 | ✅ 已确定 | 1 周上限 |

---

## 3. Local Environment Result

### 3.1 检查结果

| 工具 | 版本 | 状态 | 说明 |
|---|---|---|---|
| Docker | — | ❌ 未安装 | `docker: command not found` |
| Docker Compose | — | ❌ 未安装 | `docker compose: command not found` |
| WSL | — | ❌ 未安装 | exit code 1，输出乱码（中文"未安装"提示） |
| Windows | 11 Home China Build 26200 | ✅ | 支持 WSL2 和 Docker Desktop |
| Python | 3.11.5 | ✅ 可用 | 满足 Basjoo 要求 (3.11+) |
| Node.js | v24.15.0 | ✅ 可用 | 满足 Basjoo 要求 (18+) |
| npm | 11.12.1 | ✅ 可用 | 满足 Basjoo 要求 (9+) |
| winget | v1.28.240 | ✅ 可用 | 可用于安装 Docker Desktop |

### 3.2 环境可行性判断

| 判断项 | 结论 |
|---|---|
| **Docker 是否完全不可用** | ❌ 未安装，但可通过 winget 安装 Docker Desktop |
| **WSL 是否可用** | ❌ 未安装，Docker Desktop 安装时会自动配置 WSL2 |
| **Windows 版本是否支持 Docker Desktop** | ✅ Windows 11 Home China Build 26200 支持 WSL2 后端 |
| **是否有不依赖 Docker 的 Qdrant 方案** | ⚠️ Qdrant Cloud 免费层可用（远程服务，无需本地 Docker） |
| **当前机器是否适合做 real Qdrant eval** | ⚠️ 取决于用户选择的方案 |

---

## 4. Qdrant Options Comparison

### 方案 A：Docker Desktop + docker compose

| 维度 | 评估 |
|---|---|
| **前置条件** | 安装 Docker Desktop（winget install Docker.DockerDesktop），自动配置 WSL2 |
| **用户手动操作** | 1. 运行 `winget install Docker.DockerDesktop`；2. 重启电脑；3. 启动 Docker Desktop；4. 等待 WSL2 后端初始化 |
| **风险** | Windows Home 版 WSL2 配置可能遇到坑；首次启动较慢；占用 ~2GB 内存 |
| **成本** | 免费（Docker Desktop 个人使用免费） |
| **对 Basjoo 兼容度** | ✅ 最高 — basjoo 自带 docker-compose.yml，原生支持 |
| **网络要求** | 拉取 Qdrant 镜像需要网络，首次约 100MB |
| **推荐度** | ⭐⭐⭐⭐ 最佳方案，但需要用户手动安装 |

### 方案 B：Qdrant Cloud

| 维度 | 评估 |
|---|---|
| **前置条件** | 注册 Qdrant Cloud 账号（https://cloud.qdrant.io/） |
| **免费额度** | 1GB 存储，100k 向量，免费永久 — demo data 规模远低于此限制 |
| **用户手动操作** | 1. 注册账号；2. 创建免费集群；3. 获取 API Key 和 URL |
| **风险** | 需要稳定网络；API Key 需要安全存储；免费集群可能有冷启动延迟 |
| **成本** | 免费（免费层足够） |
| **对 Basjoo 兼容度** | ✅ 高 — 只需配置 QDRANT_URL 和 QDRANT_API_KEY 环境变量 |
| **网络要求** | 需要稳定网络访问 Qdrant Cloud API |
| **推荐度** | ⭐⭐⭐ 最简方案，无需本地安装，但依赖外部服务 |

### 方案 C：本地 Qdrant 二进制（非 Docker）

| 维度 | 评估 |
|---|---|
| **前置条件** | 下载 Qdrant Linux 二进制，在 WSL 中运行 |
| **用户手动操作** | 1. 确保 WSL 已安装；2. 下载 Qdrant 二进制；3. 在 WSL 中配置和运行 |
| **风险** | 配置复杂；Windows 原生无官方二进制；需要 WSL |
| **成本** | 免费 |
| **对 Basjoo 兼容度** | ⚠️ 中 — 需要手动配置端口映射和网络 |
| **推荐度** | ⭐ 不推荐 — 比 Docker 方案更复杂，收益相同 |

### 方案 D：不进入真实 Qdrant，冻结项目

| 维度 | 评估 |
|---|---|
| **前置条件** | 无 |
| **用户手动操作** | 无 |
| **风险** | 无技术风险；Phase 1 已有完整交付物 |
| **成本** | 零 |
| **对项目收益** | Phase 1 成果保留；缺少真实 RAG 验证；项目说服力有限 |
| **推荐度** | ⭐⭐ 备选方案 — 如果用户不想继续投入 |

### 对比表

```
方案 | 前置条件 | 成本 | 风险 | 对项目收益 | 推荐度
A: Docker Desktop | 安装 Docker + WSL2 | 免费 | 中（配置坑） | 高（最兼容） | ⭐⭐⭐⭐
B: Qdrant Cloud | 注册账号 | 免费 | 低 | 高（无需本地） | ⭐⭐⭐
C: 本地二进制 | WSL + 手动配置 | 免费 | 高（复杂） | 中 | ⭐
D: 冻结项目 | 无 | 零 | 无 | 中（保留 Phase 1） | ⭐⭐
```

---

## 5. Embedding Options Comparison

### 方案 A：Jina Embedding API

| 维度 | 评估 |
|---|---|
| **获取难度** | 低 — 注册 https://jina.ai/ 即可获取 API Key |
| **免费额度** | 1M tokens/月免费 — demo data 规模远低于此限制 |
| **成本** | 免费（免费额度足够） |
| **网络稳定性** | ⚠️ 国内访问可能不稳定，需要科学上网或代理 |
| **与 Basjoo 兼容度** | ✅ 高 — Basjoo 原生支持 Jina（代码中有 jina_api_key 配置） |
| **模型** | jina-embeddings-v3，1024 维 |
| **推荐度** | ⭐⭐⭐⭐ 最佳方案 — 原生支持，免费额度足够 |

### 方案 B：SiliconFlow Embedding API

| 维度 | 评估 |
|---|---|
| **获取难度** | 低 — 注册 https://siliconflow.cn/ 即可获取 API Key |
| **免费额度** | 有免费额度（具体需确认） |
| **成本** | 免费或极低成本 |
| **网络稳定性** | ✅ 国内服务，网络稳定 |
| **与 Basjoo 兼容度** | ✅ 高 — Basjoo 原生支持 SiliconFlow（代码中有 siliconflow_api_key 配置） |
| **模型** | BAAI/bge-m3，1024 维 |
| **推荐度** | ⭐⭐⭐⭐ 最佳方案 — 国内网络友好，原生支持 |

### 方案 C：继续 Mock Embedding

| 维度 | 评估 |
|---|---|
| **获取难度** | 无需获取 — 已有实现 |
| **成本** | 零 |
| **网络稳定性** | ✅ 无需网络 |
| **与 Basjoo 兼容度** | ✅ 已集成 |
| **局限性** | 字符频率向量不是语义向量，不能证明真实 retrieval 效果 |
| **推荐度** | ⭐⭐ 仅作为 fallback — 不能替代真实 embedding |

### 对比表

```
方案 | 获取难度 | 成本 | 网络稳定性 | 与 Basjoo 兼容度 | 推荐度
A: Jina | 低 | 免费 | ⚠️ 国内可能不稳 | ✅ 原生支持 | ⭐⭐⭐⭐
B: SiliconFlow | 低 | 免费/极低 | ✅ 国内稳定 | ✅ 原生支持 | ⭐⭐⭐⭐
C: Mock | 无需获取 | 零 | ✅ 无需网络 | ✅ 已集成 | ⭐⭐
```

---

## 6. User Decision（v1.5 已确认，v1.6 已记录）

```
USER DECISION MADE (2026-06-20)
```

### 6.1 Qdrant 方案

- [x] **A. 安装 Docker Desktop** — 最兼容，需要手动安装 + 重启

### 6.2 Embedding Provider

- [x] **B. SiliconFlow** — 国内网络友好，原生支持

### 6.3 时间投入

- [x] **A. 愿意投入 1 周** — 在 v2.0 范围内完成 real Qdrant eval adapter

### 6.4 API Key 安全策略

- [x] **A. 确认** — 使用 .env 文件，加入 .gitignore，不提交到仓库

### 6.5 当前状态（v1.6.2 验证结果）

| 条件 | 状态 | 说明 |
|---|---|---|
| Docker Desktop | ✅ v29.5.3 | 已安装，daemon 可用 |
| WSL2 | ✅ v2.7.8.0 | 已安装，docker-desktop Running |
| Qdrant | ✅ v1.18.2 | basjoo-qdrant 容器运行中，API 可访问 |
| SiliconFlow API Key | ✅ 已配置 | 不打印 |
| EMBEDDING_PROVIDER | ✅ siliconflow | 已配置 |
| .env 文件 | ✅ 存在且被 gitignore | 所有 5 个变量已配置 |
| SiliconFlow 连通性 | ✅ HTTP 200 | Qwen/Qwen3-Embedding-0.6B, 维度 1024 |
| Qdrant CRUD | ✅ 全部通过 | 创建/插入/查询/删除 |

详见 `PHASE2_ENV_SETUP.md` 和 `PHASE2_ENV_VERIFICATION.md`。

---

## 8. If User Chooses GO

下一步是：

```
v2.0-real-qdrant-eval-adapter
```

### v2.0 最小开发边界

| 项目 | 内容 |
|---|---|
| **范围** | 只做 real retrieval eval adapter |
| **改动文件** | `run_rag_eval.py`（扩展 --real 模式）+ `seed_demo_data.py`（扩展 --write-db 模式） |
| **不改** | 核心架构、前端 UI、API-level chat eval、部署 |
| **eval cases** | 只跑 3-5 个 real retrieval eval cases |
| **交付物** | mock vs real 对比报告 |
| **时间上限** | 1 周 |
| **求职包装** | 不做，直到 v2.0 闭环 |

### v2.0 硬条件

只有同时满足以下条件，才允许进入 v2.0：

1. Qdrant 可运行方案确定（Docker 或 Qdrant Cloud）
2. Embedding provider 确定（Jina 或 SiliconFlow）
3. API Key 安全策略确定（.env + .gitignore）
4. 不提交 .env
5. 开发时间上限确定为 1 周
6. v2.0 范围只做 3-5 个 real retrieval eval cases
7. 不做 API-level chat eval
8. 不做前端 UI
9. 不做部署
10. 不继续求职包装

---

## 9. If User Chooses NO-GO

下一步是：

- 冻结 Basjoo 项目
- 回到 CodePilot / Portfolio 主线
- 保留 Phase 1 成果
- 不再继续 Basjoo 开发

---

## 10. Hard Rules

本轮及后续开发必须遵守：

1. **不继续求职包装** — 开发中期不做简历话术
2. **不写面试话术** — 直到新阶段闭环
3. **不改 CAREER_PACKAGE.md** — 直到新阶段闭环
4. **不提交 .env** — API Key 永远不进 Git
5. **不把 mock 指标说成真实指标** — mock 评估结果不代表真实线上质量
6. **不为了进入 v2.0 强行安装或接入外部服务** — 用户确认后才操作
7. **不改核心架构** — 不修改 backend/services/、backend/models.py、backend/api/ 等核心文件
8. **不重做 UI** — 不涉及 frontend-nextjs/
9. **不做 Vercel 部署** — 不做线上部署
10. **不使用 git add . 或 git add *** — 精确提交

---

*Document created: 2026-06-20*
*Updated: 2026-06-23*
*Version: v1.6.2-phase2-env-final-verification*
*Decision: READY FOR v2.0 (v1.6.2 验证结果)*
*Next step: Enter v2.0-real-qdrant-eval-adapter*
