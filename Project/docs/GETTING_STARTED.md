# Getting Started with ScopeGuard

Welcome to **ScopeGuard**! This guide will walk you through setting up, configuring, and running the application on your local machine.

**Author:** Yash Rai ([yashwardhanrai2007@gmail.com](mailto:yashwardhanrai2007@gmail.com))

---

## 📋 Prerequisites

Ensure you have the following installed:
* **Git** (`git --version`)
* **Docker Desktop** (Recommended for easiest setup) or:
  * **Python 3.12+**
  * **Node.js 20+** and **npm**

---

## ⚡ Quick Start: Running with Docker (Recommended)

1. **Clone your repository**:
   ```bash
   git clone https://github.com/yashxcode91/scope-guard.git
   cd scope-guard/Project
   ```

2. **Configure Environment Variables**:
   Copy the example environment template:
   ```bash
   cp .env.example .env
   ```
   > [!NOTE]
   > No API keys are required for the built-in demo! The application includes pre-indexed fixtures and offline deterministic rules.

3. **Launch the Application**:
   ```bash
   docker compose up --build
   ```

4. **Access the Services**:
   * **Web Interface**: [http://localhost:3000](http://localhost:3000)
   * **Backend API**: [http://localhost:8000](http://localhost:8000)
   * **API Docs (Swagger UI)**: [http://localhost:8000/docs](http://localhost:8000/docs)

---

## 💻 Manual Setup: Running Locally Without Docker

If you prefer running services directly on your host machine:

### 1. Backend Setup (FastAPI)
```bash
cd Project/backend

# Create virtual environment
python3 -m venv .venv
source .venv/bin/activate

# Install dependencies
pip install -e .

# Start FastAPI server
uvicorn app.api.main:app --reload --port 8000
```

### 2. Frontend Setup (Next.js)
Open a second terminal window:
```bash
cd Project/frontend

# Install dependencies
npm install

# Start Next.js development server
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

---

## 🔑 Configuring Live Language Models (Optional)

By default, the application functions offline using pre-calculated fixtures and rule-based analyzers. To connect live language models, edit `Project/.env`:

```env
# Optional: Token Harbor API Key (DeepSeek v4.1 Flash)
TOKENHARBOR_API_KEY=your_key_here

# Optional: OpenRouter API Key (Nemotron 3 Super / Inkling / Free tier)
OPENROUTER_API_KEY=your_key_here

# Disable live LLM calls (1 to force offline rule mode, 0 for live model)
LLM_DISABLED=0

# Backend endpoint configuration
NEXT_PUBLIC_API_URL=http://localhost:8000
NEXT_PUBLIC_MOCK=0
```

Restart your containers or local servers after modifying `.env`.

---

## 🧪 Running Automated Tests

Run backend unit and integration tests:
```bash
cd Project/backend
pytest -q
```

Run frontend linting and type-checks:
```bash
cd Project/frontend
npm run lint
npm run build
```
