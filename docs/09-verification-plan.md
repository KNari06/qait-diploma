# 9. Verification and Evaluation Plan

Owner: Сариев М.Б. The plan answers two questions:
1. **Verification:** does the platform meet its requirements?
2. **Evaluation:** how accurately does it find defects compared with a baseline? This is the research part.

## 9.1 Verification (does it work as specified)

| Level | What | Tool | Requirements covered | When |
|---|---|---|---|---|
| Unit | Context builder, schema validation, provider adapters, report queries | Vitest / Jest | FR-CR3, FR-D1, NFR-M2 | Every PR (CI) |
| API / integration | REST endpoints, auth, access rules, webhook signature | Supertest + test DB in Docker | FR-P*, NFR-S2, NFR-S3 | Every PR (CI) |
| End-to-end | Main user flows in the web client | Playwright | UC1, UC2, UC7, UC9 | Before each pre-defense |
| Reliability | Kill the worker during a job; LLM error injection | Scripted chaos test | NFR-R1 | Pre-defense 2 |
| Stability | 10 repeated runs of each bench scenario on unchanged app | Batch runner | NFR-R2 | Pre-defense 2 and final |
| Performance | Review time on bench PRs; API latency with 20 users | Batch runner, k6 | NFR-P1, NFR-P2 | Final |
| Usability | 5 participants connect a repo and run the first review | Moderated test, SUS questionnaire | NFR-U1 | Final |
| Security | Secret scanning, dependency audit | gitleaks, npm audit in CI | NFR-S1 | Every PR (CI) |

Every **M** requirement in [06-requirements.md](06-requirements.md) is covered by at least one test. The traceability matrix is kept in `tests/traceability.csv` <!-- TODO: create when tests appear -->.

## 9.2 Evaluation (how well it finds defects)

### Research questions

- **RQ1.** What precision, recall and F1 does LLM code review reach on seeded code defects, compared with static analysers?
- **RQ2.** What precision, recall and F1 does the browser agent reach on seeded UI defects, compared with scripted Playwright tests?
- **RQ3.** How much do code review and UI testing overlap, and how much does combining them add (union recall)?
- **RQ4.** How do model choice (e.g. GPT vs Gemini) and the filter pass affect accuracy, time and cost?

### Test bench

| Part | Content | Ground truth |
|---|---|---|
| B1 Code bench | ≥ 10 small/medium open-source TypeScript / Python repositories. In each, ≥ 5 pull requests with **seeded** defects (mutation-style: off-by-one, missing null check, wrong condition, unhandled error, injection, race) plus clean PRs as controls | Defect list: file, line range, type |
| B2 Public data | Sample of CodeReviewer dataset [L2] comments marked as defect-related | Human review comments |
| B3 Web bench | ≥ 3 web apps (e.g. a demo shop, a to-do app, an admin panel) with seeded UI defects: broken validation, wrong navigation, disabled button, wrong total, missing error message | Defect list: page, scenario, type |
| B4 Cross bench | ≥ 2 apps where the same seeded defect is visible both in code and in UI | For RQ3 |

Target size: **≥ 100 seeded defects** in total. Defects are written by Максот and reviewed by a second team member. Members who tune prompts do not see the bench defects (prevents overfitting).

### Matching rule

- **Code finding:** a true positive (TP) when it names the same file and its line range overlaps the seeded defect's range ±3 lines, and its type is compatible.
- **UI finding:** a TP when the same scenario fails at the step where the defect shows.
- Findings that match no seeded defect are reviewed manually and marked FP, or "real but unseeded". The second category is reported separately.

### Metrics

- Precision = TP / (TP + FP), Recall = TP / (TP + FN), F1.
- Per defect type and per severity.
- Overlap (RQ3): defects found only by review, only by testing, by both; union recall.
- Cost and time: tokens, USD, wall-clock per run (RQ4).
- Stability: share of repeated runs with the same verdict.

### Baselines

| For | Baseline | Why |
|---|---|---|
| RQ1 | Static analysers: ESLint + Semgrep (TS), Pylint + Semgrep (Python) | Common non-LLM practice [L14] |
| RQ2 | Scripted Playwright tests written once for the original app version | Common practice; brittle under UI change [L11] |
| RQ2-robustness | Same scripted tests after controlled UI changes (renamed ids, moved elements) vs. agent | Measures maintenance benefit |

### Conditions

- Fixed model versions and temperature 0. Each configuration is run 3 times and reported as mean ± std.
- Same hardware / Docker image for all runs. Configuration ID stored with every result (FR-EV1).
- Statistical test: McNemar's test for paired detection on the same defects (platform vs baseline), α = 0.05.

### Success criteria (hypotheses)

| ID | Criterion |
|---|---|
| H1 | LLM review recall on B1 is higher than the static-analyser baseline |
| H2 | LLM review precision ≥ 0.6 with the filter pass |
| H3 | After controlled UI changes, agent recall drops less than scripted-test recall |
| H4 | Union of review + testing has higher recall on B4 than either alone |

These are hypotheses. The report will state whether each is supported or rejected.

## 9.3 Threats to validity

- **Seeded vs. real defects:** seeded defects may be easier. Mitigation: include B2 real review data and report it separately.
- **Data contamination:** models may have seen public repositories [L16]. Mitigation: seed new defects and use recent or private bench code.
- **Non-determinism:** mitigation by repeated runs and reporting variance.
- **Small bench:** report confidence intervals and avoid broad claims.
