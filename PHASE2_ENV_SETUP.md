# CustomerOpsAgent_2｜Phase 2 Environment Setup

> **Created**: 2026-06-20
> **Version**: v1.6-phase2-environment-setup
> **Purpose**: Phase 2 环境准备与安全验证，不是功能开发
> **Status**: READY FOR v2.0 (v1.6.2 验证结果)

---

## 1. Purpose

本文档记录 Phase 2 环境准备的检查结果和下一步行动。目标是为 v2.0-real-qdrant-eval-adapter 准备好基础设施。

**本轮只做环境准备和安全验证，不做功能开发。**

- ✅ Docker / WSL 状态检查
- ✅ Qdrant 启动方案确认
- ✅ SiliconFlow API Key 安全方案
- ✅ 环境变量配置方案
- ❌ 不写真实 Qdrant adapter
- ❌ 不写 v2.0 功能代码
- ❌ 不修改 Basjoo 核心源码
- ❌ 不接真实 API Key（除非用户明确确认）

---

## 2. Selected Phase 2 Strategy

用户已做出以下决策：

| 决策项 | 选择 | 说明 |
|---|---|---|
| **Qdrant 方案** | Docker Desktop + 本地 Qdrant | 最兼容，需要安装 Docker Desktop |
| **Embedding 方案** | SiliconFlow | 国内网络友好，原生支持 |
| **时间上限** | 1 周 | v2.0 必须在 1 周内完成最小闭环 |
| **Phase 2 最小目标** | 真实 Qdrant retrieval eval | 不做完整 API-level chat eval |
| **v2.0 范围** | 3-5 个 real retrieval eval cases | 生成 mock vs real 对比报告 |

---

## 3. Docker / WSL Status

### 3.1 检查结果（2026-06-23）

| 工具 | 版本 | 状态 | 说明 |
|---|---|---|---|
| Docker | 29.5.3 | ✅ 已安装 | Docker Desktop, desktop-linux context |
| Docker Compose | 5.1.4 | ✅ 已安装 | Docker Compose plugin |
| WSL | 2.7.8.0 | ✅ 已安装 | WSL2, docker-desktop Running |
| Windows | 11 Home China Build 26200 | ✅ | 支持 WSL2 和 Docker Desktop |

### 3.2 Docker Desktop 安装计划

**安装命令**：
```powershell
winget install -e --id Docker.DockerDesktop
```

**注意事项**：
- Docker Desktop 可能需要管理员权限
- 安装过程会自动配置 WSL2 后端
- 可能需要重启电脑
- 安装可能下载较大文件（~500MB）
- 安装后需要重新打开终端验证
- 不会修改项目代码
- 不会提交任何环境文件

**安装后验证**：
```powershell
docker --version
docker compose version
docker info
wsl -l -v
```

**如果需要重启**：停止后续步骤，等待用户重启后继续。

---

## 4. Qdrant Local Setup Status

### 4.1 docker-compose.yml 中的 Qdrant 配置

| 配置项 | 值 |
|---|---|
| 服务名 | `qdrant` |
| 镜像 | `qdrant/qdrant:latest` |
| 容器名 | `basjoo-qdrant` |
| 端口 | `127.0.0.1:6333:6333` |
| 数据卷 | `qdrant-data:/qdrant/storage` |
| 健康检查 | bash TCP probe to `/readyz` |
| Profile | `["prod", "dev"]` |

### 4.2 最小启动方案

**注意**：Qdrant 服务配置了 profile `["prod", "dev"]`，不能单独用 `docker compose up -d qdrant` 启动。

**方案 A（推荐）：直接 docker run**
```powershell
docker run -d --name basjoo-qdrant -p 127.0.0.1:6333:6333 -v qdrant-data:/qdrant/storage qdrant/qdrant:latest
```
- 只启动 Qdrant，不启动其他服务
- 不需要 docker-compose.yml
- 端口和数据卷与 compose 配置一致

**方案 B：docker compose --profile dev**
```powershell
cd "D:\Claude_workfile\CustomerOpsAgent_2\selected\basjoo"
docker compose --profile dev up -d qdrant
```
- 会同时创建 basjoo-network
- 只启动 qdrant 服务（其他 dev profile 服务需要显式指定）

### 4.3 健康检查验证

启动后验证：
```powershell
docker ps
curl http://localhost:6333/health
curl http://localhost:6333
```

### 4.4 当前状态（v1.6.2 验证）

```
READY — Qdrant 容器运行中，API 可访问
```

**v1.6.2 验证结果（2026-06-23）**：
- Docker：✅ v29.5.3
- Docker Compose：✅ v5.1.4
- WSL：✅ v2.7.8.0 (WSL2)
- Qdrant 容器：✅ basjoo-qdrant 运行中
- Qdrant 版本：✅ v1.18.2
- Qdrant API：✅ /health OK, /collections OK
- 详见 `PHASE2_ENV_VERIFICATION.md`

---

## 5. SiliconFlow API Key Safety Plan

### 5.1 Basjoo 需要的环境变量

| 变量 | 来源 | 用途 | 必需 |
|---|---|---|---|
| `QDRANT_URL` | `.env` 或 docker-compose | Qdrant 服务地址 | ✅ |
| `QDRANT_API_KEY` | `.env` 或 docker-compose | Qdrant API Key（本地可选） | ❌ |
| `SILICONFLOW_API_KEY` | `.env` 或 Agent DB | SiliconFlow Embedding API Key | ✅ |

### 5.2 SiliconFlow API Key 存储方式

Basjoo 的 API Key 存储在 **Agent 数据库记录**中，不是环境变量：

| 字段 | 类型 | 说明 |
|---|---|---|
| `agent.siliconflow_api_key` | VARCHAR(500) | 加密存储，`encrypt_api_key()` |
| `agent.embedding_provider` | VARCHAR(20) | `jina` / `siliconflow` / `custom` |

**环境变量 `SILICONFLOW_API_KEY`** 在 docker-compose.yml 中传递给容器，但实际使用时是从 Agent DB 读取。

### 5.3 API Key 安全措施

| 措施 | 状态 |
|---|---|
| `.gitignore` 忽略 `.env` | ✅ 外层和 basjoo 都已配置 |
| `.gitignore` 忽略 `.env.*` | ✅ 已配置 |
| `.gitignore` 忽略 `*.env` | ✅ 已配置 |
| API Key 加密存储 | ✅ Basjoo 使用 `encrypt_api_key()` |
| 不在文档中写真实 Key | ✅ 只使用占位符 |
| 不在终端输出 Key | ✅ 只检查存在性 |

### 5.4 本地 .env 配置方案

创建 `selected/basjoo/.env`（已被 .gitignore 忽略）：

```env
# Qdrant
QDRANT_URL=http://localhost:6333
QDRANT_API_KEY=
QDRANT_TIMEOUT=30.0

# SiliconFlow Embedding
SILICONFLOW_API_KEY=your_key_here

# 其他（可选）
DEEPSEEK_API_KEY=
SECRET_KEY=
```

### 5.5 API Key 验证方案（不打印 Key）

验证 Key 是否存在：
```powershell
# 检查环境变量是否存在（不打印值）
if ($env:SILICONFLOW_API_KEY) { Write-Host "SILICONFLOW_API_KEY is set" } else { Write-Host "SILICONFLOW_API_KEY is NOT set" }
```

验证 SiliconFlow 连通性（最小请求）：
```python
import os
import requests

api_key = os.environ.get("SILICONFLOW_API_KEY")
if not api_key:
    print("ERROR: SILICONFLOW_API_KEY not set")
    exit(1)

response = requests.post(
    "https://api.siliconflow.cn/v1/embeddings",
    headers={"Authorization": f"Bearer {api_key}", "Content-Type": "application/json"},
    json={"model": "BAAI/bge-m3", "input": "test"}
)

if response.status_code == 200:
    data = response.json()
    dim = len(data["data"][0]["embedding"])
    print(f"OK: embedding dimension = {dim}")
else:
    print(f"FAIL: HTTP {response.status_code} - {response.text[:100]}")
```

**安全规则**：
- 不把请求内容和 Key 记录到文档
- 失败时只记录 HTTP 状态和错误类型
- 成功时只记录维度和响应是否正常

### 5.6 当前状态（v1.6.2 验证）

```
READY — 所有环境变量已配置，SiliconFlow API 连通
```

**v1.6.2 验证结果（2026-06-23）**：
- .env 存在：✅
- .env 被 gitignore：✅
- QDRANT_URL：✅ `http://localhost:6333`
- EMBEDDING_PROVIDER：✅ `siliconflow`
- SILICONFLOW_API_KEY：✅ 已配置（不打印）
- SILICONFLOW_BASE_URL：✅ `https://api.siliconflow.cn/v1`
- SILICONFLOW_EMBEDDING_MODEL：✅ `Qwen/Qwen3-Embedding-0.6B`
- SiliconFlow 连通性：✅ HTTP 200, 维度 1024
- 详见 `PHASE2_ENV_VERIFICATION.md`

---

## 6. v2.0 Entry Criteria

只有同时满足以下条件，才允许进入 v2.0：

| 序号 | 条件 | 当前状态 | 必须满足 |
|---|---|---|---|
| 1 | Docker Desktop 可用 | ✅ v29.5.3 | ✅ 是 |
| 2 | Qdrant 可启动 | ✅ basjoo-qdrant 运行中 | ✅ 是 |
| 3 | Qdrant health check 通过 | ✅ /health OK | ✅ 是 |
| 4 | SiliconFlow API Key 已配置 | ✅ 已配置 | ✅ 是 |
| 5 | .env 不会被提交 | ✅ .gitignore 已配置 | ✅ 是 |
| 6 | 不需要修改核心架构 | ✅ 只扩展 2 个脚本 | ✅ 是 |
| 7 | v2.0 范围限制为 3-5 个 real retrieval eval cases | ✅ 已确认 | ✅ 是 |

---

## 7. Current Decision（v1.6.2 验证结果）

```
READY FOR v2.0
```

**全部条件满足**：
- Docker Desktop v29.5.3 可用
- WSL2 v2.7.8.0 可用
- Qdrant v1.18.2 容器运行中，API 可访问
- SiliconFlow API Key 已配置
- EMBEDDING_PROVIDER=siliconflow 已配置
- SiliconFlow embedding 连通性测试通过（HTTP 200, 维度 1024）
- Qdrant 临时 collection 测试通过（创建/插入/查询/删除）
- Basjoo 原生支持 SiliconFlow，不需要修改核心源码

**v1.6.2 验证结果**：详见 `PHASE2_ENV_VERIFICATION.md`

**下一步**：
1. 进入 v2.0-real-qdrant-eval-adapter
2. 扩展 `seed_demo_data.py`：`--write-db` 模式写入 Agent DB
3. 扩展 `run_rag_eval.py`：`--real` 模式使用真实 Qdrant + SiliconFlow
4. 运行 3-5 个 real retrieval eval cases
5. 生成 mock vs real 对比报告

---

## 8. Hard Rules

本轮及后续必须遵守：

1. **不继续求职包装** — 直到 v2.0 闭环
2. **不写面试话术** — 直到 v2.0 闭环
3. **不改 CAREER_PACKAGE.md** — 直到 v2.0 闭环
4. **不提交 .env** — API Key 永远不进 Git
5. **不修改 Basjoo 核心源码** — 本轮不写功能代码
6. **不启动完整 Basjoo 服务栈** — 只启动 Qdrant
7. **不使用 git add . 或 git add *** — 精确提交
8. **不把 mock 指标说成真实指标** — mock 评估不代表真实线上质量

---

*Document created: 2026-06-20*
*Updated: 2026-06-23*
*Version: v1.6.2-phase2-env-final-verification*
*Decision: READY FOR v2.0 (v1.6.2 验证结果)*
*Next step: Enter v2.0-real-qdrant-eval-adapter*
