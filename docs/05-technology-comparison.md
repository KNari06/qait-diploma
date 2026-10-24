# 5. Technology Comparison

Each decision is scored 1–5 against requirements from [06-requirements.md](06-requirements.md). Weights reflect project priorities.

## T1. Web client

| Criterion (weight) | **React + TS** | Vue 3 + TS | Angular |
|---|---|---|---|
| Team experience (3) | 5 | 2 | 2 |
| Ecosystem for charts / tables (2) | 5 | 4 | 4 |
| Real-time updates via WebSocket (1) | 5 | 5 | 5 |
| Learning / boilerplate cost (1) | 4 | 5 | 2 |
| **Weighted total** | **34** | 24 | 21 |

**Decision:** React with TypeScript and Vite. The frontend developer works with React commercially, and charting libraries (Recharts, ECharts) and table components are mature. Supports NFR-U1, FR-R1..R5.

## T2. Backend platform

| Criterion (weight) | **Node.js (TS, Express)** | Python (FastAPI) | Java (Spring Boot) |
|---|---|---|---|
| Same language as client, shared types (3) | 5 | 2 | 2 |
| LLM SDKs (OpenAI, Gemini) (2) | 5 | 5 | 3 |
| Playwright support (2) | 5 (native) | 4 | 4 |
| Async I/O for many waiting LLM calls (2) | 5 | 5 | 4 |
| Team experience (2) | 5 | 3 | 2 |
| **Weighted total** | **55** | 40 | 32 |

**Decision:** Node.js with TypeScript for all services. A shared package holds the defect schema used by every service (FR-D1). Python stays an option for the evaluation scripts (pandas), which are not part of the runtime.

## T3. Data storage

| Criterion (weight) | **PostgreSQL** | MongoDB | SQLite |
|---|---|---|---|
| Relational model: project → run → finding (3) | 5 | 3 | 5 |
| JSONB for model output and evidence (2) | 5 | 5 | 3 |
| Concurrent writes from several services (2) | 5 | 5 | 1 |
| Aggregation queries for dashboard (2) | 5 | 4 | 4 |
| Managed hosting options (1) | 5 | 5 | 1 |
| **Weighted total** | **50** | 42 | 32 |

**Decision:** PostgreSQL. Findings are strongly relational, and reports and dashboards need joins and grouping (FR-R3, FR-R5). JSONB stores raw model output. Screenshots and logs go to object storage (S3-compatible), and only their URLs are kept in the database.

## T4. Service interaction

| Criterion (weight) | REST only (synchronous) | **REST + Redis job queue (BullMQ)** | RabbitMQ / Kafka |
|---|---|---|---|
| Long jobs (review 1–5 min, test runs up to 15 min) (3) | 1 | 5 | 5 |
| Retries and back-off for LLM / network errors (2) | 2 | 5 | 4 |
| Operational simplicity for a 3-person team (2) | 5 | 4 | 2 |
| Progress events to UI (1) | 2 | 4 (with WebSocket) | 4 |
| **Weighted total** | 19 | **37** | 31 |

**Decision:** REST for client ↔ gateway and synchronous queries. Review and test jobs go through a Redis-backed queue with retries (NFR-R1). Progress reaches the client by WebSocket (FR-T4). Kafka's throughput is not needed at this scale.

## T5. Browser automation

| Criterion (weight) | **Playwright** | Selenium WebDriver | Puppeteer |
|---|---|---|---|
| Accessibility tree / DOM snapshot for the agent (3) | 5 | 3 | 4 |
| Cross-browser (Chromium, Firefox, WebKit) (1) | 5 | 5 | 2 |
| Auto-waiting, fewer flaky steps (2) | 5 | 2 | 3 |
| Trace, screenshot, video built in (2) | 5 | 2 | 3 |
| Headless in Docker (1) | 5 | 4 | 5 |
| **Weighted total** | **45** | 26 | 31 |

**Decision:** Playwright. Its accessibility snapshot gives the LLM a compact page description [L10], and its trace and screenshots serve as evidence (FR-D2).

## T6. LLM provider

| Criterion (weight) | OpenAI (GPT family) | Google Gemini | Self-hosted open model (e.g. Llama via vLLM) |
|---|---|---|---|
| Code review quality (3) | 5 | 4 | 3 |
| Structured JSON output (2) | 5 | 4 | 3 |
| Long context for repository files (2) | 4 | 5 | 3 |
| Cost per run (2) | 3 | 4 | 4 (needs GPU) |
| Data stays on premises (1) | 1 | 1 | 5 |
| **Weighted total** | 40 | 39 | 34 |

**Decision:** no single provider. A provider interface (`LLMProvider`) with OpenAI and Gemini adapters lets the experiments compare models (Objective O8, NFR-M2). A self-hosted model is a possible extension. It is not required.

## Summary

| Decision | Choice |
|---|---|
| T1 Web client | React + TypeScript |
| T2 Backend | Node.js + TypeScript (Express) |
| T3 Storage | PostgreSQL + S3-compatible object storage |
| T4 Interaction | REST + Redis/BullMQ queue + WebSocket |
| T5 Browser | Playwright |
| T6 LLM | Pluggable: OpenAI, Gemini |
