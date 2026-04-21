# YourRAG - 企业级私有检索增强平台 | Enterprise Private RAG Platform

[![Tests](https://github.com/however-yir/yourrag/actions/workflows/tests.yml/badge.svg)](https://github.com/however-yir/yourrag/actions/workflows/tests.yml)
[![Docs](https://img.shields.io/badge/docs-deployment-0A7EFA)](https://github.com/however-yir/yourrag/tree/main/docs)
[![License](https://img.shields.io/badge/license-Apache--2.0-16A34A)](./LICENSE)
[![Status](https://img.shields.io/badge/status-active-2563EB)](https://github.com/however-yir/yourrag)

> Status: `active`
>
> Upstream: `infiniflow/ragflow`

> **非官方声明（Non-Affiliation）**  
> 本仓库为社区维护的衍生/二次开发版本，与上游项目及其权利主体不存在官方关联、授权背书或从属关系。  
> **商标声明（Trademark Notice）**  
> 相关项目名称、Logo 与商标归其各自权利人所有。本仓库仅用于说明兼容/来源，不主张任何商标权利。
>
> Series: [local-ai-hub](https://github.com/however-yir/local-ai-hub) · [LZKB](https://github.com/however-yir/LZKB)

> **非官方声明（Non-Affiliation）**<br>
> `YourRAG` 是基于 `infiniflow/ragflow` 的社区维护衍生版，与上游项目及其权利主体不存在官方关联、授权背书或从属关系。<br>
> **商标声明（Trademark Notice）**<br>
> `RAGFlow` 及相关项目名称、Logo 与商标归其各自权利人所有；本仓库仅用于说明上游来源与兼容关系。

![YourRAG Logo](./assets/yourrag-logo.svg)

YourRAG 是一个基于 RAGFlow 深度改造的私有化 RAG + Agent 平台，目标是用于你的自有品牌交付与可持续二开。

## 项目快照

- 定位：企业交付导向的私有 RAG + Agent 平台。
- 亮点：RAGFlow 深度改造、部署形态完整、默认安全收敛、CI 通用化。
- 最短运行路径：`cd docker && cp .env.example .env.local && docker compose --env-file .env.local -f docker-compose.yml up -d`
- 系列分工：`YourRAG` 面向企业 RAG/Agent 交付，`LZKB` 面向知识平台，`Local AI Hub` 面向本地工作台。

## AI 平台分工矩阵

| Repo | 主要角色 | 部署形态 | 最适合的场景 |
| --- | --- | --- | --- |
| `Local AI Hub` | 本地 AI 工作台 | 自托管工作台 | 模型接入、团队日用、统一入口 |
| `LZKB` | 知识库平台 | 本地优先平台 | 文档入库、知识运营、检索问答 |
| `YourRAG` | 企业 RAG/Agent 平台 | 企业交付导向 | 私有化部署、RAG + Agent 交付 |

## 核心定位

- 自有品牌：项目名、默认账号、镜像、配置入口已迁移到 `YourRAG`。
- 安全优先：默认配置去敏，私钥不再提交到仓库。
- 可运维：补齐单机、Docker Compose、Kubernetes 三套部署文档。
- 可演进：保留对上游部分兼容能力，便于后续同步与迁移。

## 已完成改造

- Go 模块从 `ragflow` 迁移为 `yourrag`，内部导入路径同步更新。
- 默认管理员改为 `admin@yourrag.local`，默认口令改为 `change_me_please`。
- `service_conf` 顶层服务键从 `ragflow` 改为 `yourrag`，并保留旧键兼容读取。
- Token 前缀改为 `yourrag-`，同时保留 `ragflow-` 兼容解析。
- 私钥/公钥从仓库移除并加入忽略规则；新增一键生成脚本。
- 前端与 CLI 登录加密改为 `RSA 可选 + base64 回退`，避免强依赖仓库内密钥。
- CI 重构为 GitHub Hosted Runner 的通用流水线。

完整清单见：`docs/customization/completed-checklist.md`

## 部署文档

- 单机部署：`docs/deployment/single-host.md`
- Docker Compose：`docs/deployment/docker-compose.md`
- Kubernetes/Helm：`docs/deployment/kubernetes.md`

## 企业部署拓扑

```mermaid
flowchart LR
  U["Business Users / Ops"] --> WEB["Web Console"]
  WEB --> API["YourRAG API"]
  API --> CONF["conf/service_conf.yaml"]
  API --> STORE["Vector / Search / DB"]
  API --> AGENT["RAG + Agent Runtime"]
  AGENT --> MODEL["LLM / Embedding Providers"]
  API --> OBS["Docs / Release / Health Checks"]
```

## 快速启动（Docker Compose）

```bash
cd docker
cp .env.example .env.local
# 修改密码与管理员账号后启动

docker compose --env-file .env.local -f docker-compose.yml up -d
curl -f http://127.0.0.1:9380/v1/system/ping
```

轻量开发档位（更低资源占用）：

```bash
cd docker
cp .env.local.lite.example .env.local.lite
docker compose --env-file .env.local.lite -f docker-compose.yml up -d
```

## 评测与观测路径

| 层面 | 入口 | 用途 |
| --- | --- | --- |
| 健康探针 | `curl /v1/system/ping` | 启动后最小可用性检查 |
| 快速上手 | `docs/quickstart.mdx` | 新成员最短上手路径 |
| 部署说明 | `docs/deployment/*` | 单机、Compose、K8s 三套部署面 |
| 发布记录 | `docs/release_notes.md` | 版本变化与交付说明 |
| Helm 部署 | `helm/README.md` | 企业 K8s 安装入口 |
| 配置基线 | `conf/service_conf.yaml` | 服务级默认配置 |
| 密钥准备 | `tools/scripts/generate_rsa_keys.sh` | 私钥/公钥初始化 |

## RSA 密钥（可选）

```bash
./tools/scripts/generate_rsa_keys.sh
```

如未配置 RSA 密钥，系统会回退为 base64 传输模式以保证可用性。

## 与上游关系

- 本项目基于上游 RAGFlow 进行二次开发。
- 继续遵循 Apache-2.0（见 `LICENSE`）。
- 归属与附加说明见 `NOTICE` 与 `LICENSE-ADDITIONAL.md`。
## Engineering Quality

This repository includes a contract-based quality baseline to keep essential engineering standards stable over time.

- Quality plan: [docs/ENGINEERING_QUALITY.md](docs/ENGINEERING_QUALITY.md)
- Contract tests: [tests/repo_contract_test.sh](tests/repo_contract_test.sh)
- Contract CI workflow: [.github/workflows/repo-contract-ci.yml](.github/workflows/repo-contract-ci.yml)

Run local contract checks:

```bash
bash tests/repo_contract_test.sh
```
