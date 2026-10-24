# 6. Requirements Specification

Priorities use MoSCoW: **M** must, **S** should, **C** could. Every **M** requirement has an acceptance criterion that can be checked by a test, a demonstration or a measurement. The "Source" column links each requirement to evidence: **I** interviews (doc 02, to be confirmed), **L** literature (doc 03), **E** existing systems (doc 04), **A** team assumption.

## 6.1 Actors

| Actor | Description |
|---|---|
| User | Developer, QA engineer or team lead with an account |
| GitHub | External platform: sends webhooks, receives review comments |
| LLM provider | External API (OpenAI, Gemini) |
| Target web app | Application under test, reachable by URL |

## 6.2 Functional requirements

### Accounts and projects (P)

| ID | Requirement | Pri | Acceptance criterion | Source |
|---|---|---|---|---|
| FR-P1 | User signs in with GitHub OAuth | M | A new user signs in with GitHub. A user record is created and the user lands on the project list | A |
| FR-P2 | User creates a project and links a GitHub repository through the GitHub App | M | After installing the App, the repository appears in the project. The project shows "connected" | E |
| FR-P3 | User adds one or more target URLs (environments) to a project | M | URL is validated (http/https, reachable). It appears in the environment list | A |
| FR-P4 | User stores test credentials for a target app as an encrypted secret | S | Secret is never returned by the API in plain text. It is used by the agent at run time | A |

### Code review (CR)

| ID | Requirement | Pri | Acceptance criterion | Source |
|---|---|---|---|---|
| FR-CR1 | Review starts automatically when a pull request is opened or updated | M | Opening a PR in a connected repo creates a review job within 10 s of the webhook | E, I |
| FR-CR2 | User can start a review manually for a PR or a branch | M | "Run review" in the UI creates a job. Its status is visible | A |
| FR-CR3 | The service sends the diff plus related repository files within a token budget | M | Prompt log for a test PR contains changed files and at least one imported/related file, and stays within the configured limit | L17, L19 |
| FR-CR4 | Each finding has type, severity, file, line range, explanation and suggested fix | M | Output validates against the defect JSON schema. Invalid output is retried or rejected | L13 |
| FR-CR5 | A second filtering pass removes low-confidence or duplicate findings | S | With filter on, the number of findings on the bench decreases and precision does not drop | L7 |
| FR-CR6 | Findings are posted as PR review comments on GitHub | S | Comments appear on the correct lines of the PR | E |
| FR-CR7 | User chooses LLM provider and model per project | M | Changing the model in settings changes the model recorded on the next run | L, A |

### Testing (T)

| ID | Requirement | Pri | Acceptance criterion | Source |
|---|---|---|---|---|
| FR-T1 | User writes a test scenario as natural-language steps and an expected result | M | A scenario with ≥1 step and an expected result is saved and listed | L11, E |
| FR-T2 | The agent executes each step in a real browser and decides actions from page state | M | Login scenario on the bench app passes without any locator written by the user | L10, L18 |
| FR-T3 | A failed step or unmet expectation produces a finding with screenshot, console log and step trace | M | Seeded UI defect produces a finding whose evidence includes screenshot and failing step | L20 |
| FR-T4 | Run progress is shown live (current step, screenshot) | S | UI updates step status without page reload during a run | A |
| FR-T5 | Runs are limited by step count and time | M | A scenario that cannot finish stops at the limit with status "aborted (limit)" | L9 |
| FR-T6 | Agent can generate test scenarios by exploring a page | C | From a URL, the agent proposes ≥3 scenarios that the user can save | L15 |

### Defects and reports (D, R)

| ID | Requirement | Pri | Acceptance criterion | Source |
|---|---|---|---|---|
| FR-D1 | Code-review and testing findings use one defect schema | M | Both services' outputs validate against `defect.schema.json` | A |
| FR-D2 | Evidence (screenshots, logs, diff hunks) is stored and linked to the finding | M | Opening a finding shows its evidence. The file loads from storage | L13 |
| FR-R1 | Run history per project with status, duration, model, number of findings | M | All runs of a project are listed and can be filtered by type and status | A |
| FR-R2 | Unified report for a release / commit range shows code and UI findings together | M | Report for a bench release contains findings from both services, sorted by severity | E, I |
| FR-R3 | User marks a finding as confirmed / false positive / fixed | M | Status change is saved and counted in statistics | L7 |
| FR-R4 | Report export to PDF or Markdown | S | Exported file contains all findings of the report | A |
| FR-R5 | Dashboard: findings by type and severity, precision from user labels, trend over runs | S | Charts render from stored data for a project with ≥5 runs | A |

### Evaluation support (EV)

| ID | Requirement | Pri | Acceptance criterion | Source |
|---|---|---|---|---|
| FR-EV1 | Runs can be executed in batch over the bench with a fixed configuration | M | A CLI command runs all bench items and stores results with a configuration ID | L6 |
| FR-EV2 | Results are exported to CSV for analysis | M | CSV contains one row per finding with run, configuration and ground-truth match | A |

## 6.3 Non-functional requirements

| ID | Category | Requirement | Pri | Acceptance criterion |
|---|---|---|---|---|
| NFR-P1 | Performance | Review of a PR ≤ 500 changed lines finishes in ≤ 3 min (p90) | M | Measured over bench PRs |
| NFR-P2 | Performance | API responses (non-job) ≤ 500 ms (p95) | S | Load test with 20 concurrent users |
| NFR-R1 | Reliability | Failed LLM / network calls retried up to 3 times with back-off; job never lost on service restart | M | Kill the worker during a job, then the job resumes or fails visibly |
| NFR-R2 | Reliability | Same scenario on an unchanged app gives the same verdict in ≥ 90% of 10 repeated runs | M | Stability test in the verification plan |
| NFR-S1 | Security | Secrets (API keys, test credentials, GitHub tokens) encrypted at rest; never logged | M | Code review + log inspection; secret scan in CI |
| NFR-S2 | Security | GitHub webhooks verified by signature | M | Request with wrong signature is rejected (401) |
| NFR-S3 | Security | Users see only their own projects | M | API test: access to another user's project returns 403 |
| NFR-U1 | Usability | A new user connects a repo and runs the first review in ≤ 10 min without help | S | Usability test with 5 participants |
| NFR-M1 | Maintainability | Services containerised. `docker compose up` starts the full system | M | Fresh machine start following README |
| NFR-M2 | Maintainability | LLM provider is behind an interface. Adding a provider needs no change in services | M | Second adapter added without touching review / testing logic |
| NFR-C1 | Cost | Token usage and cost recorded per run | S | Each run shows tokens and estimated cost |

## 6.4 Traceability (summary)

| Objective | Requirements |
|---|---|
| O4 Code review | FR-CR1..7, FR-D1..2, NFR-P1 |
| O5 Testing | FR-T1..6, FR-D1..2, NFR-R2 |
| O6 Client, report | FR-P1..4, FR-R1..5, NFR-U1 |
| O8 Evaluation | FR-EV1..2, FR-R3, NFR-C1 |
