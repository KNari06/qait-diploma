# 4. Existing System Comparison

## 4.1 Selected systems

Five systems were chosen. Each covers at least one of the platform's two functions:

| System | Category | Why selected |
|---|---|---|
| **CodeRabbit** | LLM pull-request review | Widely used LLM reviewer for GitHub / GitLab |
| **GitHub Copilot code review** | LLM pull-request review | Built into GitHub, the platform our users already use |
| **SonarQube / SonarCloud** | Static analysis | Common non-LLM quality gate; represents the baseline approach |
| **testRigor** | AI / natural-language E2E testing | Tests written in plain English, no locators |
| **QAIT.dev** | AI web and mobile testing | Team members' prior work; LLM-based test recording and execution |

Mabl and Applitools were also considered, but they were not added as separate rows. Mabl is close to testRigor (low-code E2E with self-healing). Applitools is focused on visual comparison.

## 4.2 Criteria

| # | Criterion | Why it matters (source) |
|---|---|---|
| C1 | Reviews code changes (pull requests) | Static quality check |
| C2 | Tests the running web application | Dynamic quality check |
| C3 | Tests defined in natural language, no locators | Locator breakage is the main cause of test failures [L11] |
| C4 | Uses repository context beyond the diff | Context is the main difficulty for models [L17] |
| C5 | Single report joining code and UI findings | Interviews (to be confirmed, see doc 02) |
| C6 | Evidence per finding (location, screenshot, log) | Trust and false positives [L13] |
| C7 | Published / measurable accuracy | Users cannot judge trust otherwise [L6, L7] |
| C8 | Choice of LLM provider | Cost and data policy of the team |
| C9 | Self-hosting possible | Data stays in the company's infrastructure |

## 4.3 Comparison

Legend: ✔ supported · ◐ partial · ✘ not supported · ? not documented

| Criterion | CodeRabbit | Copilot review | SonarQube | testRigor | QAIT.dev | **Proposed platform** |
|---|---|---|---|---|---|---|
| C1 PR review | ✔ | ✔ | ◐ rule-based | ✘ | ✘ | ✔ |
| C2 Web app testing | ✘ | ✘ | ✘ | ✔ | ✔ | ✔ |
| C3 Natural-language tests | ✘ | ✘ | ✘ | ✔ | ✔ | ✔ |
| C4 Repository context | ✔ | ◐ | ✔ (rules over whole project) | n/a | n/a | ✔ |
| C5 Unified code + UI report | ✘ | ✘ | ✘ | ✘ | ✘ | ✔ |
| C6 Evidence per finding | ✔ (code) | ✔ (code) | ✔ (code) | ✔ (screens) | ✔ (screens, logs) | ✔ (both) |
| C7 Published accuracy | ✘ | ✘ | ◐ (rule docs) | ✘ | ◐ (own study) | ✔ (this project's evaluation) |
| C8 LLM choice | ✘ | ◐ (model picker) | n/a | ✘ | ◐ | ✔ |
| C9 Self-hosting | ◐ (enterprise) | ✘ | ✔ | ✘ | ✘ | ✔ (Docker) |

> Team note: each cell must be backed by the vendor's documentation, a trial account or a screenshot. Add the link and the date checked in 4.5 before the pre-defense. Change any cell that cannot be confirmed to "?".

## 4.4 Conclusions

- No system in the sample covers **both** code review and UI testing. A team has to buy and configure two tools and read two reports (C1 + C2 + C5).
- LLM review tools do not publish accuracy on a controlled defect set (C7). Static analysers have well-defined rules but low recall on real bugs [L14].
- Natural-language E2E tools solve locator brittleness (C3), but they are closed SaaS with no choice of model (C8, C9).
- **Need for the proposed solution:** one tool that runs both checks, produces one ranked defect report with evidence, and states its own measured accuracy.

## 4.5 Evidence log

| System | Source | Checked on | By |
|---|---|---|---|
| CodeRabbit | https://docs.coderabbit.ai | <!-- date --> | <!-- name --> |
| GitHub Copilot code review | https://docs.github.com/en/copilot | | |
| SonarQube | https://docs.sonarsource.com | | |
| testRigor | https://testrigor.com/docs | | |
| QAIT.dev | https://qait.dev | | |
