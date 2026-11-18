# company-scout

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Agent Skills](https://img.shields.io/badge/Standard-Agent_Skills-blueviolet.svg)](https://agentskills.io/)
[![Works with](https://img.shields.io/badge/Works_with-Claude_Code_|_Codex_|_Cursor_|_Gemini_CLI-blue.svg)](#install)

> **Research a company from public sources only — and be able to prove every sentence.**

Ask an agent about a company and you get a fluent paragraph that sounds right. Some of it is real, some of it is the model filling gaps, and you can't tell which is which. That's the problem this skill exists to solve.

`company-scout` takes **one company name** and produces a report where **every factual claim carries a public URL, a source grade, and the date it was checked**. Anything that can't be sourced doesn't get smoothed over — it gets demoted to a labelled hypothesis or listed under "insufficient evidence".

## What you get

A report structured along six axes, written so that someone who knows nothing about the company can follow it start to finish:

**company → products → customers → financial signals → problems → related companies**

Plus, at the end of every section, three follow-up questions worth asking next — and a section stating plainly what public sources can and cannot tell you about this company.

## The rules it follows

**Sources are graded, and the grade constrains what the claim may be.**

| Grade | What counts | How it may be used |
|---|---|---|
| **A** | Official site, job postings, engineering blog, press releases, regulatory filings, government and standards documents | Factual claims |
| **B** | Reputable business and tech press | Factual claims |
| **C** | Community posts, personal blogs, YouTube, app store reviews, wikis | **Leads only — never a factual assertion** |

**Facts and hypotheses never mix.** A confirmed fact is tagged `[fact]`. An inference is tagged `[hypothesis]` and must say "likely" or "estimated". The report is rejected if the two blur together.

**Numbers get a source and a fiscal year, or they don't appear.** When two sources disagree, both are shown and flagged for re-checking. Dead links get demoted, not quietly kept.

**At least three concrete problems, each with evidence.** Sourced from job postings that name a challenge, engineering blog posts describing hard problems, financial signals that look off (revenue up but operating profit down), and recurring complaints in reviews as leads. Each one is scored on whether it's actually tractable for an agent to work on: data availability, how well-bounded the problem is, and whether it can be expressed as a tool.

## Install

This follows the open [Agent Skills](https://agentskills.io/) standard, so it works in Claude Code, OpenAI Codex, Cursor, Gemini CLI, GitHub Copilot, VS Code, OpenCode, Goose, and other compatible agents.

**Claude Code:**

```
/plugin marketplace add aslla77/company-scout
/plugin install company-scout@aslla-skills
/reload-plugins
```

**Any agent:**

```bash
git clone --depth 1 https://github.com/aslla77/company-scout.git
cp -R company-scout/skills/company-scout ~/.claude/skills/    # or your agent's skills path
```

Then just name a company:

```
/company-scout Stripe
```

The report is written to `out/<company>-dossier-YYYYMMDD.md`.

## Before you use it

**It is written in Korean, and it leans Korean.** The instructions themselves are Korean (the frontmatter description that agents read is English). More importantly, the research strategy assumes Korean sources — it reaches for DART regulatory filings for financials and Korean business press for coverage. It still works on a foreign company, but the financial-signal section will be much thinner unless you tell it which filing system to use instead.

**Public sources only, by design.** It will not speculate to fill a gap. If a company is private and quiet, you will get a short report with an honest "insufficient evidence" list rather than a confident-sounding one. That's the intended behaviour, not a failure.

**Source quality depends on your agent's web access.** No browsing, no sources, no report.

**A few lines are left over from the contest this was built for.** The skill was written for a Korean AI competition, and two spots still reference it — an optional five-question section in the output schema, and a rule about avoiding one specific pre-announced problem. Ignore or delete them; nothing else depends on them.

**There is no bundled example.** The original was tuned against a golden example report that isn't published here. It works without one, but if you run it once and like the output, saving that report to `examples/` will make later runs more consistent.

## License

MIT — see [LICENSE](LICENSE).

## References

- [Agent Skills — the open standard](https://agentskills.io/)
- [Agent Skills — specification](https://agentskills.io/specification)
- [Claude Code — skills documentation](https://code.claude.com/docs/en/skills)
- [OpenAI Codex — skills documentation](https://developers.openai.com/codex/skills/)
