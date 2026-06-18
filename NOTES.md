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
