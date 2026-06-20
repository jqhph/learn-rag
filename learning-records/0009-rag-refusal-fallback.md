# Lesson 9: RAG 拒绝策略与兜底机制

The learner now has a complete refusal & fallback system for their RAG pipeline — the second lesson in Module 2 ("quality foundation").

**New capabilities:**
- **Retrieval confidence assessment**: Three-signal approach combining retrieval scores, Cross-Encoder relevance verification, and generation logprobs to decide whether the system should answer or refuse
- **Confidence decision logic**: A two-stage pipeline (`confidence.py`) — first check score thresholds, then Cross-Encoder verify, producing one of three decisions: answer / partial / refuse with specific refusal types (no_knowledge, low_confidence, partial_knowledge, vague_query)
- **Three refusal response templates**: Transparent refusal ("my knowledge base doesn't have this"), partial information with clear boundaries, and guided refusal asking for clarification — all designed with psychological principles (attribution transparency, alternative paths, empathetic tone)
- **Multi-layer fallback chain**: 4 tiers with escalating cost — ① local KB retrieval + confidence → ② query rewrite retry (Multi-Query from L5) → ③ web search API → ④ human ticket transfer, each tier only activated when the cheaper ones fail
- **Knowledge base coverage monitoring**: `CoverageMonitor` that logs every query's outcome, generates per-category answer/refusal/human-transfer rates, identifies top "blind spot" queries (refused ≥2 times), and drives a data-driven KB iteration loop
- **Integrated `RAGWithRefusal` pipeline**: A refusable-aware RAG class that threads retrieval → confidence → refusal/answer through all fallback tiers, with configurable switches for web search and human fallback

**Key insight:** The most important realization in this lesson is that **not answering is a feature, not a bug**. The refusal–hallucination tradeoff is fundamental: a system that tries to answer everything will inevitably hallucinate; a system that refuses too much frustrates users. The right strategy depends on the scenario (medical/legal: refuse aggressively; product rec: answer more freely). The lesson pushes back against three common fallacies: (1) "A good RAG system should answer all questions" — no, it should know its limits; (2) "Low retrieval score means bad retrieval" — it may mean the KB genuinely doesn't cover the topic, which is useful signal; (3) "Human fallback is a failure" — it's the most honest and responsible thing to do when AI can't handle it.

**Why:** After L1-L7 built a full RAG pipeline and L8 provided the tech stack map, the learner's system could retrieve and generate — but it had no guard against hallucination on out-of-KB queries. For the customer-service Q&A mission scenario, this is the single most critical quality safeguard: one hallucination can destroy user trust that took 100 correct answers to build. This lesson provides the defensive layer before moving forward with conversational features (L10) and prompt engineering (L11).

**How to apply:**
1. Calibrate your score threshold by sampling 200 queries and finding the F1-maximizing threshold
2. Implement the three refusal templates with your brand's tone of voice
3. Set up the CoverageMonitor in your production system and review the first week's report
4. For the human fallback tier, integrate with your actual ticketing system (Zendesk/DingTalk/Feishu)
5. Run the A/B test from Exercise 3 to quantify the refusal strategy's impact on Faithfulness

**Skill gaps observed:**
- No experience with production Cross-Encoder deployment (the lesson uses LLM as a fallback — real Cross-Encoder models like BGE Re-Ranker are 10-100x faster but weren't code-exercised)
- No integration with real ticketing systems (Zendesk, DingTalk robot, Feishu webhook)
- No experience with web search API setup (Serper, Tavily, Bing — the lesson only provides placeholder stubs)
- No production-grade logging infrastructure (the CoverageMonitor uses JSONL files — production would need Elasticsearch/ClickHouse)
- No real calibration data — the threshold values (0.4, 0.65, 0.5) are experience-based estimates

**Next topic:** L10 — 对话式 RAG 与多轮对话管理 (Conversational RAG & Multi-turn Dialogue). The refusal system is in place; now the learner needs to handle chat history, referent resolution, and session state management.
