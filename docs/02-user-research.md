# 2. Evidence of Relevance: User Research

Status: **guide ready; proto-personas (2.5) and evidence from literature and prior work (2.6) ready; interviews to be conducted (2.7).**

## 2.1 Goal

Check the assumptions behind the requirements:

| ID | Assumption | Requirements that depend on it |
|---|---|---|
| A1 | Code review delays merging in small teams | FR-CR1, NFR-P1 |
| A2 | Teams have few or no automated UI tests because scripts are costly to maintain | FR-T1, FR-T2, FR-T8 |
| A3 | Review and test results live in different tools and are hard to see together | FR-R2 |
| A4 | Users will not trust AI findings without evidence and a known accuracy | FR-CR4, FR-T3, FR-T7, FR-R3, FR-R5 |
| A5 | Teams are willing to send code to a cloud LLM (or need a self-hosted option) | FR-CR7, FR-P5, NFR-S1 |
| A6 | When a test fails, users need a plain-language reason with a screenshot, not a stack trace | FR-T3 |
| A7 | Cost per run must be visible and limited | NFR-C1, FR-P6, FR-T5 |

## 2.2 Participants

3–5 people. Participants are anonymised and referred to as P1…P5 with role and context only.

Target profile: a developer, QA engineer or team lead in a team of ≤ 20 people who personally performs code review or testing. Classmates without work experience and supervisors or teachers do not qualify solely through their academic role.

Possible sources: QAIT.dev pilot clients, companies where team members have worked or interned, developers and QA engineers in Astana IT companies.

## 2.3 Protocol

- 15–20 minutes, online, by voice call or in person, in Kazakh, Russian or English.
- Ask for oral consent to take notes. No recording without separate consent.
- Write notes on the same day; record the actual interview date, duration, interviewer and consent.
- Ask about current work and specific recent examples before introducing QAIT. Use follow-up prompts selectively to fit the time available.
- Record negative answers as well as positive ones. Missing information is **unclear**, not a rejection or confirmation. Separate what the participant said or showed from the interviewer's interpretation.

### Core questions

1. **Context:** What is your role, how large is your team, and what do you build? Which review or testing tasks do you personally perform?
2. **Code review (A1):** How does code review work? How many PRs are waiting, and how long do they wait? Walk me through a recent PR: did review delay merging or release?
3. **Review outcomes (A4):** What problems did review catch recently, and what slipped through? Can you give a concrete example of each?
4. **UI testing (A2):** How did you test the interface before your last release? Which scenarios were manual or automated, how long did it take, and how many people were involved?
5. **Automation and diagnosis (A2, A6):** If you have UI tests, how many failed without a product bug during the last release or another stated period, out of how many runs? Who fixes them? Walk me through the last failed run: what evidence did you use and how long did diagnosis take? If you have no UI tests, what prevents you from adding them?
6. **Reporting (A3):** Where are defects, review findings and test results recorded? How many tools do you use? Describe a recent case where information was hard to find or transfer, if any.
7. **Trust (A4, A6):** What evidence would let you check an AI-reported defect? When would you reject its conclusion? If you have used AI tools, describe a recent finding you accepted or rejected and why.
8. **Data policy (A5):** Are you allowed to send code, DOM, screenshots or logs to an external AI provider? What must be excluded, who approves this, and where are the rules recorded? If the policy is unknown, record that rather than assuming permission.

### Follow-up and concept discussion

Only after the core questions, briefly explain QAIT as a proposed tool for code review and browser-based testing with linked evidence. Do not promise accuracy or time savings.

- **Scenario authoring (PP3):** How would you prefer to create a scenario? What would you need to inspect or edit before running AI-generated steps? Which language would you use?
- **Element choices (PP1, A4):** If an agent selects a different element after a page change, what would you need to see to judge that choice? What should happen if the intended element is missing?
- **Cost (A7):** How do you currently track and limit spending on testing or AI tools? Have unexpected costs occurred? What per-run limit or approval would you need? Record any proposed price as a stated preference, not a purchase commitment.
- **Workflow fit (A3):** Which existing workflow or integration would this need to fit? What would prevent adoption? A positive reaction to the concept does not confirm a current workflow problem.

## 2.4 Interview notes template

```
Participant: P_  | Role: ______ | Team size: __ | Domain: ______ | Date: ____ | Interviewer: ____
Format / actual duration: ______ | Consent to notes: ______ | Notes written on: ______
Key quotes (verbatim, short):
Observations (what the participant said or showed; include recent examples):
Interviewer interpretation / open questions:
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
- **Expected concerns:** a tool that turns a real bug green by acting on the wrong element; safety of test credentials and rules for sending code, DOM, screenshots and logs to external models; LLM cost per run and a spending cap.
- **Assumptions:** A2, A4, A5, A6, A7 · **Requirements:** FR-T1–T3, FR-T5, FR-T7, FR-P5, FR-P6, NFR-S1, NFR-C1

### PP2: CTO of a small team without QA

- **Context:** 10–15 developers, no QA role; developers write unit tests, the product owner checks end-to-end flows by hand before release; some bugs reach users.
- **Expected pains:** hiring a test-automation engineer is expensive; nobody owns UI testing.
- **Expected needs:** checks run automatically on every pull request; one place to see what broke; notifications where the team already talks.
- **Expected concerns:** unpredictable cost and the need for a per-run spending cap; time to set up.
- **Assumptions:** A1, A3, A7 · **Requirements:** FR-CR1, FR-R2, FR-P6, NFR-U1, NFR-C1

### PP3: Manual tester moving towards automation

- **Context:** 1–3 years of manual testing; started learning Selenium / JavaScript but has no time between projects.
- **Expected pains:** high entry barrier to automation.
- **Expected needs:** write or edit test steps in their own language, see each step, and fix steps the tool misunderstood; failures explained in words with a screenshot, not a stack trace.
- **Expected concerns:** being replaced; expects the tool to take over routine work, not the judgement of what to test.
- **Assumptions:** A2, A6 · **Requirements:** FR-T1, FR-T3, FR-T4, FR-T8

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
