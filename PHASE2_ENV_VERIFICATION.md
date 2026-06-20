# CustomerOpsAgent_2｜Phase 2 Environment Verification

> **Created**: 2026-06-20
> **Version**: v1.6.1-phase2-env-verification
> **Purpose**: Phase 2 环境验证，不是功能开发
> **Status**: NOT READY FOR v2.0

---

## 1. Purpose

本文档记录 v1.6.1 环境验证的完整结果。目标是验证用户手动完成的 Docker / Qdrant / SiliconFlow 环境是否可用。

**本轮只做环境验证，不做功能开发。**

- ✅ Docker / WSL 状态验证
- ✅ Qdrant 启动验证（依赖 Docker）
- ✅ .env 安全状态验证
- ✅ SiliconFlow API Key 检查
- ❌ 不写真实 Qdrant adapter
- ❌ 不写 v2.0 功能代码
- ❌ 不修改 Basjoo 核心源码
- ❌ 不执行真实 API 请求（API Key 未配置）

---

## 2. Docker / WSL Verification

### 2.1 检查命令与结果

| 命令 | 结果 | 说明 |
|---|---|---|
| `docker --version` | `command not found` | Docker 未安装 |
| `docker compose version` | `command not found` | Docker Compose 未安装 |
| `docker info` | `command not found` | Docker 未安装 |
| `wsl --status` | exit code 1（乱码） | WSL 未安装 |
| `wsl -l -v` | exit code 1（乱码） | WSL 未安装 |

### 2.2 结论

```
Docker: NOT INSTALLED
WSL: NOT INSTALLED
Docker Compose: NOT INSTALLED
```

### 2.3 阻塞影响

- 无法启动 Qdrant 容器
- 无法执行 Qdrant health check
- 无法执行 Qdrant 临时 collection 测试
- 无法进入 v2.0

---

## 3. Qdrant Verification

### 3.1 启动状态

```
BLOCKED — 依赖 Docker Desktop，Docker 未安装
```

| 检查项 | 结果 |
|---|---|
| 容器是否启动 | ❌ BLOCKED（无 Docker） |
| 容器名 | basjoo-qdrant（计划中） |
| 端口 | 127.0.0.1:6333（计划中） |
| health check | ❌ 未执行 |
| 临时 collection 测试 | ❌ 未执行 |
| 临时 collection 清理 | ❌ N/A |

### 3.2 原因

Docker Desktop 未安装，无法执行任何 Docker 操作。

---

## 4. SiliconFlow Verification

### 4.1 .env 安全状态

| 检查项 | 结果 | 说明 |
|---|---|---|
| .env 是否存在 | ✅ 存在 | `selected/basjoo/.env` |
| .env 是否被 gitignore | ✅ 已忽略 | `.gitignore:173:.env .env` |
| QDRANT_URL | ✅ 存在 | `http://localhost:6333` |
| SILICONFLOW_API_KEY | ❌ 不存在 | .env 中未配置 |
| EMBEDDING_PROVIDER | ❌ 不存在 | .env 中未配置 |

### 4.2 .env 当前变量

```
DATABASE_URL=***（已配置）
REDIS_URL=***（已配置）
QDRANT_URL=***（已配置，值为 http://localhost:6333）
SECRET_KEY=***（已配置）
ALLOWED_ORIGINS=***（已配置）
ALLOWED_METHODS=***（已配置）
ALLOWED_HEADERS=***（已配置）
LOG_LEVEL=***（已配置）
RATE_LIMIT_PER_MINUTE=***（已配置）
RATE_LIMIT_BURST_SIZE=***（已配置）
DEFAULT_RATE_LIMIT=***（已配置）
```

**缺少**：
- `SILICONFLOW_API_KEY`
- `EMBEDDING_PROVIDER`

### 4.3 API Key 安全验证

| 安全检查项 | 结果 |
|---|---|
| .env 被 .gitignore 忽略 | ✅ |
| 不打印真实 API Key | ✅ |
| 不把 API Key 写入文档 | ✅ |
| 不把 API Key 发给 Agent | ✅ |

### 4.4 SiliconFlow 连通性测试

```
NOT EXECUTED — SILICONFLOW_API_KEY 未配置
```

无法执行真实 API 请求。

---

## 5. Ready for v2.0 Decision

```
NOT READY FOR v2.0
```

### 阻塞项清单

| 序号 | 阻塞项 | 当前状态 | 必须满足 | 用户需要做什么 |
|---|---|---|---|---|
| 1 | Docker Desktop | ❌ 未安装 | ✅ 是 | `winget install -e --id Docker.DockerDesktop`，可能需要重启 |
| 2 | WSL2 | ❌ 未安装 | ✅ 是 | Docker Desktop 安装时自动配置 |
| 3 | Qdrant 可启动 | ❌ 需要 Docker | ✅ 是 | Docker 安装后执行 `docker run -d --name basjoo-qdrant -p 127.0.0.1:6333:6333 -v qdrant-data:/qdrant/storage qdrant/qdrant:latest` |
| 4 | Qdrant health check | ❌ 需要 Docker | ✅ 是 | 启动后执行 `curl http://localhost:6333/health` |
| 5 | SILICONFLOW_API_KEY | ❌ 未配置 | ✅ 是 | 注册 https://siliconflow.cn/ 获取 Key，添加到 `selected/basjoo/.env` |
| 6 | EMBEDDING_PROVIDER | ❌ 未配置 | ✅ 是 | 在 `selected/basjoo/.env` 中添加 `EMBEDDING_PROVIDER=siliconflow` |

---

## 6. If Ready（当前不适用）

当所有阻塞项解决后，下一步允许进入：

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

---

## 7. If Not Ready — 用户需要完成的操作

### 7.1 安装 Docker Desktop

```powershell
winget install -e --id Docker.DockerDesktop
```

**注意事项**：
- 可能需要管理员权限
- 安装过程会自动配置 WSL2 后端
- 可能需要重启电脑
- 安装后需要重新打开终端验证

**安装后验证**：
```powershell
docker --version
docker compose version
docker info
wsl -l -v
```

### 7.2 启动 Qdrant

```powershell
docker run -d --name basjoo-qdrant -p 127.0.0.1:6333:6333 -v qdrant-data:/qdrant/storage qdrant/qdrant:latest
```

**验证**：
```powershell
docker ps
curl http://localhost:6333/health
curl http://localhost:6333
```

### 7.3 设置 SiliconFlow API Key

1. 注册 https://siliconflow.cn/
2. 获取 API Key
3. 编辑 `selected/basjoo/.env`，添加：

```env
SILICONFLOW_API_KEY=your_key_here
EMBEDDING_PROVIDER=siliconflow
```

**注意**：
- 不要把真实 Key 告诉 Agent
- 不要把 Key 写入任何文档
- .env 已被 .gitignore 忽略，不会被提交

### 7.4 验证完成后的下一步

所有操作完成后，再次运行环境验证：
- Docker 可用
- Qdrant health check 通过
- SiliconFlow API Key 已配置
- 进入 v2.0-real-qdrant-eval-adapter

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
*Version: v1.6.1-phase2-env-verification*
*Decision: NOT READY FOR v2.0*
*Next step: User installs Docker Desktop + sets up SiliconFlow API Key*
