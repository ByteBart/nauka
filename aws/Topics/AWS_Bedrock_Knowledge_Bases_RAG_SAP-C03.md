# Amazon Bedrock Knowledge Bases & RAG — study book (SAP-C03)

> **Scope:** what RAG is, how the **retrieval (“RAG search”)** part actually works, and how to use it on AWS with **Amazon Bedrock Knowledge Bases** — managed vs customer-managed, data sources, parsing, chunking, embeddings, vector stores, retrieval APIs, reranking, citations, guardrails, multi-tenancy, cost, evaluation, lab, traps and practice questions.
> **Knowledge state:** November 2026, verified against current Bedrock docs (links in §16).
> **Exam relevance:** SAP-C03 Domain 1 Task 1.2 (“Integrate AI and ML services into architectures” — skill **1.2.4 Design RAG architectures**), plus Domain 2 (responsible AI), Domain 3 (AI cost) and Domain 5 (evaluations/monitoring).

---

## 0. TL;DR

- **RAG (Retrieval-Augmented Generation)** = retrieve relevant snippets from *your* data, put them into the prompt as context, and let the foundation model answer from them. You don't retrain the model; you **fetch + augment** at query time.
- **“RAG search”** is the retrieval stage: embed the query → search a vector (or hybrid) index → filter by metadata → optionally **rerank** → take top-N chunks → build the prompt.
- **Amazon Bedrock Knowledge Bases** is the managed RAG service: it connects to data sources (S3, SharePoint, Confluence, Google Drive, OneDrive, Web Crawler…), parses and **chunks** documents, creates **embeddings**, writes them into a **vector store**, and exposes **`Retrieve`** (chunks) and **`RetrieveAndGenerate`** (answer + citations) APIs.
- **Two flavours:** **Managed Knowledge Base** (AWS manages ingestion/index/storage/retrieval — recommended; adds multimodal ingestion, connectors, document-level ACL filtering, agentic retrieval, AgentCore Observability) and **Customer-managed Knowledge Base** (you own the vector store — OpenSearch Serverless, Aurora, Neptune — and all pipeline settings).
- **Why RAG:** grounded answers, fresh data without retraining, **citations**, **per-user access control**, and far cheaper than fine-tuning for knowledge-heavy use cases.
- **Key levers:** chunking strategy, embedding model (float vs binary), vector store, top-k, metadata filters, **reranker**, guardrails, and the trade-off between context size (cost/latency) and answer quality.

---

## 1. What RAG actually is

A foundation model knows only what it was trained on. RAG adds a **retrieval step** so the model can answer from documents it has never seen:

```
User question
   │
   ├─► (1) RETRIEVE: embed query → search index → top-N relevant chunks (+ metadata, rerank)
   │
   ├─► (2) AUGMENT: prompt = instructions + retrieved chunks + question
   │
   └─► (3) GENERATE: foundation model answers using the chunks, with citations
```

**“RAG search”** is step 1 — the part people usually mean when they say “RAG search” or “RAG retrieval”. It is a *semantic search* system with optional keyword, metadata and reranking layers, not a database lookup.

### Why not just fine-tune or paste everything into the prompt?

| Approach | What it gives | Cost/limits | Use when |
|---|---|---|---|
| **RAG** | fresh knowledge, citations, per-user access control, no retraining | retrieval quality + context tokens + vector infra | knowledge assistants, support, compliance Q&A, internal docs |
| **Fine-tuning** | style/format/behaviour, domain tone, smaller prompts | training cost, data prep, no automatic freshness, hallucination still possible | consistent tone/format, task specialisation |
| **Long-context prompt stuffing** | simplest | token cost per request, context-window limits, no access control per chunk | small, static corpora |
| **Classic search (Kendra/OpenSearch)** | document links | no synthesis | “find the doc”, not “answer the question” |

---

## 2. The two pipelines of every RAG system

### 2.1 Ingestion (offline, batch/scheduled)

```
Data source (S3, SharePoint, Confluence, Google Drive, OneDrive, web crawler)
   → parse (text, tables, figures, PDFs, Office, images, audio/video)
   → chunk (split into retrievable pieces)
   → embed (embedding model → vectors)
   → index (vector store + metadata + mapping to source location)
   → sync (re-run when documents change; incremental where supported)
```

### 2.2 Retrieval + generation (online, per query)

```
Query
   → (optional) query rewrite/decomposition
   → embed query with the SAME embedding model
   → vector search (cosine/dot/L2 ANN) + metadata filter
   → top-K candidates
   → (optional) rerank with a reranker model
   → top-N chunks inserted into the prompt
   → foundation model generates an answer with citations
   → (optional) guardrails (PII, denied topics, grounding checks)
```

**The cardinal rule:** the **query embedding model must match the document embedding model**. Changing one requires re-embedding the corpus.

---

## 3. The retrieval (“RAG search”) stage in detail

### 3.1 Embeddings

- An embedding model turns text (or image/audio) into a **vector** — a list of numbers representing meaning. Similar meaning → nearby vectors.
- **Float32 vectors** (default) are precise; **binary vectors** use 1 bit/dimension — far cheaper to store and faster to search, slightly less precise. If you choose a binary embedding model, the vector store must also support binary vectors.
- Multiple embedding models are available in Bedrock (e.g. Amazon Titan Text Embeddings, Cohere Embed); dimensionality and language support differ — pick once and stay consistent.
- **Multimodal embeddings** (e.g. Nova multimodal) let you index images/audio/video and search with images or mixed text+image queries.

### 3.2 Vector search

- Similarity metrics: **cosine**, **dot product**, **Euclidean (L2)** — the index must be configured to match.
- **Approximate nearest neighbour (ANN)** indexes trade a tiny recall loss for speed at scale (HNSW-style graphs inside OpenSearch/Aurora pgvector/Neptune).
- **K (top-K)** controls how many chunks are fetched. Too small → missed context; too large → noise, token cost, latency. Typical starting point: K = 5–10, then rerank down to 3–5.
- **Hybrid search** combines keyword (BM25-like) and semantic scores — better for exact identifiers, product codes, names. Availability depends on the vector store and retrieval mode (customer-managed with OpenSearch supports hybrid scoring; verify current Managed KB capabilities).

### 3.3 Metadata and filtering

- Each chunk carries metadata (source URI, page number, document ID, tenant, classification, ACLs).
- **Filterable metadata** narrows the search space *before* ANN (e.g. `tenant_id = "acme"`, `doc_type = "policy"`); **non-filterable metadata** is returned with results but cannot be used to filter.
- **Document-level ACL filtering** (Managed KB): permissions from the source system can be enforced **at retrieval time** — users only get chunks they are allowed to see. Not available for the Web Crawler connector.
- Chunking affects citation precision: with **no chunking**, page-number metadata/filtering (`x-amz-bedrock-kb-document-page-number`) is unavailable.

### 3.4 Reranking

- A **reranker model** (e.g. Cohere Rerank via Bedrock `Rerank` API) re-scores the candidate chunks against the query and reorders them.
- Benefits: **fewer, more relevant** chunks → better answer, **lower generation cost and latency** (fewer context tokens).
- In Knowledge Bases, pass a reranker on `Retrieve`/`RetrieveAndGenerate`; **reranked results override the default ranking**.
- Reranking is **text-only**; it is an extra cost per request (based on documents + tokens processed).

### 3.5 Query-side improvements

- **Query decomposition / multi-hop (Agentic Retrieval, Managed KB):** breaks complex questions into sub-queries, retrieves iteratively **across multiple knowledge bases**, and checks whether retrieved context is sufficient.
- **Query rewriting/expansion:** rephrase the user question for better recall (often implemented in the application or an agent step).
- **Metadata-aware queries:** pass user/tenant/classification filters at retrieval time.

---

## 4. Amazon Bedrock Knowledge Bases — the managed service

### 4.1 Two types

| | **Managed Knowledge Base** (recommended) | **Customer-managed Knowledge Base** |
|---|---|---|
| Who runs ingestion/index/storage/retrieval | **AWS** | **you** |
| Connectors | S3, SharePoint, Confluence, Google Drive, OneDrive, Web Crawler | S3 (and your own pipelines) |
| Parsing | **Smart Parsing** auto-selects per document type (PDF/PPTX/DOCX, embedded visuals, audio, video, scans) | you configure parsers (foundation-model parser, BDA parser, custom) |
| Chunking | managed, with options | you choose all settings |
| Embeddings + reranking + reasoning models | service-managed by default, can bring your own | you choose |
| Vector store | managed | **you choose**: OpenSearch Serverless, Aurora (pgvector), Neptune Analytics, S3 Vectors, third-party (Pinecone/Redis/MongoDB Atlas where supported) |
| Document-level ACL filtering | ✅ (except Web Crawler) | ❌ |
| Agentic Retrieval (multi-hop, multi-KB) | ✅ | ❌ |
| AgentCore Gateway integration (MCP tool) | ✅ native | ❌ (wrap manually) |
| Observability | native AgentCore Observability (retrieval/agent traces, per-KB metrics) | you build it |
| Best for | fastest path, enterprise connectors, access control | existing data platform, custom pipeline, portability, cost control |

### 4.2 Data sources and sync

- Each KB has one or more **data sources**; ingestion runs as **sync/ingestion jobs** that parse → chunk → embed → index.
- Incremental updates depend on the connector; for S3, changes are detected on sync (full or incremental depending on configuration).
- **Deletion handling:** documents removed at the source must also be removed from the index — verify the connector's behaviour and re-sync strategy.
- Large corpora: sync jobs are asynchronous; monitor job status; chunk count and embedding cost scale with volume.

### 4.3 Parsing

- **Foundation-model parser:** extracts text/tables from PDFs and Office files.
- **Bedrock Data Automation (BDA) parser:** richer extraction, multimodal content → text (transcripts, scene summaries).
- **Smart Parsing** (Managed KB): automatic per-document-type strategy, including scanned docs and embedded visuals.
- Tables/figures: when a page or table is **≤ 10,000 characters**, Bedrock can attach the broader page/table context to a chunk and return it instead of the matching chunk; results may then be fewer than requested.

### 4.4 Chunking strategies

| Strategy | How it works | Good for | Watch out |
|---|---|---|---|
| **Default** | ~**300 tokens**, respects sentence boundaries | quick start | may split long arguments |
| **Fixed-size** | token limit + **overlap %** | predictable sizes, dense docs | overlap adds storage/embedding cost |
| **None** | whole document = one chunk | short docs, custom pre-splitting | loses page-number citation/filtering; huge chunks hurt retrieval |
| **Hierarchical** | parent + child chunks; retrieve children, return **parent** context | long docs where small chunks lack context | more complex; not recommended with S3 Vectors (metadata size); fewer results than requested |
| **Semantic** | splits where sentence dissimilarity crosses a percentile threshold (max tokens, buffer, breakpoint threshold) | narrative/prose where topic shifts matter | **extra foundation-model cost** during ingestion |
| **Multimodal** | chunking at embedding model level: audio/video 1–30 s (default 5 s); BDA parser converts to text first | media-heavy corpora | text chunking settings don't apply to audio/video |

Practical guidance: start with default or fixed-size (300–800 tokens, 10–20 % overlap), measure retrieval quality, then try hierarchical or semantic. **Chunk size is a trade-off**: small chunks = precise matching but fragmented context; large chunks = rich context but noisy similarity and more tokens.

### 4.5 Vector stores

Customer-managed KB options (verify supported list per Region):
- **Amazon OpenSearch Serverless** — default-style vector engine, hybrid search, metadata filtering, scales.
- **Amazon Aurora PostgreSQL (pgvector)** — reuse existing relational platform; good for moderate scale.
- **Amazon Neptune Analytics** — graph + vector; supports **GraphRAG** (knowledge base built from graphs).
- **Amazon S3 Vectors** — low-cost vector storage (watch metadata size limits; chunking caveats above).
- Third-party: Pinecone, Redis Enterprise Cloud, MongoDB Atlas (where supported).

Choice drivers: existing platform, scale/QPS, hybrid support, cost, metadata filtering, VPC/network constraints, Region availability.

### 4.6 Retrieval APIs

| API | Returns | Use |
|---|---|---|
| **`Retrieve`** | ranked text chunks + metadata + citations (S3 URI/page) | custom prompt building, agents, your own generation |
| **`RetrieveAndGenerate`** | generated answer + **citations** + source chunks | quick RAG answers without writing prompt logic |

Both accept retrieval configuration (top-K, filters, reranker) and the generation model. For full control (system prompt, tools, conversation), use `Retrieve` + `Converse`/`InvokeModel` with Guardrails.

### 4.7 Beyond plain vector RAG

- **Structured-data knowledge base:** connect to a structured store (e.g. Redshift, S3 tables); Bedrock generates SQL from the question and returns data-grounded answers — good for “how many/how much” questions.
- **Kendra GenAI index KB:** use an Amazon Kendra index (enterprise search, connectors, relevance tuning) as the retriever behind the KB.
- **GraphRAG with Neptune Analytics:** introduce relationships between entities for multi-hop questions.
- **Chat with a document without a knowledge base:** ad-hoc analysis of a single document (no ingestion pipeline) — convenient, not a scalable corpus solution.

---

## 5. Implementation patterns

| Pattern | When | Sketch |
|---|---|---|
| **Managed KB** (default) | fastest path, enterprise connectors, per-user permissions, minimal ops | Create KB → connect data source → sync → app calls `RetrieveAndGenerate` (or `Retrieve` + `Converse`) |
| **Customer-managed KB** | existing OpenSearch/Aurora/Neptune, custom parsing/chunking, data-residency or cost constraints | Your vector store + KB pointing to it; you control ingestion settings |
| **Fully custom RAG** (no KB service) | non-standard pipeline, portability, fine-grained control | S3 → Glue/Lambda → embedding API → vector DB → custom retrieval → prompt → model |
| **Agent + KB** | multi-step tasks needing tools + knowledge | Bedrock Agents with a knowledge base association, or **AgentCore**: Gateway exposes tools/MCP, KBs as tools, Memory for context, Identity for auth, Policy for tool-call guardrails, Observability for traces |
| **RAG in a pipeline** | batch enrichment/offline answers | Batch inference + retrieval over curated sets; evaluate before publish |

Agent/RAG interplay on SAP-C03: the KB is **a tool the agent can call**; the agent decides *when* to retrieve and can do multi-hop reasoning (Managed KB agentic retrieval), while **AgentCore Identity/Policy** controls what the agent may access and do.

---

## 6. Retrieval quality and evaluation

**Improve quality in this order:**
1. **Parsing** — garbage in, garbage out (scanned PDFs need OCR-capable parsing).
2. **Chunking** — match document structure; try hierarchical for long documents.
3. **Embedding model** — language/domain fit; don’t mix models between ingestion and query.
4. **Top-K + reranker** — over-fetch (e.g. 20) then rerank to 3–5.
5. **Metadata filters** — cut the search space and enforce access control.
6. **Query handling** — rewriting, decomposition for multi-hop, conversation-aware queries.

**Evaluation metrics (Bedrock Evaluations / your golden set):** retrieval relevance/precision, **groundedness** (answer supported by retrieved context), answer correctness vs reference, citation accuracy, latency, token/cost per query. Build a **golden question set** (50–200 Q/A pairs) and run it as a **pipeline gate** for prompt/chunking/model changes (D5 skill 5.2.4).

**Monitoring (production):** retrieval hit rate, empty-result rate, top-K score distribution, answer latency P50/P95, tokens per query, guardrail interventions, user feedback. Managed KB adds retrieval traces via AgentCore Observability.

---

## 7. Security, access control and multi-tenancy

- **IAM & data source roles:** the KB needs permissions to read sources (S3 bucket policy, KMS decrypt for SSE-KMS objects) and to use the embedding/rerank/generation models. Users need `bedrock:Retrieve`/`RetrieveAndGenerate` on the KB.
- **Encryption:** S3 (SSE-KMS), vector store encryption (KMS), model invocation logging destination encryption.
- **Tenant isolation:** three patterns — (a) **KB per tenant** (strong isolation, more overhead), (b) **shared KB + metadata filter on tenant ID** at retrieval (cheapest, must be enforced server-side, never trust client input), (c) **separate vector indexes/collections per tenant** (middle ground). Applied consistently to ingestion metadata.
- **Per-user permissions:** Managed KB document-level ACL filtering from source systems; otherwise pre-filter with metadata derived from an authoritative identity/entitlement service.
- **Prompt-injection defence:** retrieved text is **untrusted data** — keep instructions separate, validate outputs, use Guardrails (denied topics, contextual grounding, PII redaction), least-privilege tools for agents.
- **Data residency:** choose Region for KB + vector store + source bucket; embedding/rerank/generation calls happen in-Region (or via cross-Region inference profiles — a residency decision).
- **Audit:** model invocation logging, CloudTrail, retrieval traces, Guardrail intervention logs.

---

## 8. Cost and performance

| Cost centre | What drives it | Levers |
|---|---|---|
| Ingestion | parsing (esp. BDA/FM parser), embedding tokens, storage | parse once, incremental syncs, smaller corpus, binary embeddings |
| Vector storage | dimensions × items × indexes | binary vectors, S3 Vectors, prune stale chunks |
| Retrieval | per-query embedding + vector store reads | caching, keep K reasonable, batch offline queries |
| Reranking | documents + tokens per query | rerank once over a candidate set, not per chunk |
| Generation | **input tokens** (context!) + output tokens | fewer/smaller chunks after rerank, prompt caching, smaller model for simple Q&A, provisioned throughput for steady volume |
| Operations | observability, guardrails | tune sampling/logging, retention |

**Sizing intuition:** RAG cost scales with **context per query**, not with corpus size (corpus affects ingestion/storage). Rerank-then-reduce context is usually the biggest per-query saving.

---

## 9. SAP-C03 mapping — how this shows up in questions

| Domain / skill | The question looks like | Answer pattern |
|---|---|---|
| **1.2.4 RAG architectures** | “answers must be grounded in 500k internal docs, with citations and freshness” | Bedrock Knowledge Bases + managed retrieval |
| **1.2.4 ingestion/retrieval** | “what governs retrieval relevance?” | chunking + embedding consistency + top-K + reranker + metadata |
| **1.2.1 GenAI integration (multi-account)** | “central KB, multiple app accounts” | central KB account + IAM/RAM-style sharing via KB policies; or per-tenant KBs |
| **2.5 Responsible AI** | “prevent PII leakage / off-topic answers” | Guardrails (PII redaction, denied topics) + grounding checks |
| **3.1.4 / 3.2.5 AI cost** | “reduce cost per answer” | rerank to shrink context, cache, batch, cheaper model, binary embeddings, provisioned throughput |
| **5.2.4 Evaluations** | “validate model/app changes before release” | Bedrock Evaluations golden set gate in CI/CD |
| **5.3.5 AI observability** | “monitor latency/drift/quality” | retrieval traces, token metrics, evaluation baselines |

---

## 10. Exam traps

1. **“RAG retrains the model.”** No — retrieval + prompt augmentation; fine-tuning is separate.
2. **“You can use a different embedding model for queries and documents.”** No — same model, or similarity is meaningless.
3. **“Bigger chunks are always better.”** No — noise and token cost; tune and measure.
4. **“No chunking is fine for PDFs.”** It breaks page-number citations/filtering and hurts retrieval on long docs.
5. **“Reranking is just sorting.”** It is a model-scored relevance pass; it overrides vector-similarity ranking and lets you cut context.
6. **“Metadata filtering is for performance only.”** It is also an **access-control mechanism** (tenant/ACL) — enforce server-side.
7. **“Managed vs customer-managed KB are the same feature set.”** Managed adds connectors, smart parsing, ACL filtering, agentic retrieval, AgentCore integration.
8. **“Binary embeddings are always worse.”** Cheaper/faster with a small precision loss — a valid cost lever when supported end-to-end.
9. **“RAG guarantees truthful answers.”** It grounds them; hallucination is reduced, not eliminated — evaluate groundedness and use guardrails.
10. **“RetrieveAndGenerate is the only API.”** `Retrieve` gives you chunks for custom prompts/agents; `RetrieveAndGenerate` returns an answer + citations.
11. **“Vector store choice is irrelevant.”** It decides hybrid support, filtering, scale, cost, VPC/Region constraints.
12. **“Semantic chunking is free.”** It calls a foundation model during ingestion and costs extra.
13. **“Hierarchical chunking works everywhere.”** Not recommended with S3 Vectors (metadata size limits); result counts can be lower than requested.
14. **“A KB automatically respects source permissions.”** Only with Managed KB document-level ACL filtering (and not for Web Crawler); otherwise you must implement entitlement filtering.
15. **“More context always improves the answer.”** Beyond a point it adds cost, latency and distraction; rerank and cap.

---

## 11. Practice questions

**Q1.** A company wants grounded answers from 500k internal PDFs with citations and no model retraining. What should you use?
A. Fine-tune a foundation model on the PDFs
B. Amazon Bedrock Knowledge Bases with `RetrieveAndGenerate`
C. Paste the PDFs into every prompt
D. Amazon Kendra only

**Q2.** Which component has the biggest impact on retrieval relevance when documents are long and multi-topic?
A. Model temperature
B. Chunking strategy (and hierarchical/semantic options)
C. Output token limit
D. Provisioned throughput

**Q3.** Queries must never return another tenant's chunks. Which is the safest design?
A. Filter client-side after retrieval
B. Server-side metadata filter on tenant ID at retrieval (or a KB per tenant), with tenant metadata written at ingestion
C. Larger top-K
D. Reranker with tenant names

**Q4.** Retrieval returns many irrelevant chunks and generation cost is high. What improves both relevance and cost?
A. Increase top-K
B. Add a reranker and reduce the chunks passed to generation
C. Use a bigger generation model
D. Increase chunk overlap

**Q5.** What is the difference between `Retrieve` and `RetrieveAndGenerate`?
A. No difference
B. Retrieve returns ranked chunks + metadata; RetrieveAndGenerate also generates an answer with citations
C. Retrieve generates answers; RetrieveAndGenerate returns chunks only
D. Both only work with OpenSearch

**Q6.** Which knowledge base type gives you document-level ACL filtering and agentic retrieval?
A. Customer-managed KB
B. Managed Knowledge Base
C. Custom RAG on OpenSearch
D. Kendra-only KB

**Q7.** You switch to a new embedding model with better multilingual quality. What must happen to existing data?
A. Nothing
B. Re-embed/re-index the whole corpus with the new model (query and documents must match)
C. Only re-embed queries
D. Restart the vector store

**Q8.** A team wants exact-match search for SKU codes plus semantic search. Which capability helps?
A. Hybrid search in the vector store
B. Higher temperature
C. Binary embeddings only
D. Longer chunks

**Q9.** Which chunking strategy is least appropriate with an S3 Vectors store?
A. Fixed-size
B. Hierarchical (metadata size limits)
C. None
D. Default

**Q10.** What is the primary purpose of Bedrock Guardrails in a RAG app?
A. Replace retrieval
B. Filter/validate inputs and outputs (PII, denied topics, grounding) around the RAG flow
C. Train the embedding model
D. Reduce vector dimensions

**Q11.** You need per-query cost reduction for a high-volume Q&A app. Which combination is best?
A. Rerank then reduce context chunks + cache frequent answers + smaller model for simple questions
B. Increase top-K and model size
C. Disable reranking
D. Re-index nightly with larger chunks

**Q12.** What is the correct way to evaluate a RAG change (new chunking/prompt) before release?
A. Ship and watch user complaints
B. Run a golden question set through Bedrock Evaluations (groundedness/correctness/citation) as a pipeline gate
C. Increase top-K and hope
D. Compare model names

### Answers

**A1 — B.** Bedrock KB = managed RAG with citations; no retraining.
**A2 — B.** Chunking shapes what can be retrieved; hierarchical/semantic fit long multi-topic documents.
**A3 — B.** Enforce tenant isolation server-side via metadata written at ingestion (or separate KBs/indexes); never trust client-side filtering.
**A4 — B.** Reranker over-fetches then reduces context → better relevance and lower generation tokens.
**A5 — B.** `Retrieve` returns chunks; `RetrieveAndGenerate` returns a generated answer + citations.
**A6 — B.** Managed KB: document-level ACL filtering (except Web Crawler) and agentic retrieval.
**A7 — B.** Re-embed everything — query and document embeddings must come from the same model.
**A8 — A.** Hybrid search combines lexical and semantic scoring.
**A9 — B.** Hierarchical chunking with S3 Vectors risks metadata size limits (not recommended).
**A10 — B.** Guardrails filter/validate inputs and outputs and can enforce grounding-related checks; they don't replace retrieval.
**A11 — A.** Rerank + smaller context + caching + right-sized model is the standard cost stack.
**A12 — B.** Golden-set evaluation as a pipeline gate with Bedrock Evaluations.

---

## 12. Hands-on lab (console/API outline)

1. Create an S3 bucket; upload 20–50 PDFs/MD files with mixed topics + metadata files.
2. **Bedrock → Knowledge Bases → Create** → choose **Managed** (or customer-managed with OpenSearch Serverless).
3. Data source: S3 prefix; set sync scope; note IAM role and KMS permissions.
4. Parsing: default/Smart Parsing; inspect extracted text quality for one scanned PDF.
5. Chunking: start **default**; then compare **fixed-size (512 tokens, 20 % overlap)** and **hierarchical**.
6. Sync/ingest; check chunk counts and embedding cost estimates.
7. Test in console: ask a question; inspect **retrieved chunks + citations**; note empty/missed cases.
8. Enable a **reranker**; compare top-K=20 → rerank → 4 chunks vs top-K=4 directly.
9. Add a **metadata filter** (e.g. `doc_type`) and verify results narrow.
10. Add **Guardrails** (PII redaction + denied topic) and re-test.
11. Call `RetrieveAndGenerate` from Lambda/CLI; log tokens/latency; then `Retrieve` + `Converse` with your own prompt.
12. Build a 30-question **golden set**; run **Bedrock Evaluations**; record groundedness/correctness; use it before/after changing chunk size or model.
13. (Optional) Wire the KB as a tool into an agent (Bedrock Agents or AgentCore Gateway) and watch the traces.

---

## 13. Rapid recall

| Fact | Value |
|---|---|
| RAG stages | retrieve → augment → generate |
| “RAG search” | embed query → vector search → (metadata filter) → (rerank) → top-N chunks |
| Embedding rule | query and documents must use the **same** embedding model |
| Default chunking | ~**300 tokens**, sentence boundaries |
| Fixed-size chunking | max tokens + overlap % |
| Hierarchical | retrieve child, return **parent** context |
| Semantic chunking | FM-based; extra ingestion cost |
| No chunking | whole document; loses page-number citation/filter |
| Reranker | reorders candidates; over-fetch then cut context; text-only |
| APIs | `Retrieve` (chunks) vs `RetrieveAndGenerate` (answer + citations) |
| Managed KB extras | connectors, smart parsing, ACL filtering, agentic retrieval, AgentCore integration |
| Vector stores | OpenSearch Serverless, Aurora pgvector, Neptune Analytics, S3 Vectors, third-party |
| Tenant isolation | metadata filter server-side, per-tenant index, or KB per tenant |
| Evaluation | golden set + Bedrock Evaluations — groundedness, correctness, citation accuracy |

---

## 14. Glossary

| Term | Meaning |
|---|---|
| **RAG** | Retrieval-Augmented Generation — retrieve context, then generate |
| **RAG search / retrieval** | the vector/hybrid search stage that finds relevant chunks |
| **Chunk** | a retrievable piece of a document |
| **Embedding** | numeric vector representation of text/media meaning |
| **Vector store / index** | database of embeddings with metadata and ANN search |
| **ANN / HNSW** | approximate nearest-neighbour search structure |
| **Top-K** | number of candidates retrieved |
| **Hybrid search** | keyword + semantic scoring combined |
| **Reranker** | model that re-scores retrieved candidates for relevance |
| **Metadata filter** | pre-filter on chunk attributes (tenant, type, ACL) |
| **Knowledge Base** | Bedrock's managed RAG resource (data sources + parsing + chunking + embeddings + vector store + retrieval APIs) |
| **Managed KB** | AWS-managed ingestion/index/retrieval with connectors and ACL filtering |
| **Customer-managed KB** | KB where you own the vector store and pipeline settings |
| **Agentic retrieval** | multi-hop query decomposition across KBs (Managed KB) |
| **GraphRAG** | relationship/ graph-backed retrieval (Neptune Analytics) |
| **Golden set** | curated Q/A pairs used to evaluate retrieval/generation quality |
| **Groundedness** | degree to which an answer is supported by retrieved context |

---

## 15. SAP-C03 study pointers

- Read this book together with the D1.6 section of [AWS_SAP-C03_Study_Guide.md](AWS_SAP-C03_Study_Guide.md) and the Tier-A plan in [AWS_SAP-C03_Delta_Study_Plan.md](AWS_SAP-C03_Delta_Study_Plan.md).
- Pair with [AWS_S3_Storage_Classes_Tiering_Lifecycle_SAP-C02.md](AWS_S3_Storage_Classes_Tiering_Lifecycle_SAP-C02.md) for the data-source/security side and [AWS_IAM_Users_Groups_Roles_Policies_SAP-C02.md](AWS_IAM_Users_Groups_Roles_Policies_SAP-C02.md) for KB/role permissions.
- Exam-trigger sentence to remember: **“grounded answers from proprietary data with citations, freshness and access control → RAG, not fine-tuning; Bedrock Knowledge Bases, not a custom vector DB, unless the platform already exists.”**

---

## 16. Sources (verified November 2026)

- [Amazon Bedrock Knowledge Bases](https://docs.aws.amazon.com/bedrock/latest/userguide/knowledge-base.html)
- [How Amazon Bedrock knowledge bases work](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-how-it-works.html)
- [Content chunking for knowledge bases](https://docs.aws.amazon.com/bedrock/latest/userguide/kb-chunking.html)
- [Reranker models in Amazon Bedrock](https://docs.aws.amazon.com/bedrock/latest/userguide/rerank.html)
- [Amazon Bedrock AgentCore overview](https://docs.aws.amazon.com/bedrock-agentcore/latest/devguide/what-is-bedrock-agentcore.html)
- [SAP-C03 exam guide — Domain 1](https://docs.aws.amazon.com/aws-certification/latest/solutions-architect-professional-03/solutions-architect-professional-03-domain1.md)
