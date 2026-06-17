# Lesson 3: Document Parsing and Chunking Strategy

The learner can now parse real document formats (Markdown, PDF via pdfplumber, HTML via BeautifulSoup) and apply semantic + token-aware chunking strategies to produce high-quality retrievable segments. They have a `loader.py` module that replaces the hand-crafted `DOCS` list from Lesson 2, enabling their RAG pipeline to ingest real files from a `docs/` directory.

**Key insight:** Chunking is the most underrated lever in RAG quality. An embedding model's accuracy matters less than whether the right information survives intact inside a chunk. The learner now understands the tradeoff: too small loses context, too large dilutes semantics, no overlap risks cutting answers at chunk boundaries.

**New capabilities:**
- Parse .md, .txt, .pdf, .html files into plain text with format-appropriate libraries
- Apply semantic chunking: paragraph → sentence splitting → token-budgeted merging
- Use tiktoken (cl100k_base) for accurate token counting vs character-based approximation
- Configure chunk_size and overlap as tunable parameters
- Maintain source traceability from original file through chunk_index to final answer

**Skill gaps observed:**
- The learner hasn't yet evaluated retrieval quality quantitatively (Recall@K, MRR, NDCG)
- They're using a single embedding model (text-embedding-3-small) without comparison to alternatives like BGE, Cohere, or Jina
- No re-ranking step — they rely solely on vector similarity for ordering
- OCR for scanned PDFs is mentioned but not practiced
- Metadata filtering (by topic, date, author) is not yet used in retrieval

**Why:** Moving from hand-crafted demo data to real file ingestion is the threshold where RAG stops being a toy and starts being an enterprise tool. Most enterprise knowledge lives in PDFs and web pages. Without proper parsing and chunking, the retrieval quality collapses.

**How to apply:** The next lesson should introduce retrieval evaluation metrics and re-ranking — this is the natural next step because the learner will immediately notice that some queries return wrong or irrelevant chunks. They need tooling to measure "how bad" and techniques to improve it.
