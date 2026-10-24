# Pre-defense 1: Submission Report

| | |
|---|---|
| **Project** | Development of an AI platform for software quality assurance automation based on large language models |
| **Team** | Қайдар Н.Н., Таниберген Б.Б., Сариев М.Б. (SE-2406) |
| **Supervisor** | Айтмұханбетова Э.А. |
| **Repository** | https://github.com/KNari06/qait-diploma |
| **Tag** | `pre-defense-1` (set after the open pull requests are merged) |
| **YouTrack** | https://qait-diploma.youtrack.cloud/projects/QAIT (24 tasks, QAIT-1 … QAIT-24) |
| **Date** | 24 Oct 2026 |
| **PDF** | [QAIT_PreDefense1_Documentation.pdf](QAIT_PreDefense1_Documentation.pdf): all documents in one file |

## 1. Completed work

| # | Component (stage requirement) | Deliverable | Status |
|---|---|---|---|
| 1 | Project proposal | [01-proposal.md](01-proposal.md): problem, users, aim, 8 objectives, outcomes, scope, complexity | Done |
| 2 | Evidence of relevance | [02-user-research.md](02-user-research.md): 7 assumptions, interview guide and template, hypothetical proto-personas, evidence from literature and existing systems | **Partial**: interviews not yet conducted |
| 3 | Literature review | [03-literature-review.md](03-literature-review.md): 20 sources (18 peer-reviewed, most 2020–2025), analysis table, synthesis, gaps | Done; reference check in progress (QAIT-3) |
| 4 | Existing system comparison | [04-existing-systems.md](04-existing-systems.md): 5 systems × 9 criteria, evidence log | Done (PR #1 in review) |
| 5 | Technology comparison | [05-technology-comparison.md](05-technology-comparison.md): 6 decisions × 3 alternatives, weighted scoring | Done |
| 6 | Requirements | [06-requirements.md](06-requirements.md): 30 functional (4 of them provisional, from proto-personas; PR #3) + 11 non-functional requirements, MoSCoW, acceptance criteria, traceability | Done |
| 7 | Architecture and diagrams | [07-architecture.md](07-architecture.md); context, use case, component, ER, 2 sequence diagrams: sources in `diagrams/src` (PlantUML), exports in `diagrams/export`; defect JSON schema in `src/shared` | Done (component diagram is a draft) |
| 8 | UX / prototype | [08-ux-prototype.md](08-ux-prototype.md): QAIT.dev screen as a visual starting point, screen flow S1–S5, mockups of review (S2), unified report (S4) and dashboard (S5), example API | Done (PR #4) |
| 9 | Verification and evaluation plan | [09-verification-plan.md](09-verification-plan.md): test levels, 4 research questions, test bench (≥100 seeded defects), metrics, baselines, hypotheses, threats | Done |
| 10 | Project management and risks | [10-project-management.md](10-project-management.md): roles, 7 milestones, 24-task backlog in YouTrack, 11 risks, dependencies | Done |
| 11 | Supervisor feedback | [11-supervisor-feedback.md](11-supervisor-feedback.md): log template | **Pending**: to be filled after this review |

Repository setup: README with all required sections, `docs/`, `diagrams/`, `src/`, `tests/`, `config/.env.example`, `.gitignore`. Commits reference YouTrack task IDs, and changes go through pull requests.

## 2. Known limitations

- **User research:** the 3–5 interviews required for component 2 have not been conducted yet. Requirements currently rest on the literature, existing systems and the team's assumptions, and doc 02 marks each assumption as supported or unverified.
- **Contributions:** most commits so far are by one member. The other members' pull requests (QAIT-2, QAIT-3) are in progress.
- **Prior work:** the relation between the diploma scope and the existing QAIT.dev product still has to be stated in the README (QAIT-2).
- **YouTrack:** tasks are imported but not yet assigned to members (accounts pending).
- No runnable code is part of this submission (not required at this stage).

## 3. Outstanding tasks

| Task | Owner | Due |
|---|---|---|
| Conduct and record 3–5 interviews; update requirements (QAIT-2) | Қайдар Н.Н. | <!-- date --> |
| Verify all literature references (QAIT-3) | Сариев М.Б. | <!-- date --> |
| Describe reuse of QAIT.dev; add pilot observations (QAIT-2) | Таниберген Б.Б. | <!-- date --> |
| Record supervisor feedback and actions (doc 11) | All | after review |
| Assign YouTrack tasks; give committee access to repositories | Қайдар Н.Н. | before pre-defense |
| Next stage: skeleton, CI, auth (M2, QAIT-10 … 13) | Таниберген Б.Б., Қайдар Н.Н. | 15 Nov 2026 |

## 4. Questions for the supervisor

1. Is the scope (code review + browser-agent testing + unified report) right for a 3-person diploma, or should the evaluation focus on one of the two parts?
2. Can prior work on QAIT.dev be reused as a base, and how should this be presented?
3. Which interviewees would be acceptable for component 2 (QAIT.dev pilot clients, industry contacts)?
4. Are the evaluation hypotheses H1–H4 in doc 09 acceptable as the research contribution?
