# Lesson 2: Running Python RAG Demo

The learner wrote and ran a complete ~80-line Python RAG demo with Chroma + OpenAI. They can now: set up a Python environment for RAG, embed documents with OpenAI's text-embedding-3-small, store/query vectors in Chroma, construct a prompt that grounds LLM answers in retrieved evidence, and verify that the model refuses to answer when no relevant document exists.

**Key insight:** The demo proved that writing `DOCS` as a hand-crafted Python list and treating each entry as a pre-chunked document works for a 5-document prototype, but real documents (PDFs, web pages, Word files) require proper parsing and chunking — this is the natural next step.

**Skill gaps observed:**
- The learner hasn't yet dealt with real document formats (PDF, HTML, Markdown files).
- Chunking strategy (size, overlap, boundary awareness) is still a black box.
- They know `temperature=0.0` is good for customer service but haven't explored other generation parameters (top_p, max_tokens, system prompt design).

**Why:** The jump from "understand the concept" to "run working code" is the hardest part for engineers new to LLM APIs and vector databases. This lesson bridges that gap with a concrete artifact they can modify and extend.

**How to apply:** Next lessons should build on this demo by replacing the hand-written DOCS list with real file ingestion (PDF/HTML parsers) and introducing proper text chunking strategies. The learner should keep extending the same `demo.py` script rather than starting from scratch each time.
