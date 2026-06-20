# Lesson 10: 对话式 RAG 与多轮对话管理

The learner can now upgrade the single-turn RAG pipeline (L1-L9) to a multi-turn conversational RAG system with history management, referent resolution, session state, and multi-model support.

**New capabilities:**
- **Three Chat History management strategies**: Sliding Window (keep last N turns), Summary Compression (LLM-compress early turns into a summary), and Key Turn Extraction (retain only high-importance turns by scoring rules or LLM). The recommended starting strategy is sliding window with `window_size=6`, since most customer-service conversations resolve within 10 turns and it has zero extra LLM cost.
- **Referent resolution pipe**: A `resolve_references()` function that uses LLM (gpt-4o-mini, ~200ms, ~200 tokens) to rewrite context-dependent queries like "它多少钱？" into self-contained queries like "华为Mate60 Pro多少钱？". A rule-based variant (`resolve_references_rule_based()`, ~5ms) is provided for latency-sensitive scenes. Both are gated by a pronoun-detection check to avoid unnecessary LLM calls.
- **`SessionManager` + `SessionState`**: An in-memory session store that tracks per-user/ per-conversation chat history, current topic context, mentioned entities, and expiry. Capable of creating/deleting/cleaning-up expired sessions and updating conversation context in real time. (Production-grade Redis persistence is deferred to L17.)
- **`BaseChatAdapter` + factory**: An abstract chat API adapter pattern with three concrete implementations — `OpenAIChatAdapter`, `TongyiChatAdapter` (DashScope HTTP API), and `DeepSeekChatAdapter` (OpenAI-compatible). The `create_chat_adapter()` factory instantiates the right adapter by provider name. This enables runtime model switching (`rag.switch_model("deepseek")`) and query-routing strategies (complex→gpt-4o, sensitive→qwen-max, fallback→deepseek-chat).
- **`ConversationalRAG` integrated pipeline**: Wraps all the above into a single class that extends L9's `RAGWithRefusal` rejection/fallback logic. It does: session lookup → referent resolution → retrieval → confidence check → rejection-or-generation → history update. Tracks per-query latency, token estimate, and history depth.
- **History-length vs latency benchmark harness**: A `benchmark_history_lengths()` utility that tests different window sizes and records the tradeoff; empirical data shows TTFT grows linearly with history token count (from ~0.5s at 2 turns to ~3s at 20 turns for GPT-4o-mini).

**Key insight:** Multi-turn dialogue is not about "stuffing chat history into the prompt." Three distinct problems must each be solved: (1) **Context-dependent retrieval** — a standalone query like "多少钱？" returns garbage from the vector DB; you must rewrite it into a self-contained query before retrieval. (2) **Bounded history** — prompt length grows without bound if unmanaged, causing TTFT to degrade linearly and cost to explode. (3) **Model interface diversity** — OpenAI/DashScope/DeepSeek have different message formats, context windows, and error semantics; abstracting them behind a uniform interface is essential for production switching/fallback. The simplest correct architecture that solves all three is: rewrite → retrieve → check → generate, with a sliding window cropping the message list at each turn.

**Why:** After L9 gave the system the ability to honestly refuse, it still couldn't handle the most common user behavior pattern — follow-up questions with implicit references. For the customer-service Q&A mission scenario, single-turn interaction is unrealistic: real users ask "屏幕多大？", then "它支持无线充电吗？" without repeating the product name. Without conversational memory, the system either retrieves wrong context or hallucinates. This lesson plugs that gap before the next topic (Prompt Engineering in L11) which will further reduce hallucination in the generation stage.

**How to apply:**
1. Start with SlidingWindowHistory(window_size=6) — it handles 90% of multi-turn scenarios with zero extra cost
2. Enable LLM-based reference resolution but gate it with pronoun detection (only call LLM when "它"/"该" etc. are present)
3. For Chinese customer-service scenarios, configure TongyiChatAdapter as the primary model for better Chinese comprehension and data compliance
4. Run the history-length benchmark on your actual model to determine the optimal window size — don't guess
5. If latency is critical (> 800ms unacceptable), use rule-based referent resolution and reduce window to 4

**Skill gaps observed:**
- No Redis-based session persistence yet — sessions are lost on process restart (deferred to L17)
- The referent resolution uses gpt-4o-mini which adds 200-400ms per query — for <500ms latency requirements, a lightweight NER model (e.g. BERT-based coreference) would be needed
- No streaming output support — the adapter returns full text, so TTFT includes full-generation time for long answers
- The Tongyi adapter uses raw HTTP rather than the official dashscope SDK (practical for teaching but not production-optimal)
- No automated testing for multi-turn coherence (no metric like "context retention@K") — this is an open research area
- The KeyTurnExtractionHistory's rule-based importance scoring is simplistic (keyword matching) — a learned importance predictor would be more robust

**Next topic:** L11 — Prompt Engineering 在 RAG 中的实践. With conversational memory in place, the next big lever for quality is prompt design: system prompts that reduce hallucination 30-50%, grounding templates, and few-shot examples tailored to the mission scenario.
