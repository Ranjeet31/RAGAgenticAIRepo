# MP2 Reflection — Mini-RAG: Ask Your Documents

## 1. What I Built

I implemented an end-to-end Retrieval-Augmented Generation (RAG) pipeline using five Sherlock Holmes story summaries as the knowledge corpus. The objective was to integrate the concepts covered in Weeks 6–9 into a working application that answers questions using retrieved document evidence.

The pipeline covers the complete RAG workflow: loading documents, splitting them into manageable chunks, generating embeddings, storing vectors in Qdrant, retrieving relevant chunks for a question, and generating a grounded answer using OpenAI's `gpt-4o-mini` model.

## 2. Key Concepts Applied

- **Basic RAG (W6):** Combined document retrieval with LLM-based answer generation so that responses are grounded in the supplied corpus rather than relying exclusively on the model's pretrained knowledge.
- **Embeddings and Vector Search (W7):** Used `text-embedding-3-small` to convert document chunks and user questions into vector representations. Stored the document vectors and associated metadata in Qdrant for similarity-based retrieval.
- **Structure-Aware Chunking (W8):** Implemented paragraph-based chunking with section-header detection to preserve useful context and source information while keeping chunks within a target size.

## 3. Challenges and How I Addressed Them

One challenge was managing configuration across Jupyter Notebook and the standalone Python script. I investigated environment-variable loading for API credentials and Qdrant connectivity, highlighting the importance of consistent configuration between execution environments.

Another challenge occurred during ingestion when a large Qdrant upsert request timed out. This demonstrated that successful embedding generation does not guarantee successful vector storage. Processing chunks in smaller batches and configuring an appropriate request timeout are practical ways to make ingestion more reliable.

I also learned the importance of executing notebook cells in the correct order and ensuring that variables and document metadata are initialized within the appropriate function scope.

## 4. Validation and Evaluation

The project provides predefined golden questions for checking retrieval and source matching, alongside an interactive question-answering workflow. Testing questions that require information from multiple Sherlock Holmes stories is particularly useful for evaluating whether the system can retrieve evidence from different documents and synthesize it into a coherent response.

For meaningful evaluation, I would examine whether the retrieved chunks contain the relevant evidence, whether the answer is supported by those chunks, and whether the cited story titles and sections are correct. I would also test questions whose answers are absent from the corpus to check whether the system acknowledges insufficient evidence rather than inventing an answer.

## 5. Key Learnings and Future Improvements

The main learning was that RAG is an integrated system rather than simply an LLM call. Document preparation, chunk boundaries, embedding quality, retrieval relevance, metadata, API configuration, and ingestion reliability all influence the final answer.

As future improvements, I would explore hybrid retrieval using BM25 and vector search, followed by cross-encoder reranking. I would also evaluate different chunk sizes and retrieval values for `k`, and measure retrieval quality, answer grounding, latency, and API cost.

Overall, MP2 helped me connect the individual concepts from W6–W9 into a practical RAG application and better understand the engineering considerations involved in building a reliable document-question-answering system.