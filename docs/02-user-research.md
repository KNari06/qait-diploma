# 2. Evidence of Relevance: User Research

Status: **guide ready; proto-personas (2.5) and evidence from literature and prior work (2.6) ready; interviews to be conducted (2.7).**

## 2.1 Goal

Check the assumptions behind the requirements:

| ID | Assumption | Requirements that depend on it |
|---|---|---|
| A1 | Code review delays merging in small teams | FR-CR1, NFR-P1 |
| A2 | Teams have few or no automated UI tests because scripts are costly to maintain | FR-T1, FR-T2 |
| A3 | Review and test results live in different tools and are hard to see together | FR-R2 |
| A4 | Users will not trust AI findings without evidence and a known accuracy | FR-CR4, FR-T3, FR-R3, FR-R5 |
| A5 | Teams are willing to send code to a cloud LLM (or need a self-hosted option) | FR-CR7, NFR-S1 |
| A6 | When a test fails, users need a plain-language reason with a screenshot, not a stack trace | FR-T3 |
| A7 | Cost per run must be visible and limited | NFR-C1, FR-T5 |

## 2.2 Participants

3–5 people. Participants are anonymised and referred to as P1…P5 with role and context only.

Target profile: a developer, QA engineer or team lead in a team of ≤ 20 people.

Possible sources: QAIT.dev pilot clients, companies where team members have worked or interned, developers and QA engineers in Astana IT companies.

## 2.3 Protocol

- 20–30 minutes, online or in person, in Kazakh, Russian or English.
- Ask for oral consent to take notes. No recording without consent.
- Notes are written within 24 hours, in the template below.

### Questions

**Context**
1. What is your role, and how large is your team? What do you build?
2. How does a change get from a developer to production in your team?

**Code review (A1, A4)**

3. Who reviews code, and how long does a pull request wait for review?
4. What kinds of problems do reviews usually catch? What slips through?
5. Have you tried AI review tools (Copilot, CodeRabbit…)? What did you think of them?

**Testing (A2)**

6. How do you test the user interface before a release? Manually or automated?
7. If you have automated UI tests: how often do they break without a real bug? Who fixes them?
8. If you don't: why not?

6a. How long does regression testing before a release take, and how many people does it need?
7a. How long does it take to find out why one failed run failed (bug, flaky test or environment)?
8a. Would you write tests in natural language? In which language (kk / ru / en)?

**Reporting (A3)**

9. Where do you see the bugs found before a release? How many tools is that?
9a. Which integrations are required (Jira, Telegram, Slack, CI)?

**Trust and constraints (A4, A5)**

10. What would an AI tool need to show for you to trust a defect it reports?
11. Could you send your code to a cloud AI provider? What restrictions do you have?
11a. What would a tool like this be worth to you, and in which model (subscription / per run)?

**Closing**

12. If one tool did review and UI testing and showed one report, would you use it? What would stop you?

## 2.4 Interview notes template

```
Participant: P_  | Role: ______ | Team size: __ | Domain: ______ | Date: ____ | Interviewer: ____
Key quotes (verbatim, short):
Observations:
Assumptions:  A1 confirmed / rejected / unclear — because …
              A2 …
              A3 …
              A4 …
              A5 …
              A6 …
              A7 …
New requirements or changes:
```

## 2.5 Proto-personas (hypothetical)

> These personas are **hypothetical**. The team wrote them from its own experience in QA and from the literature. They are **not interview data**. They describe whom the interviews should reach and what we expect to hear. Each one is checked against the real interviews in 2.7.

### PP1: QA lead in a product company

- **Context:** leads 5–7 testers, mostly manual regression of 300+ cases before each release; two automation engineers.
- **Expected pains:** regression takes days; scripted tests break when UI classes or structure change, so automation engineers spend much of their time fixing tests; triaging a failed run (bug, flaky test or environment) is slow.
- **Expected needs:** testers who know the product write automated scenarios in plain language; every automatic "fix" or element choice is shown with its reason and confidence; failed runs come with the failing request, log and screenshot.
- **Expected concerns:** a tool that turns a real bug green by acting on the wrong element; safety of test credentials; LLM cost per run.
- **Assumptions:** A2, A4, A6, A7 · **Requirements:** FR-T1–T3, FR-T5, NFR-S1, NFR-C1

### PP2: CTO of a small team without QA

- **Context:** 10–15 developers, no QA role; developers write unit tests, the product owner checks end-to-end flows by hand before release; some bugs reach users.
- **Expected pains:** hiring a test-automation engineer is expensive; nobody owns UI testing.
- **Expected needs:** checks run automatically on every pull request; one place to see what broke; notifications where the team already talks.
- **Expected concerns:** unpredictable cost; time to set up.
- **Assumptions:** A1, A3, A7 · **Requirements:** FR-CR1, FR-R2, NFR-U1, NFR-C1

### PP3: Manual tester moving towards automation

- **Context:** 1–3 years of manual testing; started learning Selenium / JavaScript but has no time between projects.
- **Expected pains:** high entry barrier to automation.
- **Expected needs:** write or edit test steps in their own language, see each step, and fix steps the tool misunderstood; failures explained in words with a screenshot, not a stack trace.
- **Expected concerns:** being replaced; expects the tool to take over routine work, not the judgement of what to test.
- **Assumptions:** A2, A6 · **Requirements:** FR-T1, FR-T3, FR-T4

## 2.6 Evidence available before the interviews

Status of each assumption from published studies, existing products and the team's own work on QAIT.dev. None of this is interview data. It shows which assumptions have outside support and which still need users to confirm them.

| Assumption | Published evidence | Existing systems | Team observation (QAIT.dev pilots) | Status |
|---|---|---|---|---|
| A1 Review delays merging | Understanding the change is the main review difficulty [L3]; review speed is a core expectation [L4] | LLM reviewers sell on review time (doc 04) | <!-- TODO(Бекнұр): what pilot teams said about review --> | Supported by literature; not yet checked with users |
| A2 Few UI tests, scripts costly | Locators are the main cause of record/replay test breakage [L11]; robust locators help only partly [L12] | testRigor and QAIT.dev are built on this premise | <!-- TODO(Бекнұр): e.g. how many pilot teams had automated UI tests before --> | Supported by literature |
| A3 Results scattered across tools | No direct study found | No tool in doc 04 combines code and UI findings | <!-- TODO --> | **Unverified**: main question for the interviews |
| A4 Trust needs evidence and accuracy | False positives and poor explanations drive developers away from static tools [L13]; acceptance rises when output is verified [L20] | Review tools publish no accuracy figures (doc 04, C7) | <!-- TODO --> | Supported by literature |
| A5 Cloud LLM acceptable | No direct study | Most products are SaaS-only (C9) | <!-- TODO --> | **Unverified** |
| A6 Plain-language failure reasons | Explanations matter for tool adoption [L13] | QAIT.dev, testRigor give screenshots and step text | <!-- TODO --> | Partly supported |
| A7 Visible cost per run | No direct study | — | <!-- TODO --> | **Unverified** |

## 2.7 Interview findings

<!-- Fill in after the interviews. Keep verified findings (what participants said or showed) separate from team assumptions. -->

| Assumption | P1 | P2 | P3 | P4 | P5 | Verdict |
|---|---|---|---|---|---|---|
| A1 | | | | | | |
| A2 | | | | | | |
| A3 | | | | | | |
| A4 | | | | | | |
| A5 | | | | | | |
| A6 | | | | | | |
| A7 | | | | | | |

### Changes to requirements

<!-- e.g. "FR-R4 export raised from S to M after P2 and P4 said reports are sent to managers as PDF." -->
