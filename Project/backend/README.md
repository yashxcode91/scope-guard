# ScopeGuard — Backend API & Engine

FastAPI backend service powering the **ScopeGuard** change-impact analysis engine.

**Author:** Yash Rai ([yashwardhanrai2007@gmail.com](mailto:yashwardhanrai2007@gmail.com))

---

## 🏛️ Architecture Overview

The backend uses a strict **"Extract Facts; Deterministic Code Decides"** design principle:
1. **Fact Extraction Layer (`app/agents/`)**: Specialized extractors parse user stories and GitHub repositories to extract structured facts: touched systems, components, data classifications, public API changes, and authentication hooks.
2. **Deterministic Rules Engine (`app/engine/`)**: Pure Python code computes all 6-dimension risk scores (0–100), traverses system dependency graphs using **NetworkX**, verifies compliance requirements, and determines Go / No-Go release verdicts.
3. **Resilient Provider Chain (`app/llm.py`)**: Tries multiple providers in sequence (Token Harbor DeepSeek, OpenRouter Nemotron/Inkling) and seamlessly falls back to offline rule-based analyzers if no external provider is configured or available.

---

## 📁 Directory Structure

```
app/
├── api/             # FastAPI routers (analysis, sprint, codebase, runs, chat)
├── agents/          # Story extraction, compliance audit, test planning, release summary
├── codebase/        # Source code fetcher, parser & AST indexer
├── engine/          # Graph traversal, conflict detection & risk scoring algorithms
├── architecture.py  # Loads bank systems graph and test catalog
├── config.py        # Environment variables and configuration flags
├── contracts.py     # Pydantic v2 data models & request/response contracts
├── db.py            # SQLite database with SQLModel for run persistence
├── llm.py           # Multi-provider chain with automatic fallbacks
├── pipeline.py      # Execution pipeline orchestration
├── runs.py          # Background task execution & live status tracking
└── scoring_config.yaml  # Configurable scoring weights and rules
```

---

## 🚀 Running the Backend

### Option 1: With Docker Compose (Root directory)

From the `Project/` folder:
```bash
docker compose up --build
```
The backend will be available at `http://localhost:8000`.

### Option 2: Running Locally with Python 3.12

1. Navigate to the backend directory:
   ```bash
   cd Project/backend
   ```
2. Create and activate a virtual environment:
   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   ```
3. Install dependencies:
   ```bash
   pip install -e .
   ```
4. Run the development server:
   ```bash
   uvicorn app.api.main:app --reload --port 8000
   ```

---

## 🧪 Testing

The backend includes comprehensive test suites covering extraction, graph traversal, risk scoring, codebase analysis, and fallbacks. **Tests run offline without making external calls.**

Run tests with pytest:
```bash
pytest -q
```

---

## 📡 Key Endpoints

| Method | Path | Description |
|---|---|---|
| `GET` | `/health` | Service health status and provider configuration |
| `POST` | `/analyze` | Analyze a single user story or epic |
| `POST` | `/analyze-sprint` | Analyze a full sprint backlog with conflict detection |
| `POST` | `/codebase/analyze` | Map a user story against a real GitHub repository |
| `POST` | `/runs/{type}` | Trigger an asynchronous analysis run |
| `GET` | `/runs/{run_id}` | Live progress polling and result retrieval |
| `GET` | `/architecture` | Returns the system dependency catalog |
| `POST` | `/chat` | Conversational assistant for querying analysis details |

Interactive Swagger documentation is available at `http://localhost:8000/docs`.
