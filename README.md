# AI Study Companion



A full-stack, multi-modal agentic RAG platform where students upload their lecture notes and past exam papers, chat with them, and get study material where every answer cites the exact document and page it came from.

## 🎥 Live Demo

[![AI Study Companion demo](https://img.youtube.com/vi/SwQLAcRuiKA/maxresdefault.jpg)](https://youtu.be/SwQLAcRuiKA)

▶️ Watch on YouTube: https://youtu.be/SwQLAcRuiKA

## ✨ Features

- **Chat with your documents:** upload PDFs, DOCX, PPTX or a URL and ask questions, with every answer cited back to the source page
- **Live web search:** a supervisor agent decides when to search your documents and when to search the web, then combines both
- **Study Studio:** generates summaries, mind maps, flashcards and AI-graded adaptive Q&A from notes and past papers
- **Exam-aware revision:** cross-references lecture content against past exam questions to flag topics likely to come up again
- **Safety guardrails:** every message is checked for toxicity, prompt injection and personal data
- **Live streaming:** answers stream token by token, with the model's reasoning visible as it works

## 🧠 How It Works

**Ingestion:** an async Celery + Redis pipeline processes a 300-page document in under 45 seconds without blocking the app. Unstructured.io splits documents into text, tables and images, and GPT-4o summarises tables and images so they stay searchable. Chunks are embedded into Supabase pgvector.

**Retrieval:** multi-query generation creates several versions of each question. Each one runs parallel vector search (pgvector HNSW) and keyword search (PostgreSQL full-text), and the results are fused with Reciprocal Rank Fusion, then reranked with Cohere.

**Agents:** two LangGraph agents with tool calling: a ReAct agent grounded in your documents, and a supervisor that routes between document search and a live web-search sub-agent (Tavily, with DuckDuckGo as a fallback).

**Infrastructure:** files upload directly to S3 through presigned URLs. The app is containerised with Docker and deployed on AWS ECS Fargate behind a load balancer, with GitHub Actions CI/CD, JWT auth, multi-tenant data isolation, and structured JSON logging traced in LangSmith.

## 📊 Evaluation

Evaluated with RAGAS on 50 real student questions:

| Metric | Score |
|---|---|
| Faithfulness | **0.92** |
| Context Precision | **0.95** |

Re-tested across multiple chunking strategies until the scores held up.

## 🛠️ Tech Stack

**Frontend:** Next.js, React, TypeScript
**Backend:** FastAPI, Python, Celery, Redis
**AI:** LangGraph, LangChain, OpenAI GPT-4o, Cohere Rerank, LangSmith, RAGAS
**Data:** Supabase (PostgreSQL + pgvector), Unstructured.io
**Infrastructure:** Docker, AWS (ECS Fargate, ALB, S3), GitHub Actions

## 🔗 Links

- **Backend code:** [fyp-server](https://github.com/mueed26/fyp-server)
- **LinkedIn:** [Mueed Hyder Mir](https://linkedin.com/in/mirmueed)

---

Built by **Mueed Hyder Mir** 
