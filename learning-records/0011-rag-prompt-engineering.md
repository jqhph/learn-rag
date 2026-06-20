# L11: Prompt Engineering 在 RAG 中的实践

## 学习日期
2026-06-20

## 核心知识点

### System Prompt 四块积木设计骨架
- **角色设定**：告诉 LLM 扮演什么角色（如"客服助手"），设定行为边界
- **知识范围**：限定知识来源（严格检索/检索优先/检索+推理）
- **拒答指令**：知识不足时的行为规范（简洁拒答/透明拒答/引导拒答）
- **输出格式**：规范回答结构（引用标注/表格输出/多步推理/简洁回答）
- 四块积木相互独立，可以 A/B 测试每一块的效果

### Grounding Prompt 三种模式
- **strict**：Faithfulness 0.92-0.98，幻觉率 < 5%，适合金融/医疗
- **standard**：Faithfulness 0.85-0.93，适合客服 Q&A（推荐）
- **lenient**：允许补充通用知识，适合创意辅助
- 实践中建议从 strict 开始上线，按需放松

### Few-shot 示例策略
- 格式敏感场景（表格/对比/排序）必须加 few-shot
- 动态选择（按关键词或 embedding 匹配）比固定给出更省 token 且效果更好
- 标准 QA 一般不需要

### Chain-of-Thought 在 RAG 中的应用
- **direct**：简单"请一步步思考"，开销最低
- **structured**：已知信息→需要推理→结论，适合可审计场景
- **verify**：先审查每个声称是否有来源支持，适合高准确度场景
- CoT 会增加 1.5x-2.5x 延迟，只在需要多步推理时使用

### 生成参数调优
- RAG 场景 temperature 推荐 0.0-0.1（比纯 LLM 场景低 0.2-0.3）
- temperature > 0.5 时 Faithfulness 急剧下降
- max_tokens 推荐 512-1024（客服 Q&A 场景）
- top_p 推荐固定 0.9

### 多模型 Prompt 适配
- GPT 系列：指令式，不需要过度强调
- DeepSeek：结构化指令（bullet list）
- 通义千问：礼貌指令（加"您好""谢谢"）
- 本地模型（Qwen2.5/Llama）：需要更明确的格式约束 + few-shot

## 关键代码模块
1. `prompt_designer.py` — System Prompt 积木式构建器
2. `grounding_prompts.py` — Grounding Prompt 模板（三种模式）
3. `few_shot_selector.py` — 动态 Few-shot 选择器
4. `rag_cot.py` — CoT 推理 prompt 模板（三种模式）
5. `parameter_tuning.py` — 生成参数调优实验框架
6. `prompt_adapter.py` — 多模型 prompt 适配层
7. `prompt_engineering_rag.py` — 集成所有 prompt 工程能力的 RAG 管线

## 关键认知
- Prompt 是 RAG 系统中最后一道也是最容易调整的质量杠杆——零成本、高杠杆、立竿见影
- 好的 prompt 能让 RAG 幻觉率降低 30-50%
- 检索质量决定天花板，prompt 决定离天花板还有多远
- 积木式设计让 A/B 测试成为可能——固定其他三块只换一块，用 RAGAS 量化评估
