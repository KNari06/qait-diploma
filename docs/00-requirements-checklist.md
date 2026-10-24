# Pre-defense 1: Requirements Checklist

How this submission covers the components and the Git repository requirements set for 24 October.

Status: ✅ done · ⚠️ partial · ⏳ pending

## A. Components

| № | Component | What the committee evaluates | Where in this submission | Status |
|---|---|---|---|---|
| 1 | Project proposal | Whether the problem is clearly defined, the scope is realistic, and the project involves sufficient engineering complexity | Section 1: problem 1.1, users 1.2, aim 1.3, objectives 1.4, outcomes 1.5, scope 1.6, complexity 1.7 | ✅ |
| 2 | Evidence of the relevance | What evidence informs the requirements and whether verified findings are distinguished from assumptions | Section 2: assumptions A1–A7, interview guide, proto-personas (hypothetical) 2.5, evidence from literature and existing systems 2.6, interview findings 2.7 | ⚠️ the 3–5 interviews are not conducted yet; every assumption is marked as supported or unverified |
| 3 | Literature review | What approaches exist, what is already known, and which limitations the project takes into account | Section 3: 20 sources (18 peer-reviewed, most from 2020–2025, foundational works kept), analysis table 3.2, synthesis 3.3, gaps 3.4 | ✅ (reference check in progress, QAIT-3) |
| 4 | Existing system comparison | Why the proposed solution is needed and whether claims about existing systems are supported by evidence | Section 4: 5 systems × 9 criteria 4.3, conclusions 4.4, evidence log with sources and dates 4.5 | ✅ |
| 5 | Technology comparison | Whether technology choices are justified by the project requirements | Section 5: 6 key decisions (client, backend, storage, service interaction, browser automation, LLM), 3 alternatives each, weighted criteria linked to requirements | ✅ |
| 6 | Requirements specification | Whether the fulfilment of each mandatory requirement can be verified | Section 6: 30 functional + 11 non-functional requirements, MoSCoW priorities, acceptance criterion for every Must, source of each requirement, traceability 6.4 | ✅ |
| 7 | Architecture and diagrams | Whether requirements, components, data, and scenarios are aligned | Section 7: system context 7.1, use case diagram with requirement mapping 7.2, component diagram (draft) with owners 7.3, data model 7.4, two sequence diagrams 7.5, architectural decisions 7.6; editable PlantUML sources in `diagrams/src` | ✅ |
| 8 | UX / prototype | Whether it is clear how users or external systems will interact with the solution | Section 8: visual starting point, screen flows for key scenarios S1–S5, example API | ⚠️ mockups of review, report and dashboard screens pending |
| 9 | Verification and evaluation plan | How the achievement of the project aim will be demonstrated | Section 9: test levels 9.1, research questions, test bench, matching rule, metrics, baselines, conditions, hypotheses 9.2, threats to validity 9.3 | ✅ |
| 10 | Project management and risks | Whether project activities are traceable and the next steps are clearly defined | Section 10: Git repository and YouTrack 10.1, responsibilities 10.2, milestones 10.3, backlog 10.4, risks 10.5, dependencies 10.6; Appendix B: YouTrack backlog | ✅ |
| 11 | Supervisor feedback | Whether the supervisor reviewed the substance of the work and whether that review led to improvements | Section 11: feedback log | ⏳ to be filled after the supervisor's review |

## B. Minimum requirements for 24 October

| Requirement | Where | Status |
|---|---|---|
| A completed project README | `README.md` (Appendix A) | ✅ |
| Project proposal and scope | Section 1 | ✅ |
| Literature review, existing system comparison, and technology comparison | Sections 3, 4, 5 | ✅ |
| Initial requirements and acceptance criteria | Section 6 | ✅ |
| Required architectural diagrams, including editable source files | Section 7; `diagrams/src/*.puml`, `diagrams/export/*.svg/png` | ✅ |
| Prototype materials or a link to the prototype | Section 8 | ⚠️ |
| Verification and evaluation plan | Section 9 | ✅ |
| A link to YouTrack with assigned and prioritized tasks | https://qait-diploma.youtrack.cloud/projects/QAIT (24 prioritized tasks; owner named in each task) | ⚠️ assignment to member accounts pending |
| Evidence of progressive contributions from each team member | Commit history and pull requests | ⚠️ contributions of all members in progress |

## C. Git repository requirements

| Step | Requirement | How it is met | Status |
|---|---|---|---|
| 1 | Create the project repository | https://github.com/KNari06/qait-diploma (private) | ✅ |
| 2 | Provide access and identify contributors | Team members invited with write access; supervisor and committee access to be given before the pre-defense | ⚠️ |
| 3 | Prepare the README | All required sections present | ✅ |
| 4 | Organize the repository | `src/`, `docs/`, `diagrams/`, `tests/`, `config/`, `.gitignore`, `.env.example` with placeholders | ✅ |
| 5 | Record work progressively | One commit per deliverable, descriptive messages | ✅ |
| 6 | Link changes to YouTrack tasks | Task ID in every commit, branch name and pull request (e.g. `QAIT-4-evidence-log`) | ✅ |
| 7 | Use branches and pull requests | Changes go through pull requests with a reviewer from the team | ✅ |
| 8 | Tag each pre-defense submission | Tag `pre-defense-1` set on the submitted version | ⏳ after open pull requests are merged |
| 9 | Demonstrate the repository | Trace example: YouTrack QAIT-4 → branch `QAIT-4-evidence-log` → pull request #1 → `docs/04-existing-systems.md` | ✅ |
