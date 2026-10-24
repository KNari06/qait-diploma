# QAIT — LLM-Based Platform for Software Quality Assurance Automation

Diploma project, Astana IT University, School of Software Engineering, 2026–2027.

QAIT is a web platform that combines **LLM-based code review of Git repositories** and **AI-driven testing of web applications by a browser agent**, and merges the results of both into **one defect report**. The research part measures how accurately each approach finds defects and how far the two complement each other.

## Problem statement

Manual code review and manual / scripted testing take a large share of development time. Scripted UI tests (Selenium, Playwright, Cypress) break when the interface changes and need constant maintenance. Static analysers find only part of the defects and produce many false positives. Small teams without a dedicated QA-automation engineer often skip both.

Large language models can read code changes and drive a browser from a natural-language description, but existing tools do either code review **or** testing, keep the results in separate places, and rarely report how accurate they are.

## Target users

- Small and medium development teams without a dedicated test-automation engineer.
- QA engineers who want to describe test scenarios in natural language instead of scripts.
- Team leads who need one view of the defects found in a release.

## Aim

Design, build and evaluate a platform that automates code review and web testing with LLMs and presents their findings in a single defect report.

## Objectives

1. Analyse the users' code-review and testing workflows and derive requirements (see [docs/02-user-research.md](docs/02-user-research.md)).
2. Review the literature on LLM-based code review, LLM-based testing and web agents.
3. Compare existing systems and technology alternatives, and justify the choices.
4. Design a service-based architecture with a common defect format.
5. Implement the code-review service (GitHub integration, pull-request analysis).
6. Implement the testing service (browser agent that runs natural-language scenarios).
7. Implement the web client with the unified report and dashboard.
8. Build a test bench of repositories and web applications with known (seeded) defects and evaluate precision, recall and F1 against a baseline.

## Expected outcomes

- A working web platform with code-review and testing services and one defect report.
- A test bench with seeded defects and a reproducible evaluation method.
- An experimental comparison of LLM configurations against a baseline (static analysers / scripted tests).

## Scope

**In scope:** GitHub repositories (public and private through a GitHub App), pull-request review, web applications reachable by URL, natural-language test scenarios, OpenAI and Gemini models, the unified report, the evaluation bench.

**Out of scope:** GitLab / Bitbucket, native mobile apps, load and security testing, self-hosted model training.

Full scope: [docs/01-proposal.md](docs/01-proposal.md).

## Team

| Student | Group | Responsibility |
|---|---|---|
| Қайдар Нариман Нұрланұлы | SE-2406 | Web client, unified report, dashboard, UX prototype |
| Таниберген Бекнұр Болатбекұлы | SE-2406 | Backend, service architecture, GitHub integration, database, deployment |
| Сариев Максот Бахтыгалиевич | SE-2406 | Test bench, evaluation method, experiments, platform testing |

**Supervisor:** Айтмұханбетова Эльвира Айтмұханбетқызы, senior lecturer, School of Software Engineering.

## Relation to prior work

Team members previously worked on QAIT.dev, an AI platform for automated web and mobile testing. <!-- TODO(team): state exactly which components, if any, are reused from QAIT.dev and which are developed for the diploma. --> The comparison with QAIT.dev is part of [docs/04-existing-systems.md](docs/04-existing-systems.md).

## Current status

Stage: **pre-defense 1** (analysis, design, planning).

| Area | Status |
|---|---|
| Proposal, scope | Done |
| User research | In progress |
| Literature review, comparisons | Done (first version) |
| Requirements | Done (first version) |
| Architecture, diagrams | Done (draft) |
| UX prototype | In progress |
| Verification plan | Done (first version) |
| Implementation | <!-- TODO(team) --> |

## Technologies

| Layer | Choice | Justification |
|---|---|---|
| Web client | React, TypeScript, Vite | [docs/05](docs/05-technology-comparison.md#t1-web-client) |
| Backend services | Node.js (TypeScript, Express) | [docs/05](docs/05-technology-comparison.md#t2-backend-platform) |
| Database | PostgreSQL | [docs/05](docs/05-technology-comparison.md#t3-data-storage) |
| Service interaction | REST + Redis-based job queue (BullMQ), WebSocket for live status | [docs/05](docs/05-technology-comparison.md#t4-service-interaction) |
| Browser automation | Playwright | [docs/05](docs/05-technology-comparison.md#t5-browser-automation) |
| LLM providers | OpenAI, Google Gemini (pluggable) | [docs/05](docs/05-technology-comparison.md#t6-llm-provider) |
| Deployment | Docker, GitHub Actions | |

## Links

- YouTrack: https://qait-diploma.youtrack.cloud/projects/QAIT (import file: [docs/youtrack-backlog.csv](docs/youtrack-backlog.csv))
- Source code repositories:
  - Backend: https://github.com/KNari06/diploma-back
  - Worker (testing runner): https://github.com/KNari06/diploma-worker
  - Web client: https://github.com/KNari06/diploma-front
- Prototype: [docs/08-ux-prototype.md](docs/08-ux-prototype.md)
- Documentation index:

| # | Document |
|---|---|
| 1 | [Project proposal and scope](docs/01-proposal.md) |
| 2 | [User research (evidence of relevance)](docs/02-user-research.md) |
| 3 | [Literature review](docs/03-literature-review.md) |
| 4 | [Existing system comparison](docs/04-existing-systems.md) |
| 5 | [Technology comparison](docs/05-technology-comparison.md) |
| 6 | [Requirements specification](docs/06-requirements.md) |
| 7 | [Architecture and diagrams](docs/07-architecture.md) |
| 8 | [UX prototype](docs/08-ux-prototype.md) |
| 9 | [Verification and evaluation plan](docs/09-verification-plan.md) |
| 10 | [Project management and risks](docs/10-project-management.md) |
| 11 | [Supervisor feedback](docs/11-supervisor-feedback.md) |
| — | [Pre-defense 1 submission report](docs/pre-defense-1-report.md) |
| — | [Requirements checklist](docs/00-requirements-checklist.md) |

## Repository structure

```
src/            application code, one folder per component (web, gateway, review-service, testing-service, report-service)
docs/           proposal, research, reviews, comparisons, requirements, decisions
diagrams/       editable sources (.puml) and exports (.svg / .png)
tests/          automated tests and the evaluation bench
config/         configuration examples (.env.example)
```

## Setup, run and test

No runnable code is submitted at this stage. Instructions will be added with the first service.

```bash
cp config/.env.example .env
# docker compose up   (from pre-defense 2)
```

## Workflow

- `main` is the reviewed baseline; work is done in branches named after the YouTrack task, e.g. `QAIT-12-defect-schema`.
- Commit messages start with the task ID: `QAIT-12: Add defect schema draft`.
- Every change goes through a pull request; from milestone M2 each pull request is reviewed by another team member before merging.
- Each pre-defense submission is tagged: `prob-period-1` (pre-defense 1), then the tags announced for the next stages.
