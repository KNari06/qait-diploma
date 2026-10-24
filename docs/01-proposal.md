# 1. Project Proposal

| | |
|---|---|
| **Title** | Development of an AI platform for software quality assurance automation based on large language models |
| **Programme** | Software Engineering |
| **Form** | Team diploma project (3 students) |
| **Team** | Қайдар Н.Н. (web client, reports) · Таниберген Б.Б. (backend, architecture) · Сариев М.Б. (test bench, evaluation) — group SE-2406 |
| **Supervisor** | Айтмұханбетова Э.А., senior lecturer, School of Software Engineering |

## 1.1 Problem statement

Quality assurance in small teams relies on two activities: **code review** of changes and **testing** of the running application.

- Code review is manual and slow. Reviewers miss defects when a change is large or when they are not familiar with the code. Studies of modern code review show that most review comments concern maintainability, and only a small share point at functional defects [L3, L4].
- Scripted end-to-end tests break whenever selectors or page structure change, and keeping them working takes a lot of effort [L11, L12]. Small teams therefore often test manually or not at all.
- Static analysers find only part of real defects, and developers ignore them because of false positives [L13, L14].

LLMs can now comment on code [L1, L2, L5] and operate a web browser from a natural-language goal [L9, L10]. Existing products, however, cover **either** review **or** testing, keep the findings in separate tools, and seldom publish how accurate they are (see [04-existing-systems.md](04-existing-systems.md)).

**The problem:** there is no single, measurable tool that uses LLMs for both static (code) and dynamic (running application) quality checks and presents the defects found by both in one report.

## 1.2 Target users

| User | Need |
|---|---|
| Developer in a small team | Get review comments on a pull request in minutes, before a colleague reviews it |
| QA engineer / manual tester | Describe a scenario in plain language and have it executed, without writing scripts |
| Team lead | See all defects of a release in one place, ranked by severity |

Assumptions about these users are checked through user interviews ([02-user-research.md](02-user-research.md)).

## 1.3 Aim

To design, implement and evaluate a web platform that automates code review and web-application testing with large language models and joins their findings into a single defect report.

## 1.4 Objectives

| # | Objective | Owner | Result |
|---|---|---|---|
| O1 | Study users' review and testing practices | All | Interview findings, requirements |
| O2 | Review literature and existing systems | All | Docs 03, 04 |
| O3 | Design architecture and common defect format | Бекнұр | Doc 07, schema |
| O4 | Implement code-review service with GitHub integration | Бекнұр | Service + API |
| O5 | Implement browser-agent testing service | Бекнұр, Максот | Service + API |
| O6 | Implement web client, unified report, dashboard | Нариман | Web app |
| O7 | Build test bench with seeded defects | Максот | Bench repositories and apps |
| O8 | Evaluate accuracy vs. baseline | Максот | Precision / recall / F1 results |

## 1.5 Expected outcomes

1. A deployed web platform where a user connects a GitHub repository, starts a review of a pull request, runs natural-language test scenarios against a URL and reads one defect report.
2. A common defect format used by both services (type, severity, location in code or step, evidence: screenshot, log, diff hunk).
3. A test bench of at least 10 repositories / web apps with at least 100 seeded defects of known type.
4. Experimental results: precision, recall and F1 of code review, of testing, and of both combined, compared with a baseline (static analysers for code; scripted Playwright tests for UI).

## 1.6 Scope

**In scope**

- GitHub repositories, connected through a GitHub App (webhooks on pull requests).
- Review of pull-request diffs with repository context (changed files, related files).
- Web applications reachable by URL; scenarios written in English or Russian.
- OpenAI and Gemini models behind one provider interface.
- Unified report, run history, dashboard with experiment statistics.
- Evaluation bench and method.

**Out of scope**

- GitLab, Bitbucket, Azure DevOps.
- Native mobile applications.
- Performance, load and penetration testing.
- Training or fine-tuning own models.
- Billing and multi-organisation administration.

## 1.7 Engineering complexity

- Several independent services communicating through REST and an asynchronous job queue.
- Integration with an external platform (GitHub App, webhooks, checks / comments API).
- LLM engineering: prompt design, retrieval of repository context within token limits, structured (JSON-schema) output, cost control.
- Control of a real browser by an LLM agent: page-state extraction, action selection, recovery from failed actions.
- Experimental evaluation on a purpose-built bench with statistical comparison.

## 1.8 Milestones

| Date | Milestone |
|---|---|
| 24 Oct 2026 | Pre-defense 1: analysis, design, plan (this submission) |
| <!-- TODO --> | Pre-defense 2: working code-review and testing services, report, first experiments |
| <!-- TODO --> | Final pre-defense: full evaluation, explanatory note |

Detailed plan: [10-project-management.md](10-project-management.md).
