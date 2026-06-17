# LLM RAG Enterprise Knowledge Base Resources

## Knowledge

- [Paper: "Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks" - Lewis et al.](https://arxiv.org/abs/2005.11401)
  Foundational RAG paper. Use for: the core idea that a generation model can be grounded by retrieved external memory instead of relying only on model parameters.
- [OpenAI Docs: Vector embeddings](https://platform.openai.com/docs/guides/embeddings)
  Official explanation of embeddings and their search use cases. Use for: understanding how text becomes vectors and why similarity search works.
- [OpenAI API Reference: Create embeddings](https://platform.openai.com/docs/api-reference/embeddings/create)
  Official endpoint reference. Use for: exact embedding request/response shape, model choices, token limits, and output vector format.
- [OpenAI Docs: Text generation](https://developers.openai.com/api/docs/guides/text)
  Official guide for model text generation through the Responses API. Use for: basic LLM API calls and prompt structure.
- [Chroma Docs: Getting Started](https://docs.trychroma.com/docs/overview/getting-started)
  Official Chroma quickstart. Use for: local vector database basics, collections, adding documents, and querying similar documents.
- [Chroma Docs: Adding Data](https://docs.trychroma.com/docs/collections/add-data)
  Official Chroma collection ingestion guide. Use for: document IDs, metadata, and collection behavior.

## Wisdom (Communities)

- [OpenAI Developer Community](https://community.openai.com/)
  Use for: practical API questions, implementation tradeoffs, and error troubleshooting.
- [Chroma Discord](https://discord.gg/MMeYNTmh3x)
  Use for: Chroma setup, retrieval behavior, persistence, and production migration questions.

## Gaps

- Need to add a high-quality source for RAG evaluation metrics such as retrieval recall, groundedness, answer correctness, and citation accuracy.
- Need to add production architecture references for enterprise permissions, tenant isolation, monitoring, and cost controls after the demo stage.
