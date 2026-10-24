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
| C1 PR review | ✔ | ✔ | ◐ rule-based PR analysis + LLM fix suggestions (AI CodeFix) | ✘ | ✘ | ✔ |
| C2 Web app testing | ✘ | ✘ | ✘ | ✔ | ✔ | ✔ |
| C3 Natural-language tests | ✘ | ✘ | ✘ | ✔ | ✔ | ✔ |
| C4 Repository context | ✔ | ◐ (instruction files: `copilot-instructions.md`, `AGENTS.md`) | ✔ (rules over whole project) | n/a | n/a | ✔ |
| C5 Unified code + UI report | ✘ | ✘ | ✘ | ✘ | ✘ | ✔ |
| C6 Evidence per finding | ✔ (code) | ✔ (code) | ✔ (code) | ✔ (screens) | ✔ (screens, logs) | ✔ (both) |
| C7 Published accuracy | ✘ (none found in docs) | ✘ (none found in docs) | ◐ (rule docs) | ✘ (vendor marketing claims only) | ◐ (team's paper vs. Playwright) | ✔ (this project's evaluation) |
| C8 LLM choice | ◐ (only self-hosted: bring your own model) | ✘ (model switching not supported for code review) | n/a | ? | ◐ (OpenAI / Gemini) | ✔ |
| C9 Self-hosting | ◐ (Enterprise, 500+ seats) | ✘ | ✔ (SonarQube Server) | ◐ (on-premise mentioned by third-party listings, not in public docs) | ✘ | ✔ (Docker) |

Cells were checked against public vendor documentation on 24 Oct 2026 (see 4.5). Absence (✘) means the feature or figure was not found in public documentation. QAIT.dev cells are based on the team's own work on the product.

## 4.4 Conclusions

- No system in the sample covers **both** code review and UI testing. A team has to buy and configure two tools and read two reports (C1 + C2 + C5).
- LLM review tools do not publish accuracy on a controlled defect set (C7). Static analysers have well-defined rules but low recall on real bugs [L14].
- Natural-language E2E tools solve locator brittleness (C3), but they are mostly SaaS without a documented choice of model (C8, C9).
- LLM review tools hide or restrict model choice: Copilot code review does not support model switching, and CodeRabbit allows its own model only in self-hosted Enterprise (C8).
- **Need for the proposed solution:** one tool that runs both checks, produces one ranked defect report with evidence, and states its own measured accuracy.

## 4.5 Evidence log

| System | Cells | Source | Checked on | By |
|---|---|---|---|---|
| CodeRabbit | C1, C4, C8, C9 | Self-hosted overview: https://docs.coderabbit.ai/self-hosted/overview | 24 Oct 2026 | Қайдар Н.Н. |
| GitHub Copilot code review | C1, C4, C8 | Concepts — code review: https://docs.github.com/en/copilot/concepts/agents/code-review ; tutorial "Use Copilot code review across the pull request lifecycle": https://docs.github.com/en/copilot/tutorials/use-copilot-code-review-across-the-pull-request-lifecycle | 24 Oct 2026 | Қайдар Н.Н. |
| SonarQube | C1, C4, C6, C9 | Pull request analysis: https://docs.sonarsource.com/sonarqube-cloud/analyzing-source-code/pull-request-analysis ; AI CodeFix: https://www.sonarsource.com/blog/ai-codefix-sonar | 24 Oct 2026 | Қайдар Н.Н. |
| testRigor | C2, C3, C6, C9 | Features: https://testrigor.com ; deployment options per third-party listing (Gartner Peer Insights): https://www.gartner.com/reviews/product/testrigor | 24 Oct 2026 | Қайдар Н.Н. |
| QAIT.dev | all | Team's own work on the product; https://qait.dev | 24 Oct 2026 | Таниберген Б.Б. (to confirm) |

Open points: testRigor model choice (C8) and on-premise details (C9) are not described in public docs and should be asked from the vendor.
