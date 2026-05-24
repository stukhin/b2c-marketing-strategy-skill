# Marketing Strategy Skills for Claude Code

Two Claude Skills that turn a short interview (or an uploaded document) into a publish-ready marketing strategy artefact. Output is always a single interactive HTML dashboard, with a built-in `.docx` export button.

Latest release: **[v1.4.0](https://github.com/stukhin/b2c-marketing-strategy-skill/releases/latest)** · visual system unification + animation polish + cleanup.

---

## What's in this repo

This is a monorepo holding two related but independent Claude Skills:

### 📊 [`b2c-marketing-strategy`](./b2c-marketing-strategy/) · **writes annual marketing strategies** (v1.4.0)

Turns a 2 / 9 / 22-question interview into a 20-section interactive dashboard covering market context, consumers, competitors, brand health, SWOT, brand pyramid, positioning, audience × lifecycle, communication matrix, product initiatives, media, MMM (budget × ROAS), regional plan, communication plan, risk register, references, executive summary, basics, business goal, assumptions, next steps. Three modes (Fast / Guided / Deep), 16 industry presets, English or Russian output.

Full docs → [`b2c-marketing-strategy/README.md`](./b2c-marketing-strategy/README.md)

### 🔍 [`marketing-strategy-audit`](./marketing-strategy-audit/) · **audits existing strategy documents** (v1.0)

Takes uploaded strategy docs (PDF / PPTX / DOCX / plain text), scores them on a 7-criterion rubric (Specificity / Verification / Risk register quality / Action specificity / Synthesis integrity / Reality check / Completeness), and surfaces non-obvious insights the document itself doesn't articulate — cross-doc inconsistencies, hidden assumptions, missed data opportunities, reality gaps via web search.

Full docs → [`marketing-strategy-audit/README.md`](./marketing-strategy-audit/README.md)

---

## Quick install

You need [Claude Code](https://claude.ai/download) (free, available for macOS / Windows / Linux).

**Grab the latest `.skill` archives from Releases:**

- [`b2c-marketing-strategy.skill`](https://github.com/stukhin/b2c-marketing-strategy-skill/releases/latest/download/b2c-marketing-strategy.skill) (~156 KB)
- [`marketing-strategy-audit.skill`](https://github.com/stukhin/b2c-marketing-strategy-skill/releases/latest/download/marketing-strategy-audit.skill) (~32 KB)

Drag either file into Claude Code, or drop into `~/.claude/skills/`. Restart Claude Code if the skill doesn't appear.

Invoke with `/b2c-marketing-strategy` or `/marketing-strategy-audit`. Or simply ask in natural language: *"Generate a marketing strategy for [Brand] in [Country]"* / *"Audit this strategy deck"*.

**Pro tip — set up permissions before first run.** A full run touches dozens of files. Without pre-approved permissions, Claude Code will prompt "Allow?" 40+ times. Add these to Settings → Permissions:

- `Read(*)` · `Edit(output/**)` · `Bash(python3 -c *)` · `Bash(grep:*)` · `WebSearch`

One-time setup, then runs proceed without interruptions.

---

## What the output looks like

Screenshots from a real Fast-mode run — Chowdeck (Nigerian food delivery) 2026 strategy. Same dashboard, different sections.

### 1 · Consumer segmentation

![Consumer segmentation — donut charts + segment cards with sensitivities, voice quotes, brand chips](./docs/screenshots/01-consumers.png)

Revenue / GMV share and audience share donuts auto-derive from the segment-card data below. Each card carries reach %, sensitivities, voice-of-customer quote, preferred competitors, primary channels. Up to 8 segments in horizontal scroller.

### 2 · Brand pyramid + positioning maps

![Brand pyramid 5 tiers + 2 positioning maps with competitor dots](./docs/screenshots/02-brand-pyramid.png)

Purpose → Essence → Personality → RTBs → Features hierarchy, plus functional + emotional positioning maps with competitor placement.

### 3 · Competitive landscape

![Indirect competitors table — Bolt rides, Jumia, WhatsApp, Shoprite with overlap analysis](./docs/screenshots/03-competitors.png)

Direct + indirect competitor tables with sticky first column, parameter-by-parameter comparison, app ratings, monetization model, strategic risk per rival.

### 4 · Media plan

![Channel overview table with chip system + sidebar theme switcher](./docs/screenshots/04-channel-overview.png)

10+ channel rows with reach, funnel stage, KPI, and Plan-2026 direction chip (↑ Increase / → Hold / ↓ Decrease). Theme switcher visible in left sidebar — 4 themes flip instantly via View Transitions API.

### 5 · Assumptions + KPI tiles

![Assumptions cards with confidence levels + market KPI tiles](./docs/screenshots/05-assumptions.png)

3-tier confidence cards (High / Medium / Low) anchor what the year is betting on. Each card carries claim + leading signal + trigger (collapsed by default, click to expand). Market KPI tiles below show 4 macro signals with mini-charts.

---

## How it works (in 30 seconds)

1. Start a new Claude Code session, type `/b2c-marketing-strategy`
2. Pick a mode (Fast ~5min interview / Guided ~15-20min / Deep ~30-45min) and output language (EN / RU)
3. Answer the interview — brand + market, use case + stage, plus 7-20 prioritised fillers depending on mode
4. The skill runs a parallel research sweep via `web_search` (DataReportal, Statista, World Bank, Sensor Tower, industry press), then fills the 20-section dashboard
5. Self-check verifies the output is free of placeholder leaks
6. Open `output/<brand>-<market>-v1.html` in any browser

Every fact-looking number is plated: blue dot for verified external source (with T1/T2/T3 tier), `★` for hypothesis, `[to be confirmed]` for internal-only fact only you can provide. Nothing reads as fact without a source plate.

For wall-clock time + token cost expectations + Pro vs Max plan guidance, see the [skill-specific README](./b2c-marketing-strategy/README.md#token-cost-and-wall-clock-expectations).

---

## Versions

| Version | Date | Highlights |
|---|---|---|
| **v1.4.0** | 2026-05-24 | Visual system unification — chip / table / comm-matrix consolidation, View Transitions API on theme switch, smooth `<details>` animations |
| v1.3.2 | 2026-05-24 | Manrope font adoption + Chart.js label font fix |
| v1.3.1 | 2026-05-24 | Remove "Etc." stubs and `Add another` placeholders |
| v1.3.0 | 2026-05-24 | Fast-mode honesty pack — banner + confidence chips + Mode-aware depth caps |
| v1.2.1 | 2026-05-23 | Self-challenge fix-pass + cleanup duplicate root paths |
| v1.2.0 | 2026-05-23 | Monorepo restructure + add `marketing-strategy-audit` skill |
| v1.0–v1.1 | 2026-04 → 05 | Consolidation after 11 patches' worth of real-tester feedback |

Full release notes on each tag: [Releases page](https://github.com/stukhin/b2c-marketing-strategy-skill/releases).

---

## Repository structure

```
b2c-marketing-strategy-skill/
├── b2c-marketing-strategy/          ← Skill 1 — writes annual strategies (v1.4.0)
│   ├── SKILL.md                       Workflow brain
│   ├── README.md                      Detailed docs for this skill
│   ├── LICENSE                        CC BY-NC-SA 4.0
│   └── references/                    Template + presets + question banks
├── marketing-strategy-audit/        ← Skill 2 — audits existing strategy docs (v1.0)
│   ├── SKILL.md
│   ├── README.md
│   ├── LICENSE
│   └── references/
├── .gitignore                       Excludes archives + .skill + output/
└── README.md                        You are here
```

`.skill` archives + local `output/` directory are gitignored — built artefacts ship via GitHub Releases, generated dashboards stay local.

---

## License

Both skills released under [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/).

You may use, share, and adapt for **non-commercial purposes** with attribution. Any derivative work must be released under the same license. Commercial use requires explicit written permission from the author.

---

## Author

Built and maintained by **Alexander Styukhin** — Chief Marketing Officer with 15+ years across marketplaces, fintech, ride-hail, food delivery. The skills are a personal side-project: a way to turn the playbook I've used across hundreds of strategic planning cycles into something other CMOs can actually run.

- LinkedIn: [linkedin.com/in/stukhin](https://www.linkedin.com/in/stukhin/)
- Issues / feedback / category-specific gaps: please open a GitHub Issue or DM on LinkedIn — real-world failure modes drive every meaningful improvement.
