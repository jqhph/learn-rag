# Mission: LLM RAG Enterprise Knowledge Base

## Why
The goal is to build a usable customer-service Q&A RAG demo from zero, while learning the core concepts and basic usage of LLM APIs, embeddings, and vector databases. The practical outcome is being able to ingest product/support documents and answer customer questions with grounded, traceable responses.

## Success looks like
- Build a local Python demo that ingests a small customer-service knowledge base.
- Query the knowledge base through semantic retrieval, not only keyword matching.
- Generate answers that cite retrieved source snippets.
- Explain the basic RAG loop: chunk, embed, store, retrieve, prompt, answer, evaluate.
- Know what must change before a demo can become an enterprise-ready knowledge base.

## Constraints
- Learner knows Python, Go, and TypeScript.
- Learner is not yet familiar with vector databases or LLM APIs.
- First stage should prioritize a working demo, core concepts, and basic usage.
- Customer-service Q&A is the primary scenario.

## Out of scope
- Full enterprise deployment, SSO, RBAC, audit logging, and observability in the first stage.
- Fine-tuning models.
- Multi-agent workflows beyond the simplest RAG pipeline.
