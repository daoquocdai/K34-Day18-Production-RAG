# Individual Reflection — Lab 18

**Tên:** Dao Quoc Dai  
**Module phụ trách:** M1, M2, M3, M4, M5

---

## 1. Đóng góp kỹ thuật

- **Module đã implement:** Đã hoàn thành toàn bộ M1 (Chunking), M2 (Hybrid Search), M3 (Reranking), M4 (Evaluation), M5 (Enrichment).
- **Các hàm/class chính đã viết:** `chunk_semantic`, `chunk_hierarchical`, `BM25Search`, `DenseSearch`, `reciprocal_rank_fusion`, `CrossEncoderReranker`, `evaluate_ragas`, `enrich_chunks`, `_enrich_single_call`. Thay thế mô hình local thành Hugging Face Inference và Pinecone Inference.
- **Số tests pass:** 37/37

## 2. Kiến thức học được

- **Khái niệm mới nhất:** Reciprocal Rank Fusion (RRF) để kết hợp kết quả từ Sparse và Dense Retrieval, Enrichment bằng LLM gộp (1 API call/chunk).
- **Điều bất ngờ nhất:** Tốc độ cải thiện rõ rệt khi chuyển từ local execution các model nặng (BGE-M3) sang Cloud Inference API (Hugging Face / Pinecone) mà vẫn giữ nguyên luồng Pipeline. RAGAS scores rất nhạy cảm với format của câu hỏi.
- **Kết nối với bài giảng:** Bài giảng về Advanced Retrieval (Hybrid Search, Reranking) và Evaluation metrics (RAGAS framework).

## 3. Khó khăn & Cách giải quyết

- **Khó khăn lớn nhất:** Việc chạy model local `BAAI/bge-m3` và `bge-reranker-v2-m3` gây OOM và thời gian chạy lâu, Hugging Face Serverless API không hỗ trợ pairwise scoring cho cross-encoder reranking.
- **Cách giải quyết:** Đổi sang Hugging Face Inference API cho Dense Embedding và Pinecone Inference API cho Cross-Encoder Reranking.
- **Thời gian debug:** ~4 giờ (từ lúc cấu hình M1-M5 cho đến lúc migrate sang remote providers).

## 4. Nếu làm lại

- **Sẽ làm khác điều gì:** Bổ sung metadata filtering ngay từ khâu chunking để cải thiện RAGAS `context_precision` và giảm tải cho reranker.
- **Module nào muốn thử tiếp:** Mở rộng M5 Enrichment để trích xuất Knowledge Graph.

## 5. Tự đánh giá

| Tiêu chí | Tự chấm (1-5) |
|----------|---------------|
| Hiểu bài giảng | 5 |
| Code quality | 5 |
| Teamwork | 5 |
| Problem solving | 5 |
