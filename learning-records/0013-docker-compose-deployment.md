# 学习记录：L13 — Docker Compose 容器化部署

## 核心收获

1. **从 Python 脚本到 HTTP API**：用 FastAPI 将 RAG 管线封装为 /retrieve、/ask、/evaluate 三个标准端点，配合 Pydantic 模型做请求/响应校验和自动 API 文档生成。

2. **生产级 Dockerfile 的核心**：
   - 多阶段构建（builder → runtime）将最终镜像从 ~1.5GB 压缩到 ~200MB
   - slim 基础镜像 + 非 root 用户 + HEALTHCHECK 是生产最低标准
   - COPY 分层的顺序（requirements.txt 单独复制）利用 Docker 缓存加速构建

3. **Docker Compose 编排 4 个服务**：
   - RAG API + Chroma 向量库 + Redis 缓存 + Nginx 网关
   - `depends_on + condition: service_healthy` 控制启动顺序
   - 服务名即 hostname（Python 代码中通过 `http://chroma:8000` 访问）
   - `restart: unless-stopped` 提供基础自愈能力

4. **环境变量与 Secrets 管理**：
   - 用 pydantic-settings 集中管理配置，支持 .env 文件 / 环境变量自动加载
   - .env 文件永远不提交 Git，提交 .env.example 作为模板
   - 不同环境（开发/测试/生产）使用不同的 .env 文件

5. **JSON 结构化日志**：
   - 每行日志是独立的 JSON 对象，可被 Loki/ELK 等系统自动解析
   - 包含 time/level/logger/message + 业务字段（query、latency_ms、session_id）
   - 永远不写 API Key 等敏感信息到日志

6. **Nginx 反向代理**：
   - 统一入口 `:80/api/*` → RAG API `:8000/*`
   - `proxy_read_timeout 120s` 适配 LLM 慢生成
   - JSON 格式的访问日志

## 理解转变

- 过去看待 Docker 只是"Python 环境打包工具"，现在理解它是一种**服务边界划分手段**——每个服务独立容器、独立资源、独立生命周期
- 学完本课才真正理解了 L8 技术栈全景图中"运维成本"这个维度：Chroma 虽然开发体验好，但生产部署仍需容器编排；Pinecone 的"零运维"优势在此时显得非常突出
- JSON 日志看似是小事，但它直接决定了线上故障排查的效率——从"翻日志搜关键词"变成"在 Grafana 里拖图表看趋势"

## 后续关联

- L14（云部署与 CI/CD）将在本课的 docker-compose.yml 基础上，部署到阿里云 ECS 并接入 GitHub Actions
- L15（监控与可观测性）将收集本课的 JSON 日志，在 Loki + Grafana 中建立仪表盘
- L17（成本控制与性能优化）将对本课的 Redis 缓存做深度使用

## 为什么学这个

没有本课的容器化封装，你的 RAG 系统始终只是一段"能跑在你自己电脑上的代码"。企业级交付的第一步就是**可部署性**——把代码+环境+依赖打包成别人能一键启动的制品。
