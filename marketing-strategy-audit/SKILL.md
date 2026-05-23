---
name: marketing-strategy-audit
description: Audit a marketing strategy document (or set of documents) against an objective 7-criterion rubric and surface non-obvious insights beyond what's literally written. Use when the user wants to evaluate their own draft strategy before presenting, review a strategy from an agency or internal team, or pressure-test a marketing section of a pitch deck. Accepts PDF, PPTX, DOCX, and plain text uploads. Supports single-doc or multi-doc input. Produces an interactive HTML dashboard with scorecard, gap list, recommendations, and a "Beyond the document" section of insights the skill derived through cross-reference, web research, and pattern detection. Two modes (Quick score / Full audit), English or Russian output, picked at the start. Mild formative tone — meant as a strengthening tool, not a takedown.
---

# Marketing Strategy Audit skill

Reviews a marketing strategy document — or several related documents — and gives an honest, formative assessment. Primary audience: in-house marketers stress-testing their own draft before presenting; marketing leads reviewing what agencies and internal teams bring them; consultants doing third-party audits.

The output is **NOT a structured summary of what's already in the files**. That would be useless — the user can read their own document. The output IS:

1. A scorecard against an objective 7-criterion rubric
2. A concrete gap list with priorities
3. Specific recommendations to close each major gap
4. **A "Beyond the document" insight layer** — what the skill noticed that the document itself doesn't surface: cross-document inconsistencies, hidden assumptions, missed opportunities in the uploaded data, reality-gap signals from web research, strategic alternatives not considered. This is the differentiation. Without it the skill is a fancy summarizer.

> **Skill state:** v1.0. First release. Companion to `b2c-marketing-strategy` in the same repo.

## Workflow — fixed, do not reorder

### Step 0 — Mode + language pick

Ask the user to pick **two things in one prompt**:

**(a) Mode:**

1. **Quick score** (~10 min) — runs steps 1–4 + 6 + 7. Output: scorecard + gap list, no recommendations, no Reality check, no Strategic alternatives. Use for a preliminary read.
2. **Full audit** (~30–45 min interview + generation, total run can take a couple of hours) — runs all steps. Output: scorecard + gap list + recommendations + Beyond-the-document insights + Reality check + Cross-doc map. Use for a serious review.

**(b) Output language:** English (default) or Russian. Same two-language scope as the main `b2c-marketing-strategy` skill — section headings, prose, chart titles, takeaways all render in the selected language. Sidebar nav stays English (structural).

**Mode recommendation nudge:** if the user is doing this on their own draft for the first time, recommend Quick score — it's enough to see the obvious gaps in ~10 min. Full audit makes sense when the document is far along, or when reviewing someone else's work where a soft answer doesn't help.

### Step 1 — Upload + context

Ask the user to attach the document(s). Accept these formats: **PDF, PPTX, DOCX, plain text / Markdown**. Multiple files in one run are fine — the skill cross-references them in Step 4.

Then ask two short context questions, but **skip what the user already told you**. If the user named the brand+market or the category in their Step 0 message (e.g. "audit my Spravochnaya strategy in Russia"), don't re-ask the same thing — just confirm in one line ("Got it, brand=Spravochnaya, category=media") and only ask the missing piece. The point is to capture context, not to interrogate.

- **What brand and market is this strategy for?** (helps Reality check via web_search) — ask only if unknown from Step 0
- **What category is the brand in?** (helps Strategic alternatives via category presets in the parallel `b2c-marketing-strategy` skill if available, or via Research Sweep otherwise) — ask only if unknown from Step 0

**Do not scan the user's filesystem.** Only parse the files the user explicitly attached or named with a path. Same rule as in the main skill — no opening Downloads / Documents speculatively.

### Step 2 — Parse documents

For each attached file:
- Extract text content
- Identify document type (annual marketing plan / brand strategy / pitch deck marketing section / campaign brief / channel strategy / other)
- Extract structural elements: sections / headings / KPI tables / risk lists / segments / channels mentioned / numbers with sources / claims without sources
- Hold the extracted data in working memory — referenced by file ID in Steps 3–4

If a file is image-heavy (slide deck with mostly visual content), extract whatever text is available and flag that visual content was not analyzed. Don't fabricate what's in slide images.

### Step 3 — Run 7-criterion audit

For each uploaded document, score on a 0–100 scale against these 7 criteria. Full rubric in `references/audit-rubric.md`. Brief definitions:

1. **Specificity** — Are claims grounded in concrete numbers (KPI targets, budget figures, dates, percentages), or stated in generic language ("grow brand", "expand reach", "engage audience")?
2. **Verification** — Are numbers backed by named sources (citations, study references, internal data attribution), or floating without provenance?
3. **Risk register quality** — Is there a risk register? Are risks scattered (off-diagonal — low-prob/high-impact tail-risks AND high-prob/low-impact drags) or aligned in a single severity line?
4. **Action specificity** — Does each initiative carry owner + deadline + success metric, or read as a wishlist?
5. **Synthesis integrity** — Does the executive summary actually reflect the content of the appendix / detailed sections, or are they two disconnected documents?
6. **Reality check** *(skipped in Quick score)* — Does the document's claimed positioning / market understanding match observable reality (App Store / Play Store reviews, Google Trends momentum, recent press coverage)?
7. **Completeness** — Are the standard blocks of a marketing strategy present (market context / consumer / competitive set / positioning / KPI tree / media mix / budget / risks / next steps) or are there gaping holes?

Multi-doc handling: score **each document individually** for criteria 1–5 and 7. Criterion 6 (Reality check) is brand-level, scored once across all docs. Add a cross-document consistency score (separate, see Step 4).

### Step 4 — Outside-the-box insight extraction

**This is the differentiating step.** Without this, the audit is a sophisticated checklist. With this, it's a strategic second pair of eyes.

For each of the 7 insight types in `references/insight-types.md`, scan the parsed content and surface what's NOT obvious from reading the document alone:

1. **Cross-document inconsistencies** *(multi-doc only)* — strategy says X, brand book says Y, agency proposal says Z. Find divergences and name them concretely.
2. **Hidden assumptions** — claims that imply unstated bets (e.g. "we'll grow 50% YoY" assumes the category itself won't contract; "Gen Z will adopt this" assumes attention shifts not happening). Pull them up and flag.
3. **Missed opportunities in the uploaded data** — data is present in the document but not used in conclusions. Example: doc lists 6 segments, strategy only addresses 3 — what about the other 3?
4. **Reality gap via web_search** *(Full audit only)* — run a targeted 3–5 query batch on App Store / Play Store / Google Trends / recent press for the named brand. Compare the document's claimed positioning to what observable signals say. Surface the gap if there is one.
5. **Strategic alternatives not considered** *(Full audit only)* — what plays do comparable brands in this category use that the document doesn't mention? Use either the matched preset from the companion `b2c-marketing-strategy` skill if accessible, or a quick research sweep.
6. **Pattern detection / blind spots** — recurring themes that imply a bias. Example: all 5 listed risks are about competitors, none about supply / regulation / macro — that's a linearity bias. Or: every initiative is a brand campaign, no performance or CRM bets — that's a channel bias.
7. **Benchmark outliers** *(Full audit only)* — claimed metrics that are unusual for the category (KPI targets 3× the category benchmark, or 1/3 the typical level). Surface and ask if the gap is intentional or unconsidered.

Each insight gets a 1-line statement of what was found, 1 line of why it matters, and 1 line of suggested action.

In Quick score, only run insight types 1, 2, 3, 6 (cross-doc, hidden assumptions, missed opportunities in data, pattern detection). These don't require web_search and are the lowest-cost insights.

### Step 5 — Research Sweep *(Full audit only)*

Targeted batch of 5–8 queries to support criteria 6 (Reality check) and insights 4 (Reality gap), 5 (Strategic alternatives), 7 (Benchmark outliers):

- `[Brand] App Store rating reviews [country] [year]`
- `[Brand] Google Play rating reviews [country] [year]`
- `[Brand] vs [comp 1] vs [comp 2] Google Trends [country] 12 months`
- `[Brand] [country] recent press coverage [year]`
- 1–2 queries on category benchmarks for the named metrics
- 1–2 queries on comparable strategic plays in this category

One parallel batch. Findings feed into Steps 6.

### Step 6 — Generate HTML dashboard

Copy the template into the output folder via Bash:

```bash
cp references/audit-dashboard-template.html output/<brand>-audit-v1.html
```

(or `<short-doc-name>-audit-v1.html` if no clear brand from context).

**Then `Read` the output file once** (first ~20 lines is enough) before the first `Edit`. Same constraint as the main skill — Claude Code's `Edit` tool refuses to operate on a file it hasn't seen yet. `cp` doesn't register with the tool's file-state tracker. Without this `Read`, the first Edit fails with "file not read yet".

Fill 8 sections:

1. **Header** — file name(s) audited + brand + category + mode + date
2. **Score summary** — 7 criteria × score 0–100 each + overall composite + 1-sentence headline (e.g. "strong synthesis, weak risk register, gaping data hole on consumer")
3. **Gap list** — gaps grouped by criterion, each gap = 1-line title + 2-line explanation + priority tag (high / medium / low)
4. **Recommendations** *(Full audit only)* — concrete fixes mapped to gaps. Each rec = action verb + target outcome + suggested approach
5. **Beyond the document** — outside-the-box insights from Step 4, grouped by type, each as a card
6. **Cross-document map** *(multi-doc only)* — visual matrix showing which claims appear in which doc, where docs agree, where they diverge
7. **Reality check** *(Full audit only)* — claimed positioning vs observable signals, with brand-tracker-style attribute table where applicable
8. **References** — what was cited from each uploaded file + what was sourced via Research Sweep

**Tone of voice** — mild, formative, strengthening. "Here's how to make this stronger" not "this is wrong". Same single-author register across all 8 sections. No corporate hedging, no MBA jargon, max 30-word sentences.

### Step 7 — Self-check + present

Run a Python self-check (subset of the main skill's check, see `references/audit-self-check.md`):
- Header filled
- All 7 scores numeric (not placeholders)
- Gap list non-empty (audit found at least 3 gaps — if literally zero gaps, that's suspicious, re-examine)
- Section balance 8/8

Then present to user:
1. **File path** of output
2. **3-line summary** — overall score, top 3 gaps, top insight from "Beyond the document"
3. **Token cost** vs mode budget (Quick ≤120K, Full audit ≤280K)
4. **Invite specific feedback** — "Did the Beyond-the-document insights surface anything you hadn't seen? If yes, where; if no, what would have been useful?"

## Three states for every claim

Same plate system as the main skill:

- **Verified** → `<a class="ext" href="<url>" title="Source [Tier]"></a>` blue-dot link to a source the audit found via web_search
- **Hypothesis** → `<span class="hyp">★</span>` for inferences the audit made from the document (e.g. "this implies an assumption that...")
- **From-document** → `<span class="from-doc" data-doc="<file>">▣</span>` for claims pulled directly from the uploaded file, with the file name in `data-doc`. New marker specific to this skill — distinguishes "doc says this" from "we hypothesized this".

## Per-section content notes

- **#header** — file name(s) audited + brand + category context + mode + audit date. Keep concise, 4 lines max.
- **#score-summary** — Composite score is unweighted average of the criteria that apply. Full audit: avg of 7. Quick score: avg of **6** (Reality check is skipped — 7 minus 1 leaves 6, not 5). One-sentence headline must name the dominant pattern: "synthesis-strong, risk-weak", "data-rich but specificity-poor", etc.
- **#gap-list** — group by criterion. Within criterion, order by priority (high → low). 3–8 gaps per criterion is typical; if a criterion has 12+ gaps, the document is probably very weak there — note that explicitly.
- **#recommendations** *(Full audit)* — one rec per high-priority gap. Action-verb led ("Add risk register section with..."), 2-line max each.
- **#beyond-the-document** — 5–9 insight cards, grouped by type. Each card: 1-line finding + 1-line why-it-matters + 1-line action. Most-impactful insight goes first.
- **#cross-doc-map** *(multi-doc)* — table or matrix. Claims as rows, documents as columns, ✓ / ✗ / divergence flag in cells.
- **#reality-check** *(Full audit)* — 4-row table: claimed (from document) vs observed (from web_search) for the 4 most important attributes / positioning claims. Each row carries `.ext` link to the observable source.
- **#references** — separate "From uploaded files" subsection (with file names + page or slide references where available) and "From research sweep" subsection (T1/T2/T3 sources).

## Anti-patterns — three hard rules

1. **No restating the document.** If a paragraph in the output could have been pasted from the uploaded file, it doesn't belong. The audit is what the document does NOT say.
2. **No subjective takedowns.** "This is weak / bad / unprofessional" is a value judgment, not a finding. Replace with concrete "X is missing" or "Y is unsupported".
3. **No inventing insights.** Beyond-the-document insights must trace back to something — a cross-doc divergence, a numeric inconsistency, a web_search finding, a category pattern. If you can't trace it, don't write it.

## Token budget guardrails

| Mode | Total target | Composition |
|---|---|---|
| Quick score | ≤ 120K | parse + audit + 4 insight types + dashboard |
| Full audit | ≤ 280K | parse + audit + 7 insight types + research sweep + dashboard |

If Quick score breaks 150K — too much was parsed (multi-doc bloat); compress doc summaries before insight step.
If Full audit breaks 320K — research sweep too broad or insight extraction over-elaborated; tighten.

## When in doubt

Read `references/insight-types.md` for the insight extraction patterns and `references/audit-rubric.md` for scoring details.
