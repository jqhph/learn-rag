# Notes

- User wants to learn in Chinese.
- User's first-stage target: customer-service Q&A RAG demo plus core concepts and basic usage.
- User knows Python, Go, and TypeScript, but is new to vector databases and LLM APIs.
- Keep early lessons hands-on and demo-oriented before expanding into enterprise architecture.
- Lesson 2 done — learner now has a runnable Python RAG demo (Chroma + OpenAI). Next lesson should cover real document parsing (PDF/HTML/Markdown) and chunking strategies.
- Lesson 4 done (检索评估与重排序).
- Lesson 5 done (混合检索与查询转换: BM25, RRF, Query Rewrite, Multi-Query, HyDE).
- Lesson 6 done (生产级多源检索架构: 查询路由, Function Calling, NL2SQL, 联邦检索, 数据分水岭原则).
- Lesson 7 done (答案质量评估与 RAGAS: Faithfulness, Answer Relevancy, Context Precision, Context Recall, LLM-as-a-Judge).
- 课程大纲 v3 已规划完成 → 见 `课程大纲.md`（**23节课**，6个模块）。AI写新课前必须先读此文件。
- 用户选择：均衡+知识图谱增强、多模型(通义/DeepSeek)+企业级(Milvus/ES/PG)、Docker Compose自建+云部署展示、客服Q&A+企业知识库双场景。
- 知识图谱(Graph RAG)是用户明确关注的进阶方向，在L19-L20覆盖。
- v2 相比 v1 的核心变化：新增 L8(拒绝策略)和 L10(Prompt Engineering)两节基础课；L9(对话式RAG)提前到模块二；L15 拒绝+护栏拆分为 L8(基础拒答)和 L17(安全护栏)两节课。指导思想：RAG地基不打完不上生产部署。
- L9 已完成 — 拒绝策略与兜底机制（检索置信度评估、三种拒答模板、多层兜底链、覆盖度检测）。
- L10 已完成 — 对话式 RAG 与多轮对话管理（三种 Chat History 策略、指代消解、会话状态管理、多模型适配层、ConversationalRAG 综合管线）。代码文件在 `lessons/0010-conversational-rag.html`，速查表在 `reference/0010-conversational-rag-cheatsheet.html`。
- L11 已完成 — Prompt Engineering 在 RAG 中的实践（System Prompt 四块积木设计骨架、Grounding Prompt 三种模式、动态 Few-shot 选择器、Chain-of-Thought 三种模式、生成参数调优实验框架、多模型 prompt 适配层、PromptEngineeringRAG 集成管线）。代码文件在 `lessons/0011-rag-prompt-engineering.html`，速查表在 `reference/0011-rag-prompt-engineering-cheatsheet.html`，学习记录在 `learning-records/0011-rag-prompt-engineering.md`。
- L12 已完成 — Embedding 模型全景与选型（四类模型横向对比：OpenAI/BGE/Cohere/Jina；MTEB 榜单解读；中文 embedding 深度评测：BGE-zh/m3e/stella/GTE/text2vec；EmbeddingAdapter 适配层设计；Ollama/Infinity 本地部署；维度影响分析；基于 L4 评估框架的 A/B 对比实验）。代码文件在 `lessons/0012-embedding-model-selection.html`，速查表在 `reference/0012-embedding-model-selection-cheatsheet.html`，学习记录在 `learning-records/0012-embedding-model-selection.md`。
- L13 已完成 — Docker Compose 容器化部署（FastAPI 封装 /retrieve、/ask、/evaluate 端点；生产级 Dockerfile 多阶段构建；Docker Compose 编排 RAG API + Chroma + Redis + Nginx 四服务；环境变量与 pydantic-settings 配置管理；健康检查与重启策略；JSON 结构化日志）。代码文件在 `lessons/0013-docker-compose-deployment.html`，速查表在 `reference/0013-docker-compose-deployment-cheatsheet.html`，学习记录在 `learning-records/0013-docker-compose-deployment.md`。
