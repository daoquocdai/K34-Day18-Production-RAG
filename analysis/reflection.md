# Reflection

## Part 1 — Lecture Mapping

* **M1 Semantic / advanced chunking:** Implemented in `chunk_semantic` and `chunk_hierarchical` inside `src/m1_chunking.py`. The strategy segments Vietnamese paragraphs and preserves contextual metadata (e.g. section headers).
* **M2 BM25 + Dense + RRF:** Implemented via `BM25Search`, `DenseSearch`, and `reciprocal_rank_fusion` in `src/m2_search.py`.
* **M3 Cross-encoder reranking:** Implemented in `CrossEncoderReranker.rerank` within `src/m3_rerank.py`, taking Top-20 retrieved chunks and reordering them based on semantic alignment.
* **M4 RAGAS evaluation:** Implemented using `evaluate_ragas` and `failure_analysis` in `src/m4_eval.py`, running against 20 benchmark questions to measure faithfulness, answer relevancy, context precision, and context recall.
* **M5 Enrichment:** Implemented via `enrich_chunks` in `src/m5_enrichment.py`, injecting document metadata, summaries, and hypothetical QA directly into chunks prior to indexing.

## Part 2 — Difficulties & Solutions

* **Local heavy BGE model execution was unsuitable:** The local `SentenceTransformer("BAAI/bge-m3")` caused out-of-memory errors and extremely long load times.
  * **Solution:** Moved embedding extraction to Hugging Face Inference (`hf-inference` via `InferenceClient`).
* **HF serverless did not provide the required query + documents[] rerank semantics:** Hugging Face's serverless endpoints do not support pairwise scoring for `bge-reranker-v2-m3` directly.
  * **Solution:** Replaced local `CrossEncoder` with Pinecone Inference using `pinecone.Pinecone` and `pc.inference.rerank`, which natively supports cross-encoder reranking over a list of documents.

## Part 3 — Project Action Plan

### Hiện tại
- RAG pipeline hiện tại: Sử dụng HF `BAAI/bge-m3` cho embedding và Pinecone `bge-reranker-v2-m3` cho reranking, kết hợp Hybrid Search (BM25 + Dense + RRF). Pipeline có độ chính xác cao nhưng LLM thi thoảng vẫn hallucinate.
- Known issues: LLM sinh câu trả lời bịa đặt (hallucination) cho một số câu hỏi thiếu context chặt chẽ hoặc trả lời chung chung khi policy bị thiếu chi tiết.

### Plan áp dụng
1. [x] Chunking strategy: Thêm bộ lọc metadata filter.
2. [x] Search: Tinh chỉnh trọng số RRF giữa BM25 và Dense.
3. [x] Reranking: Dùng Pinecone Inference online để giảm tải phần cứng nội bộ.
4. [x] Evaluation: Thêm bộ test cho đa dạng multi-hop reasoning.
5. [x] Enrichment: Hoàn thiện tính năng sinh hyQA cho các văn bản chính sách khó hiểu.

### Timeline
- Tuần 1: Hoàn thiện Hybrid Retrieval và gán Pinecone Reranker.
- Tuần 2: Fine-tune generation prompt để hạn chế LLM hallucination dựa vào failure analysis.
