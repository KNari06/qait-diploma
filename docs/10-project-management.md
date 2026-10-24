# 10. Project Management and Risks

## 10.1 Tools and workflow

- **Repositories:** [back](https://github.com/bekhop2006/qait-dimploma-back), [worker](https://github.com/bekhop2006/qait-dimploma-worker), [front](https://github.com/bekhop2006/qait-dimploma-front). `main` is protected and changes come only by pull request with one approving review from another team member.
- **YouTrack:** https://qait-diploma.youtrack.cloud/projects/QAIT, project key `QAIT`. The backlog in 10.4 is imported from [youtrack-backlog.csv](youtrack-backlog.csv). Each task has an assignee, expected outcome, acceptance criteria, deadline and priority.
- **Branch / commit convention:** `QAIT-12-defect-schema`, commit `QAIT-12: Add defect schema draft`. The PR description links the task and says how the change was checked, and the YouTrack task links back to the PR.
- **Tags:** `prob-period-1` (pre-defense 1), then the tags announced for the next stages.
- **Meetings:** weekly with the supervisor, and a short team sync twice a week.

## 10.2 Responsibilities

| Area | Responsible | Reviewer |
|---|---|---|
| Web client, UX prototype, report and dashboard | Қайдар Н.Н. | Таниберген Б.Б. |
| Gateway, services, database, GitHub integration, deployment | Таниберген Б.Б. | Сариев М.Б. |
| Test bench, evaluation, verification, experiments | Сариев М.Б. | Қайдар Н.Н. |
| Documentation sections | Each owner for their area | Supervisor |

## 10.3 Milestones

Dates after M1 are tentative and will be aligned with the School's pre-defense schedule.

| # | Milestone | Deliverables | Due |
|---|---|---|---|
| M1 | Pre-defense 1 | Docs 01–11, diagrams, prototype, YouTrack backlog | 24 Oct 2026 |
| M2 | Skeleton | Monorepo, CI, Docker Compose, auth, project CRUD | 15 Nov 2026 |
| M3 | Code review MVP | GitHub App, review service, findings in UI | 13 Dec 2026 |
| M4 | Testing MVP | Browser agent, scenarios, evidence | 31 Jan 2027 |
| M5 | Pre-defense 2 | Unified report, first bench results | 28 Feb 2027 |
| M6 | Evaluation | Full bench, baselines, statistics | 4 Apr 2027 |
| M7 | Final pre-defense | Explanatory note, final results, demo | 30 Apr 2027 |

## 10.4 Initial backlog (YouTrack)

Import: YouTrack → project → Import → CSV → [youtrack-backlog.csv](youtrack-backlog.csv). Map *Assignee* to team logins and *Fix versions* to milestones M1–M7.

| ID | Task | Assignee | Priority | Milestone |
|---|---|---|---|---|
| QAIT-1 | Project proposal and scope | Бекнұр | High | M1 |
| QAIT-2 | Interview guide; conduct and summarise 3–5 interviews | Нариман | High | M1 |
| QAIT-3 | Literature review and analysis table | Максот | High | M1 |
| QAIT-4 | Existing system comparison, with evidence log | Нариман | High | M1 |
| QAIT-5 | Technology comparison | Бекнұр | High | M1 |
| QAIT-6 | Requirements and acceptance criteria | Максот | High | M1 |
| QAIT-7 | Context, use case, component diagrams | Бекнұр | High | M1 |
| QAIT-8 | UX prototype of key scenarios | Нариман | High | M1 |
| QAIT-9 | Verification and evaluation plan | Максот | High | M1 |
| QAIT-10 | Defect schema v1 | Бекнұр | High | M2 |
| QAIT-11 | Monorepo, CI, Docker Compose | Бекнұр | High | M2 |
| QAIT-12 | GitHub OAuth sign-in | Бекнұр | High | M2 |
| QAIT-13 | Web client skeleton, routing, auth screens | Нариман | High | M2 |
| QAIT-14 | GitHub App + webhook handling | Бекнұр | High | M3 |
| QAIT-15 | Review service: context builder and prompt | Бекнұр | High | M3 |
| QAIT-16 | Run history and finding view | Нариман | High | M3 |
| QAIT-17 | Bench B1: select repos, seed defects | Максот | High | M3 |
| QAIT-18 | Testing service: agent loop with Playwright | Бекнұр | High | M4 |
| QAIT-19 | Scenario editor and live run view | Нариман | Medium | M4 |
| QAIT-20 | Bench B3: web apps with seeded defects | Максот | High | M4 |
| QAIT-21 | Unified report and export | Нариман | High | M5 |
| QAIT-22 | Batch runner and CSV export | Максот | High | M5 |
| QAIT-23 | Baselines: static analysers, scripted tests | Максот | High | M6 |
| QAIT-24 | Dashboard | Нариман | Medium | M6 |

## 10.5 Risks

| # | Risk | Prob. | Impact | Mitigation | Owner |
|---|---|---|---|---|---|
| R1 | LLM output unstable or not in schema | High | High | Structured output, schema validation with retry, temperature 0, repeated runs | Бекнұр |
| R2 | LLM API cost exceeds budget | Medium | Medium | Token budget per run, cost logging, smaller models for the filter pass, cached bench runs | Бекнұр |
| R3 | Browser agent fails on complex UIs | High | Medium | Step and time limits, simplified page state, bench apps of controlled complexity | Максот |
| R4 | Bench too small or biased for conclusions | Medium | High | ≥ 100 defects, second reviewer, real-data subset B2 | Максот |
| R5 | API provider changes or rate limits | Medium | Medium | Adapter interface, second provider | Бекнұр |
| R6 | Workload imbalance, a member falls behind | Medium | High | Weekly review of YouTrack, re-plan tasks, cross-review | All |
| R7 | GitHub App permissions / webhook issues in demo | Low | High | Test repo for demo, recorded fallback video | Бекнұр |
| R8 | Data privacy: sending customer code to LLM | Medium | Medium | Bench uses open or own code; self-hosted option documented | All |
| R9 | Agent "passes" a test by acting on the wrong element, hiding a real bug | Medium | High | Log every element choice with reason and confidence; confidence threshold; seeded defects measure false passes | Максот |
| R10 | Test credentials leak into prompts or logs | Low | High | Credentials stored as secrets and injected at action time, never sent to the LLM; log redaction | Бекнұр |
| R11 | Scope too large for the diploma | Medium | High | Focus evaluation on two ideas: review + testing complementarity and robustness to UI change | All |

## 10.6 Dependencies

- OpenAI and Gemini API keys (team budget). <!-- TODO: confirm budget -->
- GitHub organisation for the GitHub App and test repositories.
- Hosting for the demo (VPS or cloud credits).
- Supervisor review at each milestone.
