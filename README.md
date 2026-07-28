# Omni-RAG 🚀
<div align="center">
  <img src="Owl.jpeg" alt="Wise Owl width="250" height="250">



> **A Production-Grade, Domain-Agnostic Retrieval-Augmented Generation Engine**

Omni-RAG is a high-throughput, highly extensible RAG framework engineered to drop seamlessly into any production environment—from e-commerce recommendation engines and customer support agents to internal enterprise knowledge bases. Built with a modular core in PyTorch, a high-performance asynchronous FastAPI backend, a resilient Redis caching/queuing layer, and a modern Next.js + TypeScript dashboard.


[![License: MIT](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)
[![Docker](https://img.shields.io/badge/docker-compose-2496ED.svg)](docker-compose.yml)
[![Python 3.11+](https://img.shields.io/badge/python-3.12%2B-blue.svg)](https://www.python.org/downloads/)
[![Code style: ruff](https://img.shields.io/badge/code%20style-ruff-000000.svg)](https://github.com/astral-sh/ruff)
[![Type-checked: mypy](https://img.shields.io/badge/type%20checked-mypy-blue.svg)](http://mypy-lang.org/)
[![FastAPI](https://img.shields.io/badge/API-FastAPI-009688.svg)](https://fastapi.tiangolo.com)
[![Next.js 15](https://img.shields.io/badge/Frontend-Next.js%2015-black.svg)](https://nextjs.org)
[![PyTorch 2.x](https://img.shields.io/badge/ML-PyTorch%202.x-ee4c2c.svg)](https://pytorch.org)
[![MLflow](https://img.shields.io/badge/Tracking-MLflow-0194E2.svg)](https://mlflow.org)
[![PostgreSQL 16](https://img.shields.io/badge/DB-PostgreSQL%2016-336791.svg)](https://www.postgresql.org)
[![Redis 7](https://img.shields.io/badge/Cache-Redis%207-DC382D.svg)](https://redis.io)
[![Security: bandit](https://img.shields.io/badge/security-bandit-yellow.svg)](https://github.com/PyCQA/bandit)
[![OpenSSF](https://img.shields.io/badge/OpenSSF-Scorecard-blue.svg)](https://scorecard.dev/viewer/?uri=github.com/diptu/ecoLens)

</div>
---

## 🏗️ Architectural Overview

```
+---------------------------------------------------------------------------------+
|                                 Next.js + TS UI                                  |
|                 (Chat Interface, Document Management, Analytics)                |
+---------------------------------------------------------------------------------+
                                         |
                                         v (REST / WebSocket)
+---------------------------------------------------------------------------------+
|                                FastAPI Backend                                  |
|        (Async API Gateway, Pipeline Orchestrator, Auth & Rate Limiting)         |
+---------------------------------------------------------------------------------+
            |                                         |
            v                                         v
+-----------------------+                 +-------------------------------+
|     PyTorch Core      | <-------------> |         Redis Layer           |
| (Embeddings, Vector   |                 | (Semantic Cache, Rate Limits, |
|  Search, Reranking)   |                 |      Background Queue)        |
+-----------------------+                 +-------------------------------+
```

---

## 🛠️ Technology Stack

| Layer | Technology | Purpose |
| :--- | :--- | :--- |
| **Core ML / AI** | **PyTorch** | Deep learning runtime, tensor operations, custom embedding & reranking models. |
| **Backend API** | **FastAPI** | High-performance asynchronous Python API server with automatic OpenAPI documentation. |
| **Caching & State** | **Redis** | Sub-millisecond semantic caching, session management, and rate-limiting queues. |
| **Frontend UI** | **Next.js (App Router) + TypeScript** | Type-safe, production-ready admin and chat interface with streaming responses. |

---

## 📂 Repository Structure

```tree
omni-rag/
├── backend/                  # FastAPI Application Core
│   ├── app/
│   │   ├── core/             # Configuration, logging, security
│   │   ├── models/           # PyTorch models & embedding pipelines
│   │   ├── services/         # Retrieval, generation, & orchestration logic
│   │   ├── api/              # API routers (v1 endpoints)
│   │   └── main.py           # FastAPI entrypoint
│   ├── Dockerfile
│   └── requirements.txt
├── frontend/                 # Next.js Dashboard & Chat App
│   ├── src/
│   │   ├── app/              # App router pages & layouts
│   │   ├── components/       # Reusable UI components (shadcn/ui style)
│   │   └── lib/              # API clients & state hooks
│   ├── Dockerfile
│   └── package.json
├── docker-compose.yml        # Multi-container orchestration (Redis, Backend, Frontend)
└── README.md
```

---

## ⚙️ Quick Start (Docker Compose)

The fastest way to spin up the entire production-grade stack locally is using Docker Compose.

### Prerequisites
* [Docker](https://www.docker.com/) & Docker Compose
* NVIDIA Container Toolkit (Optional, for GPU-accelerated PyTorch inference)

### 1. Clone & Configure Environment
```bash
git clone https://github.com/your-org/omni-rag.git
cd omni-rag

# Copy and fill out environment variables
cp .env.example .env
```

### 2. Run with Docker Compose
```bash
docker-compose up --build -d
```

Services will be available at:
* **Frontend Dashboard:** `http://localhost:3000`
* **FastAPI Backend Swagger Docs:** `http://localhost:8000/docs`
* **Redis Instance:** `localhost:6379`

---

## 🚀 Manual Local Development Setup

### Backend (FastAPI + PyTorch)
```bash
cd backend
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate
pip install -r requirements.txt

# Run development server
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload
```

### Frontend (Next.js + TypeScript)
```bash
cd frontend
npm install

# Run development server
npm run dev
```

---

## 🔌 API Integration Example

Omni-RAG exposes clean REST endpoints designed to be integrated into any external system (e.g., e-commerce cart assistant, internal HR bot).

### Query Endpoint (`POST /api/v1/query`)

**Request:**
```json
{
  "query": "What is the return policy for international electronics?",
  "top_k": 5,
  "namespace": "ecommerce-policies",
  "temperature": 0.1
}
```

**Response:**
```json
{
  "response": "International electronics orders are eligible for returns within 30 days of delivery, provided they are in their original packaging...",
  "sources": [
    {
      "id": "doc_8842",
      "score": 0.9412,
      "metadata": { "source": "global_shipping_policy_v2.pdf", "page": 14 }
    }
  ],
  "latency_ms": 142
}
```

---

## 🛡️ Production Hardening & Features

- **Semantic Caching:** Frequent queries and near-duplicate embeddings are cached directly in Redis to reduce LLM token costs and latency by up to 70%.
- **Asynchronous Ingestion Pipeline:** Document parsing, text chunking, and PyTorch tensor embedding generation run asynchronously through worker queues.
- **Strict Typing:** End-to-end type safety with TypeScript on the frontend and Pydantic validation schemas on the backend.
- **Horizontal Scalability:** Stateless FastAPI instances allow seamless scaling behind an Nginx or cloud load balancer.

---

## 🤝 Contributing

Contributions are welcome! Please review our [Contributing Guidelines](CONTRIBUTING.md) before submitting pull requests.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE).
