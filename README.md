# B2C Marketing Strategy Skill

A Claude Skill that turns a short interview into a fully-formed annual marketing strategy for a B2C digital product. Output is a single interactive HTML dashboard, 20 sections, with a built-in `.docx` export button.

> Status: **v1.0**, stable across all three modes. Actively evolving based on real-world feedback.

---

## What it does

Run the skill, answer a few questions, and get a publish-ready marketing strategy document covering:

- **Context** — market, consumers, competitors, brand health, SWOT
- **Brand & audience** — brand pyramid, positioning maps, audience × lifecycle, communication matrix
- **Execution** — product initiatives, media approach, MMM (budget × ROAS), regional play, communication plan
- **Governance** — risk register, references
- **Synthesis** — executive summary, our basics, business goal, assumptions, next steps

The skill runs its own research via `web_search` (DataReportal, Statista, World Bank, Sensor Tower, SimilarWeb, regulatory sources, recent press) so you do not need to feed it data manually.

Every fact-looking number is labelled: blue dot for an external source (with T1/T2/T3 tier), `★` for a hypothesis, `[to be confirmed]` for an internal-only fact only you can provide. Nothing is stated as fact without a source plate.

---

## Three modes

| Mode | Time | Questions | Hypothesis density |
|---|---|---|---|
| **Fast draft** | ~5 min | 2 | 70–80% `★` |
| **Guided** | ~15–20 min | 9 (+ optional document upload) | 30–50% `★` |
| **Deep interview** | ~30–45 min | 22 (+ optional document upload) | 10–20% `★` |

All three modes produce the same 20-section dashboard. Modes are a hypothesis-density dial, not a feature switch.

---

## Categories supported

The skill ships with category presets for 15 B2C digital verticals:

Classifieds · Full-stack e-commerce · Jobs · Real estate · Fashion / resale · Digital banking · Payments & remittance · Lending & BNPL · Investment & trading · Ride-hail & mobility · Quick-commerce · Food delivery · Subscription streaming · Edtech · Travel / OTA

Plus an **adjacent** preset for partial-coverage verticals (telemed, insurtech, dating, gaming, social/UGC, crypto) and a **first-principles fallback** for everything else.

---

## How to install

You need [Claude Code](https://claude.ai/download), free. Available for macOS, Windows, and Linux.

**Option 1 — drop the `.skill` archive into Claude Code:**

1. Download the latest `b2c-marketing-strategy.skill` from the Releases page
2. Drag it into Claude Code, or place it in `~/.claude/skills/`
3. Invoke with `/b2c-marketing-strategy` or simply ask: *"Generate a marketing strategy for [Brand] in [Country]"*

**Option 2 — clone the repo:**

```bash
git clone https://github.com/stukhin/b2c-marketing-strategy-skill.git ~/.claude/skills/b2c-marketing-strategy
```

The skill is auto-discovered. Restart Claude Code if it does not appear in the skills list.

---

## How to use

Once installed:

1. Start a new Claude Code session
2. Type `/b2c-marketing-strategy` (or describe what you need in natural language)
3. Pick a mode (Fast / Guided / Deep)
4. Answer the interview questions, in any language
5. The skill runs a research sweep, generates the dashboard, runs a self-check
6. Output appears in `output/<brand>-<market>-v1.html`, ready to open in a browser

The dashboard supports four themes (Steel / Vichy / Opaline / Mono) via the sidebar switcher, has a sticky table of contents, and ships with a `Download .docx` button so you can hand the doc to colleagues who do not want to open HTML.

---

## Output language

The interview can be in any language. The final dashboard is **always in English**, by design — keeps the document portable across multinational teams and consistent for benchmarking.

---

## Token cost expectations

Approximate full-run cost on Claude Sonnet 4.6 with prompt caching:

- Mode 1 (Fast): ≤ 200K tokens, roughly $1–2 via API or ~5–10% of a Pro subscription's 5-hour window
- Mode 2 (Guided): ≤ 300K tokens
- Mode 3 (Deep): ≤ 420K tokens

Opus 4.7 runs are roughly 5× the API cost of Sonnet but produce sharper synthesis sections. For most production runs, Sonnet is the right default.

---

## Troubleshooting

### Skill doesn't appear after I drop the `.skill` file

- Restart Claude Code. The skills directory is scanned on launch; new skills aren't auto-detected during a running session.
- Confirm the file landed in the right place: `~/.claude/skills/` for user-level install, or the project's `.claude/skills/` for per-project install. The file should keep its `.skill` extension.
- The `.skill` archive is a renamed zip. If your OS auto-extracted it on download (some browsers do this), re-download with right-click → "Save Link As" and keep the extension.

### Run aborted in the middle (rate limit, network, hand-stop)

- The output file is preserved at the wave it reached. Open `output/<brand>-<market>-v1.html` to see what's filled.
- To resume: in a new session, point at the existing output file and ask the skill to continue from the next unfilled section. Don't restart from scratch — you'll lose what was already done.
- For rate-limit aborts: wait until the 5-hour Pro window resets (or switch to the Anthropic API for the rest of the run).

### Self-check reports placeholder leaks

- The Step 5 Python self-check catches `[Company]`, `Competitor 1`, `[Axis 1]`, etc. that survived into the output. Most live in JS arrays (`const RISKS`, `const BRAND_TREND_DATA`, radar `competitors[]`) which generate visible chart labels.
- Open the output file, search for the leaked pattern, and replace inline. Then re-run the self-check command from Step 5.
- The pre-Wave-4 JS data sweep catches most of these proactively. If you see leaks in the final self-check, the sweep was skipped.

### `Download .docx` button does nothing

- Open the browser dev console (Cmd+Option+I → Console). The button logs to console on click. Common causes:
  - Pop-up blocker preventing the file download dialog. Allow downloads from `file://` or from the host where the dashboard is served.
  - JS error from a malformed section (e.g. unbalanced HTML tags). Fix the section and reload.
- If the button still fails, the dashboard is fully self-contained — you can also save the page as PDF from the browser as a workaround.

### Output is mostly hypotheses (★ everywhere)

- That's the expected hypothesis density for Mode 1 Fast (70-80% `★`). For lower density, switch to Mode 2 Guided (30-50% `★`) or Mode 3 Deep (10-20% `★`) and upload internal docs (brand tracker, last year's marketing plan, segmentation study).
- For brands without strong public coverage (small startup, nascent market), even Mode 3 will lean hypothesis-heavy. The skill is most accurate for brands with rich public data (DataReportal / Sensor Tower / press coverage).

### Channel reallocation chart is empty

- The chart auto-derives from the `#mmm` budget table. If the table has placeholder rows (`[Bucket N]`), the chart can't populate. Fill the MMM table first, the chart updates on page reload.
- If the brand is in launch / GTM mode, the chart is intentionally hidden via `class="gtm-hide"` (no prev-year baseline to compare against).

### Sidebar nav doesn't scroll to the right section

- The sidebar links use `<section id="...">` anchors. If you renamed a section or removed one, the nav-item still tries to scroll to a non-existent id.
- Verify with: `grep -E '<section id=' output/<brand>-<market>-v1.html` — the 20 expected ids are: `exec, basics, business-goal, assumptions, market, consumers, competitors, brand-health, swot, brand-pyramid, positioning, audience-lifecycle, comm-matrix, product-initiatives, media, mmm, regions, comm-plan, risks, references, next-steps`.

### Force a specific model (Sonnet vs Opus)

- Claude Code respects the model you pick in the session settings. For full prod runs Sonnet 4.6 is the default — production-grade output, ~5× cheaper than Opus.
- For sharper synthesis sections (`#exec` / `#assumptions` / `#risks`), Opus 4.7 produces noticeably tighter prose, but token cost is higher and Pro plan limits will fill faster.

### Roll back to a previous version

- All releases are kept on GitHub. If a newer version regresses output quality, download an older `.skill` from the [Releases page](https://github.com/stukhin/b2c-marketing-strategy-skill/releases) and replace the file in `~/.claude/skills/`.

### Something else broke / output is weird

Open an issue or DM me on [LinkedIn](https://www.linkedin.com/in/stukhin/) with: which mode you ran, what brand + market, the section that misbehaved, and ideally a screenshot. Real-world failure modes are the main driver of skill improvements.

## Author

Created and maintained by **Alexander Styukhin**.

LinkedIn: [linkedin.com/in/stukhin](https://www.linkedin.com/in/stukhin/)

This is a personal side-project. If you spot a bug, hit a rough edge, or have category-specific feedback, ping me on LinkedIn — I am actively iterating on the skill and real-world feedback is the main driver of improvements.

---

## License

[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

You may use, share, and adapt the skill for **non-commercial purposes** with attribution. Any derivative work must be released under the same license. Commercial use requires explicit written permission from the author.

See the `LICENSE` file for the full terms.

---

## Repository structure

```
b2c-marketing-strategy/
├── SKILL.md                          The workflow brain
├── README.md                         You are here
├── LICENSE                           CC BY-NC-SA 4.0
└── references/
    ├── dashboard-template.html       9.6K-line interactive HTML template
    ├── industry-presets.md           Category index + routing logic
    ├── interview-questions.md        Question banks for Modes 1, 2, 3
    ├── 40-question-bank.md           Priority-ranked question library
    ├── research-sources.md           Tier 1–3 source catalog per category
    ├── prompting-notes.md            Marker / opt-out / mode-fit reference
    └── presets/                      17 per-category preset files
```
