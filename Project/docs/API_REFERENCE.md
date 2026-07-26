# API Reference — ScopeGuard

The backend is built with **FastAPI**. Interactive OpenAPI documentation (Swagger UI) is available at `http://localhost:8000/docs`.

**Author:** Yash Rai ([yashwardhanrai2007@gmail.com](mailto:yashwardhanrai2007@gmail.com))

---

## 🔍 Core Endpoints

### 1. Service Health
**`GET /health`**

Checks backend status and available language model configurations.

**Response:**
```json
{
  "status": "ok",
  "providers_configured": {
    "tokenharbor": false,
    "openrouter": false
  },
  "llm_disabled": false
}
```

---

### 2. Single Story Analysis
**`POST /analyze`**

Performs full change impact analysis on a single user story or epic.

**Request Body:**
```json
{
  "story": {
    "id": "EX-201",
    "title": "Add OTP multi-factor login",
    "description": "Add a one-time passcode step to customer login. Authenticate via notification-service SMS.",
    "type": "epic",
    "acceptance_criteria": [
      "Customers must enter valid 6-digit OTP before login session is granted",
      "Codes expire in 5 minutes"
    ]
  }
}
```

**Response Properties:**
* `impacted_systems`: List of directly and transitively affected systems.
* `risk_scores`: 0–100 scores across Security, Compliance, Technical, Operational, Performance, and Delivery.
* `compliance`: Triggered regulatory frameworks (GDPR, PCI DSS, SOX) and required actions.
* `release_decision`: `Go`, `Go with conditions`, or `No go`.
* `test_plan`: Recommended unit, API, integration, and security tests.

---

### 3. Sprint Backlog Analysis
**`POST /analyze-sprint`**

Analyzes an entire sprint backlog and flags system conflicts between concurrent stories.

**Request Body:**
```json
{
  "sprint_id": "SPRINT-42",
  "stories": [
    {
      "id": "STORY-101",
      "title": "Update payment limit",
      "description": "Increase standard transfer allowance to AED 50,000"
    },
    {
      "id": "STORY-102",
      "title": "Card freeze overhaul",
      "description": "Refactor card freeze workflow in payment service"
    }
  ]
}
```

**Response Properties:**
* `kpis`: Total stories, total touched systems, portfolio risk index, and release readiness.
* `conflicts`: Detected collision pairs where two stories touch the same services or database tables.
* `recommendations`: Sprint planning guidance to minimize deployment risk.

---

### 4. Codebase Mapping
**`POST /codebase/analyze`**

Clones/indexes a target GitHub repository and maps the story to specific files, classes, endpoints, and tables.

**Request Body:**
```json
{
  "repo_url": "https://github.com/example/demo-bank",
  "story": {
    "id": "EX-201",
    "title": "OTP multi-factor login",
    "description": "Add SMS OTP verification before issuing JWT session tokens"
  }
}
```

**Response Properties:**
* `matched_files`: Exact files requiring changes with diff previews.
* `classes_impacted`: Classes and methods touched.
* `api_routes`: Endpoints modified or newly exposed.
* `db_tables`: Database tables affected by migrations.

---

### 5. Copilot Chat
**`POST /chat`**

Asks context-aware questions about an active analysis or sprint report.

**Request Body:**
```json
{
  "query": "Why was the security risk scored at 93 for story EX-201?",
  "story_id": "EX-201"
}
```

**Response:**
```json
{
  "answer": "Security risk is scored at 93 because the change directly alters the customer authentication pathway, touches cardholder identity validation, and exposes a new public API endpoint.",
  "citations": ["auth-service", "notification-service"]
}
```
