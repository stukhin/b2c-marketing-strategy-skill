# Marketing Strategy Audit Skill

A Claude Skill that audits a marketing strategy document — your own draft or someone else's — against an objective 7-criterion rubric, and surfaces non-obvious insights the document itself doesn't articulate. Output is a single interactive HTML dashboard with scorecard, gap list, recommendations, and a "Beyond the document" section.

> Status: **v1.0**. First release. Companion to [`b2c-marketing-strategy`](../b2c-marketing-strategy/) in the same repo.

---

## What it does

You upload one or several documents (PDF, PPTX, DOCX, plain text). The skill:

1. **Parses** each document and identifies what type it is (annual marketing plan / brand strategy / pitch deck / campaign brief / channel strategy)
2. **Scores** the document on 7 criteria — Specificity, Verification, Risk register quality, Action specificity, Synthesis integrity, Reality check, Completeness
3. **Identifies gaps** with priorities (high / medium / low) grouped by criterion
4. **Recommends fixes** for high-priority gaps (Full audit mode only)
5. **Surfaces "Beyond the document" insights** — what we noticed across the documents and against external signals that the document doesn't articulate: cross-doc inconsistencies, hidden assumptions, missed opportunities in the data, reality gaps via web search, strategic alternatives not considered, pattern detection, benchmark outliers
6. **Maps cross-document agreement / divergence** if multiple files are uploaded
7. **Reality-checks** claimed positioning vs observable signals (App Store reviews, Google Trends, recent press)

The output is **not** a structured summary of what's already in the files (you can read your own document). It's a strengthening tool that says where the document is solid, where it's exposed, and what's worth adding.

---

## Audience

In priority order:

1. **In-house marketers** — stress-testing your own draft before presenting to leadership
2. **Marketing leads / directors** — reviewing what agencies or internal teams bring you
3. **Consultants** — third-party audits as a service

The tone is **mild, formative, strengthening** — "here's how to make this stronger" not "this is wrong".

---

## Two modes

| Mode | Time | What you get |
|---|---|---|
| **Quick score** | ~10 min interview, ~30–45 min total | Scorecard + gap list. 5 criteria scored (Reality check + outside-the-box research skipped). 4 of 7 insight types run. |
| **Full audit** | ~30–45 min interview, can take a couple of hours total | Everything: 7 criteria + 7 insight types + Reality check via web search + Strategic alternatives + Benchmark outliers + Recommendations. |

Quick score is the right pick for a first read on your own draft. Full audit is for serious reviews — agency proposals, third-party work, strategies going to a board.

---

## Output language

English (default) or Russian, picked at the start. Same two-language scope as the companion skill — entire dashboard renders in selected language.

---

## How to install

You need [Claude Code](https://claude.ai/download), free.

1. Download the latest `marketing-strategy-audit.skill` from the Releases page
2. Drag it into Claude Code, or place it in `~/.claude/skills/`
3. Invoke with `/marketing-strategy-audit`, or just ask: *"Audit this marketing strategy"* + attach files

Permissions: same setup as the main `b2c-marketing-strategy` skill. See that README's "Before the first run — set up permissions" section.

---

## How to use

1. Start a new Claude Code session
2. Type `/marketing-strategy-audit` or describe what you need
3. Pick mode (Quick / Full) + output language (English / Russian)
4. Attach the document(s) to be audited
5. Answer two short context questions (brand + category — needed for Reality check and alternative-strategy lookups)
6. Wait for parsing → scoring → insight extraction → dashboard generation
7. Output appears in `output/<brand>-audit-v1.html` (or `<short-name>-audit-v1.html` if no clear brand)

---

## What this skill does NOT do

- It does not **write** a strategy — that's what the companion `b2c-marketing-strategy` skill is for
- It does not **rewrite** or **fix** your document — it identifies gaps; closing them is your job
- It does not **judge** the strategy as good or bad — it scores against an objective rubric and lets you draw conclusions
- It does not **invent** insights — every finding traces back to a specific document fact, a cross-doc divergence, a web search hit, or a category benchmark

---

## Author

Created and maintained by **Alexander Styukhin**.

LinkedIn: [linkedin.com/in/stukhin](https://www.linkedin.com/in/stukhin/)

If you spot a failure mode or have category-specific feedback, ping me on LinkedIn — real-world feedback drives improvements here.

---

## License

[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/) — same as the companion skill. Use freely for non-commercial purposes with attribution.

---

## Repository structure

```
marketing-strategy-audit/
├── SKILL.md                              The workflow brain
├── README.md                             You are here
└── references/
    ├── audit-dashboard-template.html     ~600-line HTML template for the audit output
    ├── audit-criteria.md                 7 criteria — deep definitions, scoring anchors, failure patterns
    ├── insight-types.md                  7 outside-the-box insight types with examples
    ├── audit-rubric.md                   0-100 scoring bands per criterion
    └── audit-self-check.md               Python self-check snippet for Step 7
```
