# Failure Analysis

## Bottom-5 Worst Questions

### 1. Bao lâu phải đổi mật khẩu một lần?
- **Worst metric:** faithfulness
- **Score:** 0.0
- **Diagnosis:** LLM hallucinating
- **Failure category:** Generation
- **Root cause:** The LLM hallucinates facts not strictly within the retrieved context, or contradicts context.
- **Suggested fix:** Tighten prompt, lower temperature.

### 2. Muốn mua thiết bị trị giá 55 triệu cần ai phê duyệt?
- **Worst metric:** faithfulness
- **Score:** 0.0
- **Diagnosis:** LLM hallucinating
- **Failure category:** Generation
- **Root cause:** Missing strict grounding constraint causing LLM to answer using general knowledge.
- **Suggested fix:** Tighten prompt, lower temperature.

### 3. Nhân viên tạm ứng 15 triệu, sau 20 ngày mới thanh toán. Bị phạt bao nhiêu?
- **Worst metric:** faithfulness
- **Score:** 0.0
- **Diagnosis:** LLM hallucinating
- **Failure category:** Generation
- **Root cause:** The LLM generates a hallucinated penalty or makes assumptions based on missing policy details.
- **Suggested fix:** Tighten prompt, lower temperature.

### 4. Nhân viên thử việc có được hưởng bảo hiểm sức khỏe PVI không?
- **Worst metric:** answer_relevancy
- **Score:** 0.0
- **Diagnosis:** Answer doesn't match question
- **Failure category:** Generation
- **Root cause:** The LLM failed to directly address the core of the question, providing an overly broad or tangential response.
- **Suggested fix:** Improve prompt template.

### 5. Nghỉ phép không lương 20 ngày cần ai phê duyệt?
- **Worst metric:** context_precision
- **Score:** 0.5000
- **Diagnosis:** Too many irrelevant chunks
- **Failure category:** Retrieval
- **Root cause:** The dense retrieval is picking up chunks with general "nghỉ phép" and "phê duyệt" terminology but missing the specific 20-day rule context.
- **Suggested fix:** Add reranking or metadata filter.

## Error Tree
```text
RAG Failure
├── Chunking
├── Retrieval
│   ├── missing evidence
│   ├── irrelevant evidence (Q5)
│   └── wrong document/version
├── Reranking
│   └── relevant candidate ranked too low
├── Augmentation
│   ├── conflicting evidence
│   └── missing multi-hop evidence
└── Generation
    ├── hallucination (Q1, Q2, Q3)
    ├── unsupported numeric claim
    └── irrelevant answer (Q4)
```
