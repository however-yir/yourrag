# YourRAG

> A customizable RAG + Agent platform forked from RAGFlow, tailored for private deployment and vertical business use cases.

## 1. 项目定位

`YourRAG` 是一个基于开源 RAGFlow 深度定制的知识增强与智能体平台。
本仓库的目标不是“原版镜像”，而是为你的实际业务场景提供：

- 可控的私有化部署能力
- 可替换的模型与数据源接入能力
- 更安全的默认配置
- 明确的二次开发边界

## 2. 本仓库已做的关键改动

本分支当前已完成第一阶段改造（品牌与配置基础层）：

- 品牌入口改造：
  - Web 标题改为 `YourRAG`
  - 前端默认应用名改为 `YourRAG`
  - 默认云域名常量改为 `cloud.yourrag.io`
- 默认账号与安全基线调整：
  - 默认超管邮箱从 `admin@ragflow.io` 调整为 `admin@yourrag.local`
  - 默认超管密码从 `admin` 调整为 `change_me_please`
- 默认配置去敏：
  - `docker/.env` 与 `service_conf` 中的默认密码改为 `change_me_*`
  - 默认数据库命名从 `rag_flow` / `ragflow_doc` 调整为 `yourrag` / `yourrag_doc`
- 包发布名调整（用于后续独立发布）：
  - Python 主包：`yourrag`
  - Python SDK：`yourrag-sdk`
  - Admin CLI：`yourrag-cli`
- Helm 元信息调整：
  - Chart 名称：`yourrag`

## 3. 与原版的关系

- 本项目基于上游 RAGFlow 进行二次开发。
- 许可证遵循上游 Apache-2.0（见 `LICENSE`）。
- 额外归属说明见 `NOTICE`。
- 你可在此基础上继续添加私有业务模块、私有协议与商业发行策略。

## 4. 快速启动（Docker）

### 4.1 准备配置

进入 docker 目录后，先检查并修改敏感配置：

```bash
cd docker
# 重点修改：MYSQL_PASSWORD, REDIS_PASSWORD, MINIO_PASSWORD,
# ELASTIC_PASSWORD, OPENSEARCH_PASSWORD, DEFAULT_SUPERUSER_PASSWORD
```

### 4.2 启动

```bash
docker compose -f docker-compose.yml up -d
```

### 4.3 健康检查

```bash
docker ps
```

如果你启用了 Web 网关，默认端口通常为 `80/443`（取决于 `docker/.env`）。

## 5. 建议的生产部署基线

上线前建议至少完成以下动作：

1. 将所有 `change_me_*` 默认口令替换为强随机值。
2. 使用外部密钥管理（Vault/KMS/Secrets Manager）托管密钥。
3. 关闭默认开放注册（按需设置 `REGISTER_ENABLED`）。
4. 使用 HTTPS、固定镜像版本（digest pinning）和最小权限运行。
5. 为数据库、对象存储、Redis 配置独立网络与访问控制。

## 6. 开发与二次改造路线

建议按以下顺序推进你的深度私有化：

1. 全量品牌替换（UI 文案、多语言、CLI 提示、镜像名、发布名）
2. 模块化重构（ingestion / retrieval / agent / connector 分层）
3. 配置中心化（环境隔离：dev/staging/prod）
4. CI/CD 与安全扫描（SAST/依赖漏洞/Secret Scan）
5. 上游同步策略（定义可回放的 rebase / cherry-pick 规则）

## 7. 当前已知事项

- 本阶段主要完成“品牌入口 + 默认配置去敏 + 文档重建”。
- 代码中仍有部分 `ragflow` 历史标识（尤其在多语言文案、测试、内部变量名中），这是后续第二阶段会清理的内容。
- 如果你要对外发布，建议补齐：
  - 仓库描述与 Topics
  - 新 Logo 与品牌资产
  - Release/Changelog 与升级指南

## 8. 免责声明

- 本仓库中的附加协议模板仅用于工程示例，不构成法律意见。
- 对外发布前，请让法务或专业律师审阅许可证与商业条款。

---

如果你希望，我可以继续直接执行第二阶段：

- 全仓品牌残留扫描与安全替换（分批）
- 构建一个你自己的发布分支结构（community / enterprise）
- 生成完整部署文档（单机、Docker Compose、Kubernetes）
