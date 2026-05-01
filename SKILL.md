---
name: b2c-marketing-strategy
description: Create a structured annual marketing strategy for B2C digital products (marketplaces, fintech, subscription services, ride-hailing, delivery, consumer apps). Use when the user mentions "marketing strategy", "annual marketing plan", "GTM strategy", "marketing roadmap", "brand strategy document", or asks for help structuring a top-down marketing strategy with sections like positioning, segmentation, competitors, media plan, or brand health. Offers three generation modes (Fast draft / Guided / Deep interview) picked at the start; Guided and Deep accept optional document uploads. Produces an interactive HTML dashboard (with voting, charts, sortable tables) plus a built-in "Download .docx" button. English output only.
---

# B2C marketing strategy skill

Produces a one-file interactive HTML dashboard — annual strategy for a B2C digital product. 20 sections across overview / context / brand-audience / execution / governance / references. Single output file, English only, with client-side .docx export button.

> **Skill state:** v1.0. Stable across all three modes. Continues to evolve based on real-world feedback.

## Workflow — fixed, do not reorder

### Step 0 — Mode select

Ask user to pick:

1. **Fast draft** (~5 min) — 2 questions (brand+market, use-case+stage), no document upload, 70–80% hypotheses marked ★, straw-man for review
2. **Guided** (~15–20 min) — 9 questions total: 2 base + upload prompt at start + 7 prioritized fillers (drawn from `references/40-question-bank.md`), ~30–50% hypotheses depending on doc richness
3. **Deep interview** (~30–45 min) — 22 questions total: 2 base + upload prompt at start + 20 prioritized fillers + final upload reminder, 10–20% hypotheses, consulting-grade

If the user mentions internal docs / decks / brand tracker, OR the brand has rich public coverage on this market — nudge toward Guided or Deep with upload: "Notice you have [docs] / [Brand] in [market] has substantial public coverage. Guided or Deep with even one document beats Fast alone." User decides.

### Step 1 — Interview

Use `references/interview-questions.md` for the mode's question set. Don't add extra questions beyond the spec.

**Brand-existence sanity check — after Q1, before Q2 (mandatory, all modes).** Take 5 seconds from your existing knowledge: does this brand operate in this market at this stage? Public footprint (app store presence, brand global page, recent news) usually answers in 1–2 lines. If the brand is **absent** from the market or has **exited** it, fold the flag inline into the Q2 follow-up — one extra line, not a separate turn:

> "Note: [brand] doesn't currently operate in [market] — treating as GTM hypothesis. Confirm or adjust. Q2: what's the use case + stage?"

When the check returns "not in market" OR Q2 answer is `stage=launch` OR `use case=GTM for new market` — auto-trigger the **Stage modifier — launch / GTM** rules (see section below) for Wave 1, 3, 5 sections. Don't gate it as a separate confirmation; the user can correct in their Q2 reply.

**If the user uploads documents (Mode 2 or Mode 3):** parse uploaded content first, then ask only the questions whose answers aren't already covered. Don't repeat questions the docs answer — that's the whole point of upload. For partially-answered questions (doc has revenue band but not channel split), ask only the missing slice.

### Step 2 — Confirm

Summarize in ONE LINE: `[Brand] · [stage: launch/scale/mature/decline] · [primary market] · [use case: investment pitch / annual plan / brand relaunch / GTM]. OK to proceed?` User confirms.

### Step 3 — Research Sweep

First, **load the matched category preset.** Open `references/industry-presets.md` (index, ~30 lines), match the brand to one of the 15 categories, then read **only that category's preset file** in `references/presets/<category>.md` (e.g. `presets/food-delivery.md`). Don't load all presets — the index is enough to route.

Then run ONE parallel batch of `web_search` calls. Use `references/research-sources.md` to assemble 12–18 queries:

- 6 universal Tier 1 queries (DataReportal / GWI / Statista / Sensor Tower / SimilarWeb / World Bank)
- 4–6 category-specific queries (from the matched preset)
- 1–2 country-specific (regulatory / national stats)
- 2 competitor financials (SEC, IR pages, public news)

Do the whole sweep in **one parallel call**. Don't scatter searches through the build — that's the single biggest context drain.

If you notice a high-value source outside the catalog (recent regulator news, breaking story, niche specialist publication) — add it. 1–3 ad-hoc additions are fine; 10+ defeats the speed-up.

### Step 4 — Generate

1. Copy `references/dashboard-template.html` → `output/<brand>-<market>-v1.html` (one Bash command).

2. **HEADER FIRST.** Before any section work, fill these three slots — they fail silently otherwise (browser tab + page heading carry the placeholder):
   - `<title>` tag — `[Brand + Market] — Marketing Strategy [Year]`
   - `.doc-title-main` — `[Brand + Market]`
   - `.doc-title-sub` — `Marketing Strategy [Year]`

3. Edit the body section by section in this fixed wave order:

```
Wave 1 — Context     market → consumers → competitors → brand-health → swot
Wave 2 — Brand       brand-pyramid → positioning → audience-lifecycle → comm-matrix
Wave 3 — Execution   media → mmm → regions → comm-plan
Wave 4 — Governance  risks → references
Wave 5 — Synthesis   business-goal → assumptions → exec → basics → next-steps
```

**Synthesis is LAST.** Exec written first becomes a generic platitude. Written last, it distills already-filled appendix.

Per section: replace `[Slot]` placeholders with real content via Edit. Anchor by the section's `<h3 class="h-sub">` heading + nearest unique placeholder.

**Atomic-edit-per-section discipline (revised 2026-04-29).** One Edit-tool call per section IF section ≤120 lines. Sections at 150–220 lines (`#competitors`, `#product-initiatives`, `#media`, `#mmm`, `#consumers`) MUST split into 2–3 logical blocks (header / table / takeaway). At 150+ lines the `old_string` payload itself dominates token cost — a 200-line atomic Edit costs more than 3 × 70-line Edits despite the narration overhead. The original "atomic-only" rule prevented a different anti-pattern (4–5 micro-Edits per section); that still applies. The right shape: 1 Edit for short sections, 2–3 Edits for long ones, never 4+.

**Channel Reallocation chart auto-populates from MMM table** — fill `#mmm` budget planning table once; the diverging-bar chart picks up channel labels + Δ% automatically at runtime. Don't fill the JS `const channels = [...]` array.

**JS data sweep — between Wave 3 and Wave 4 (mandatory).** Before starting Wave 4, scan the file for placeholder labels in JavaScript constants and `data-*` attributes. Wave 1–3 fix the visible HTML body, but `const BRAND_TREND_DATA`, `const RISKS`, radar `competitors[]` array, chart axis labels, `data-tip` / `data-value` / `data-axis` attrs carry their own copies of `[Company]`, `Competitor [1-9]`, `[Axis [1-9]]`, `[Bucket [1-9]]`. They render visibly in chart tooltips, radar polygon labels, and info-tip popovers — but they live below the visible HTML so they're easy to miss. Self-check at Step 5 catches them post-fact, but a sweep here saves a fix loop. Run one grep: `grep -nE '\[Company\]|Competitor [1-9]|\[Axis [1-9]\]|\[Bucket [1-9]\]|\[Risk [1-9]\]' output/<file>.html` and resolve each hit before Wave 4. This is the single most common visible-output bug class — JS arrays are why "looks done" outputs still ship with `Competitor 1` on the radar.

### Step 5 — Self-check

Run from terminal:

```bash
python3 -c "
import re
s = open('output/<brand>-<market>-v1.html').read()

# Check 1: Header still placeholder
if '[Brand + Market]' in s or 'Dashboard Template' in s:
    print('✗ HEADER not filled — fix <title> + .doc-title-main + .doc-title-sub')
else:
    print('✓ Header filled')

s_visible = re.sub(r'<!--[\s\S]*?-->', '', s)  # strip HTML comments to avoid false-positives on instructional text

# Check 2: Known leaked patterns from template (these slip past visual scan).
#   Template carries placeholder text in JS chart data, info-tip data-tip attributes,
#   and radar/competitor labels. They render visibly but are easy to miss.
#   Run on s_visible (HTML comments stripped) so example/guidance text inside
#   <!-- ... --> doesn't fail the check.
leaks = [
    (r'\[Company\]',           'literal [Company] placeholder — fill with brand name'),
    (r'\[Capital city\]',      'literal [Capital city] placeholder'),
    (r'\[Year\]|\[year\]',     '[Year] / [year] placeholder'),
    (r'Competitor [1-9]\b',    'JS / data-tip label — replace Competitor 1/2/3 with real competitor names'),
    (r'\[Axis [1-9]\]',        'radar axis placeholder'),
    (r'\[Bucket [1-9]',        'channel bucket placeholder (matches both [Bucket N] and [Bucket N — e.g. ...])'),
    (r'\[Region [1-9]\]',      'region placeholder'),
    (r'\[Channel [1-9]\]',     'channel placeholder'),
    (r'\[Segment [1-9]\]',     'segment placeholder'),
    (r'\[Bet [1-9]\]',         'bet placeholder'),
    (r'\[\+/-X% vs [12]\d{3}\]', 'budget-bar-seg data-delta placeholder — fill with +/-N% or —'),
    (r'\[X\.X[x×]\]',          'budget-bar-seg data-roas / blended ROAS placeholder — fill with N.Nx or —'),
    (r'\[Optional 1-line context\]', 'budget-bar-seg data-note placeholder — fill or remove the attribute'),
    (r'\[Total envelope \$X\.X M\]', 'basics Budget total placeholder — fill with $X.XM'),
    (r'\[Risk [1-9] short\]|\[Risk [1-9] —', 'RISKS short / title placeholder — fill with concrete risk'),
    (r'\[Strategic risk —|\[Regulatory tail-risk\]|\[Operational drag — high-prob low-impact\]|\[Mid-mid competitive squeeze\]|\[High-prob low-impact miss\]|\[Operational supply gap\]|\[Macro tail-risk\]', 'RISKS scaffolding placeholder — replace with business-specific risk title'),
    (r'classifieds|real estate(?!\s+marketplace)|fashion-resale',  'old marketplace bleed in JS data captions'),
    # JS / data-attribute placeholder leaks — render visibly in chart tooltips, radar labels, info-tip popovers
    (r"name:\s*['\"]Competitor [1-9]['\"]", 'JS competitors[] array still on Competitor N labels'),
    (r'data-(?:tip|value|axis|bucket|note|company)="\[[^\]]+\]"', 'data-* attribute carries [...] placeholder'),
    (r'BRAND_TREND_DATA[\s\S]{0,400}?\[Company\]', 'BRAND_TREND_DATA const carries [Company] placeholder'),
    (r'\[Y[12]\s*target\]|\[Y[12]\]', 'Y1/Y2 target placeholder — fill with concrete number or ★'),
]
total_leaks = 0
for pat, label in leaks:
    matches = re.findall(pat, s_visible)
    if matches:
        total_leaks += len(matches)
        print(f'✗ {label}: {len(matches)} hits — first: {matches[0]!r}')
print(f'  Total leaked patterns (visible): {total_leaks}')

# Check 3: Broad bracket sweep (after HTML comments stripped) — catches anything Check 2 missed.
hits = re.findall(r'\[[A-Za-z][^\]]{0,80}\]', s_visible)
real = [h for h in hits if not re.match(r'\[(data-|open|aria-|class=|role=|T[1-3]\])', h)]
real_no_js = [h for h in real if 'TextRun' not in h and 'colIdx' not in h and 'headerRow' not in h]
print(f'  Other bracket hits (excl. attrs/T1-T3/JS code): {len(real_no_js)}')
for h in real_no_js[:15]: print(f'    {h}')

# Check 4: HTML balance
print(f'  Sections: {s.count(\"<section\")} open / {s.count(\"</section>\")} close (must be 20/20)')
"
```

All checks must pass: header filled, zero leaked patterns, low bracket-hit count (each remaining bracket is intentional `[to be confirmed]` or `[T1]/[T2]/[T3]` source-tier marker), 20/20 sections balanced. If anything fails — fix and re-run.

Also visually verify: every sidebar `nav-item` scrolls to a real `<section id="...">`.

### Step 6 — Present

Tell user:
1. **File path** of the output.
2. **3-line summary** — what's filled with verified data (count of T1/T2/T3 sources), what's hypothesis-heavy (count of ★), what's `[to be confirmed]`.
3. **Token cost** — total estimate vs Mode budget guardrail (Mode 1 ≤105K, Mode 2 ≤135K, Mode 3 ≤175K). Flag if any wave exceeded its band.
4. **Invite block-level feedback explicitly:**

   > "If any section needs refinement — comment with the section name + what's off (e.g. `#brand-pyramid: essence too generic`, `#competitors: missing local player X`, `#media: TikTok share too high`). I'll iterate before locking the file."

   This is a mandatory part of Present — never skip it. The user is best-positioned to spot blocks where the hypothesis lands wrong; explicit invitation lowers the bar to flag.
5. **Offer one iteration shortcut:** deepen a specific section / replace a hypothesis with real data / sharpen the competitive section / re-run brand-tracker calibration with newly-loaded numbers / etc.

## Three states for every claim

Every line of fillable content takes one of three states. **No fact-looking text without one of these plates.**

- **Verified** → `<a class="ext" href="<url>" title="Source [Tier]"></a>` blue-dot link.
  - **T1** = official primary (regulator, IR, World Bank). Cite as fact.
  - **T2** = research aggregator (Statista, GWI, McKinsey). Attribute the source.
  - **T3** = signal / editorial (Adweek, Reddit, news). Qualitative claims only, never precise numbers.
- **Hypothesis** → `<span class="hyp">★</span>` small superscript star, visual marker only. Use for any number / claim NOT user-provided AND NOT web-sourced. The marker has no hover behaviour — it's a flag, not a tooltip. (Earlier versions carried `data-hyp="<rationale>"` for a hover plate; that was removed because the rationale was rarely read and the cursor change was distracting. `data-hyp` attributes still in old runs are inert.)
- **To be confirmed** → `<span class="tbd">[to be confirmed]</span>` for genuinely-internal facts only the user can provide (specific people, internal tracker setups, past campaign results).

## Per-section content notes

- **#exec / #basics / #next-steps** — synthesis only. **▼ density rule:** each basics-title (Target audience / Positioning / Points of difference / Communication plan / Budget) MUST carry `<a class="tri" href="#<appendix>">▼</a>` to its appendix anchor; each substantive sub-bullet that names a segment / brand / claim / metric gets its own `<a class="tri" href="...">▼</a>` too. Anchors: #consumers (segments), #competitors (rivals), #positioning (brand pyramid + maps), #brand-health (tracker stats), #comm-plan (channel/lifecycle), #mmm (budget + ROAS), #product-initiatives (flagships), #audience-lifecycle (cohort × stage). Use `<a class="tri" href="#…">` not `<span class="tri">` — the latter is unclickable.
  - **Key objectives tiles — EXACTLY 3, never 4.** The objectives grid in `#exec` is a forcing function for sharper plan. If the year carries 4 ambitions, pick the top 3. The 4th tile is always the one that gets watered down in synthesis and dilutes the visual weight of the 3 real bets. Template no longer suggests "2-4 tiles" — locked to 3.
- **#business-goal** — funnel stages with 3-dot priority and delta arrows. Numbers either sourced or starred.
- **#assumptions** — 3–7 cards: claim + confidence + leading signal + breaker.
- **#market** — overview + 3–7 trends. All numbers sourced (T1/T2) or starred.
- **#consumers** — **5–8 segment cards (minimum 5)**: reach % bar, 1–2 metrics, sensitivity, voice quote, brand chips. Hard quality cap: each segment must have distinguishable financial behaviour, distinct media mix, OR distinct message — if two segments collapse on all three, merge them and pick a 5th from elsewhere.
  - **Card top-corner badges (mandatory, all cards):** `<span class="seg-num">#N</span>` left + `<span class="seg-prio high|med|low">High|Med|Low</span>` right. N is the sequential card index (1, 2, …); priority distribution **must be ~1–2 High, 1–3 Med, the rest Low** — the priority signal is where budget actually concentrates, not a flat spread. Don't tag every card the same level.
  - Two donut charts above the scroller (Revenue/GMV share + Audience share) auto-derive from `.segment-bar.reach` + `.segment-bar.gmv` widths and `.seg-num` + `.segment-name` labels. Bob fills the cards once; pies populate.
- **#competitors** — direct (parameters-as-rows × competitors-as-columns), head-to-head verdicts, indirect, 1–2 positioning maps, radar chart.
- **#brand-health** — funnel attribute bars vs benchmark. If no real tracker data, flag whole section with `.hypothesis-badge`.
- **#swot** — 4 quadrants × 3–5 items, each with `.ext` evidence.
- **#brand-pyramid** — 5 levels (essence → values → personality → benefits → attributes).
- **#positioning** — 1–2 positioning maps with category-specific axes + 4–6 hypothesis points.
- **#audience-lifecycle** — segments × funnel-stages matrix, 1 line per cell.
- **#comm-matrix** — segments × stages × communication objective, terse.
- **#product-initiatives** — 3–5 flagship cards on timeline: launch pill + 3-block grid (audience+problem / channel+messages / metrics+assets).
- **#media** — 6–12 channel rows: reach + funnel-stage + KPI + plan direction (↑/→/↓).
- **#mmm** — budget bucket bar + channel saturation × ROAS table.
  - **Each `.budget-bar-seg` MUST carry data-attributes** that drive the on-hover tooltip card: `data-bucket="<name>"`, `data-pct="<X>%"`, `data-delta="<+/-X% vs <prev-year>>"`, `data-roas="<X.Xx>"`, optional `data-note="<1-line context>"`. If a bucket has no Δ or ROAS signal (Tools, Reserve), use `—`. Don't leave the `[+/-X% vs 2025]` / `[X.Xx]` placeholders — they render visibly when hovered. The legacy `title="..."` attribute is ignored.
- **#comm-plan** — quarterly grid: T1 / T2 / T3 / Special projects / Always-on rows × 4 quarter columns. **T1 and T2 cells MUST carry `.q-meta` block** with budget + lead KPI + KPI target — they're the anchor campaigns of the year. Format inside the cell: `<div class="q-bet">[Bet ref]</div><div class="q-meta"><span class="q-budget">$X.XM</span><span class="q-kpi">[Lead KPI]: <span class="q-kpi-target">[target]</span></span></div>`. T3 experiments and Special projects (seasonal moments) do NOT need `.q-meta` — they're either small-spend or non-campaign rows; Always-on rows already carry budget inline. Lead KPI maps to the funnel stage the campaign drives (Awareness for T1 launch wave, Acquisition for T2 push, Retention for CRM-led pushes). T1/T2 budgets across all four quarters MUST sum to the corresponding `#mmm` Brand + Performance bucket totals — that's the cross-section validation. KPI target carries `★` in launch year (Stage modifier triggered).
- **#regions** — 3–7 cluster rows with tube-bar reach. **Cluster logic block uses 3-column grid** (`.cluster-grid`), not the deprecated `<ul class="cluster-bullets">` flow. Each row of the grid is three direct children: `<span class="cluster-pill cN">[Cluster name]</span>` + `<strong class="cluster-regions">[Regions]</strong>` + `<span class="cluster-intent">[Strategic intent]</span>`. The grid keeps pill / regions / intent vertically aligned across rows regardless of content length. Add or drop sets of three to flex cluster count 2–6. **Adaptive granularity by business scope:**
  - **Multi-country activity** (parent rollout, regional play, several markets) → rows = countries; cluster = Core / Expansion / Probe / Monitor.
  - **Single-country activity** (most B2C strategies) → rows = cities / metros / tier-2 clusters within that country.
  - **Single-city activity** (hyper-local, city-only) → SKIP section entirely; replace body with the `<div class="card"><p class="intro-para text-mute"><em>Not applicable — single-city operation…</em></p></div>` opt-out card; keep heading + sidebar nav entry. Do NOT fabricate regional priorities for a single-city business.
- **#comm-plan** — quarterly grid: seasonal / experiment / always-on dots.
- **#risks** — 5–7 risks on probability × impact bubble chart + 5-sentence risk picture.
  - **Anti-linearity rule (hard):** don't generate risks where p and i correlate diagonally. Real risk maps are scattered — every risk register MUST include at least **one off-diagonal in each direction**: a low-probability / high-impact tail-risk (regulatory action, black-swan competitor move, sovereign event) AND a high-probability / low-impact constant drag (FX volatility, CPM creep, supply churn). If all 8 risks march along the diagonal, the strategist hasn't separated likelihood from severity — fix before shipping. The narrative ("What the risk picture tells us") should explicitly call out the off-diagonal points and what makes their response different from on-diagonal risks.
- **#product-initiatives** (already covered above for flagships) — additional UX contract: each `.lt-mark` in the year timeline binds to the matching `.flagship-card` by index. When the user drags a Gantt dot, the JS already updates both the timeline `.lt-date` AND the flagship card's `.fs-launch-pill` ("Launch Q[X] [year]") + first `.fs-stat-value` ("Mon DD, YYYY"). Bob just needs `.lt-bar` to carry `data-year="<YYYY>"` and the lt-mark / flagship-card order to match — no other action.
- **#references** — 8–15 sources cited via `.ext` throughout, source-tier badges.

## Stage modifier — launch / GTM

Added 2026-04-29. The dashboard template defaults assume an operating brand in a market with measurable history (current MAU, last-year spend, brand tracker waves, prev-year ROAS). Pre-launch and GTM-into-new-market scenarios have **none of that** — every "current" value is zero by definition. Without a stage modifier, Bob ends up either fabricating numbers (which then become ★ everywhere) or manually rewriting column headers section-by-section. Both paths corrode output quality.

**Trigger.** Apply this modifier when ANY of these is true:
- Q2 answer: `stage = launch` (first 12 months)
- Q2 answer: `use case = GTM for new market`
- Brand-existence sanity check at Step 1 returned "not currently in market"

When triggered, six sections deviate from their default content notes. Apply these rules **in addition to** "Per-section content notes" above — they override conflicting parts.

- **#brand-health — opt-out card.** The funnel/attribute table assumes existing tracker data. Pre-launch all values are zero. Replace section body with:
  ```
  <div class="card">
    <p class="intro-para">
      <em>Not applicable pre-launch — brand tracker baseline wave commissioned post-launch (target Q2 [year]). For Y1 awareness / consideration / trial targets, calibrate against [matched preset's brand-funnel benchmark — challenger band] from comparable [category]-class market entries.</em>
    </p>
    <p class="intro-para">Recommended tracker design: n=[size], [cadence] waves in core metros from launch +90 days, attributes battery aligned to brand pyramid §[anchor].</p>
  </div>
  ```
  Keep heading + sidebar nav entry. Don't fabricate "current" values. Don't apply `.hypothesis-badge` — the section is genuinely not applicable, not a low-confidence guess.

- **#mmm — drop prev-year semantics.** Default tooltip card uses `data-delta="<+/-X% vs prev-year>"` and `data-roas="<X.Xx>"`. For launch, prev-year doesn't exist. Two acceptable patterns, pick one:
  - **(a) Benchmark-from-comparables:** set `data-delta` against the brand's other markets' Y1 mix or against a structural-class peer's Y1 mix (e.g. an adjacent-region challenger with similar category position) — flag the bucket-row with `★` since it's a proxy, not a direct year-over-year comparison. `data-note="Y1 forecast — benchmarked against [comparable market] Y1 mix"`.
  - **(b) Plain forecast:** set `data-delta="—"` and `data-roas="—"` for all rows. `data-note="Y1 forecast — no prev-year baseline"`.
  Default to (a) when Research Sweep returned data on comparable market entries; otherwise (b).
  **ALSO — hide the "Channel reallocation — Δ vs 2025" chart entirely.** It needs prev-year channel splits to be meaningful; in launch year it renders an empty diverging bar with a "[fill MMM table to populate]" placeholder. Add `class="gtm-hide"` to the `<div class="chart-card">` that contains the `#budgetDeltaChart` canvas — the helper class is in template CSS (`.gtm-hide { display: none !important; }`). Don't try to repurpose the chart for "Y1 vs comparable market" — that's a different visualization and confuses readers expecting a year-over-year delta.

- **#business-goal — Y1 / Y2 / Y3 build, not 2025-actual vs 2026-target.** The KPI tree's default "prev-year actual vs current-year target" comparison breaks pre-launch. Replace funnel-stage chips with **Y1 target / Y2 target / Y3 target** progression. Strip `delta-arrow` chips that reference prev-year movement. All KPI-tree numbers carry `★` until first-year actuals exist; in #references add a line: "Y1 targets calibrated from [comparable-market source]; revise post first-quarter actuals."

- **#audience-lifecycle — Y1 forecast label.** The segments × funnel-stages matrix structure is fine; only the framing changes. Add at top of section, immediately after `<h3>`:
  ```
  <p class="intro-para text-mute"><em>Y1 forecast — segments × stages projected from category benchmarks; matrix re-baselined post-launch tracker wave (Q3 [year]).</em></p>
  ```
  No other structural change. Cell content remains 1 line per cell.

- **#assumptions — minimum 7 cards (vs default 3–7).** Launch plans rest entirely on hypotheses. The assumptions page is where they're inventoried with leading signals. Required coverage: market sizing (1+), segment behaviour (2+), unit economics (LTV, CAC, payback) (1+), competitive response (1+), regulatory / supply-side (1+), channel-mix viability (1+). Each card carries `claim + confidence + leading signal + breaker` per the standard structure.

- **#risks — minimum 7 risks (vs default 5–7), bias toward entry-specific.** Add at least:
  - 1 regulatory tail-risk specific to entry market (licensing, data residency, P2P transport rules)
  - 1 supply-side drag (driver / merchant / inventory acquisition cost vs forecast)
  - 1 incumbent retaliation scenario (price war, exclusivity push, regulatory capture)
  Anti-linearity rule from "Per-section content notes" still applies — at least 1 off-diagonal in each direction.

**Sections NOT changed by this modifier (use defaults):** market, consumers, competitors, swot, brand-pyramid, positioning, comm-matrix, product-initiatives, media (channel rows are forecasts but structure is identical), regions, comm-plan, references, exec, basics, next-steps. The modifier touches 6 sections, not 20 — the rest of the dashboard is stage-agnostic.

**Research Sweep addendum for GTM cases.** When stage modifier is triggered by GTM use case, add 2–3 queries to the Sweep batch on **comparable-market entry tactics** — pattern: `"[category] [target-region] new-entrant case study"`, `"[category] [region] market entry [year]"`, `"[adjacent-market peer] launch [region]"`. This populates the assumptions / business-goal / mmm-benchmark inputs that defending-share data can't.

## Must-cite checklist per section

For each section, certain facts MUST be `.ext`-sourced (not `★ hyp`). Use this as a baseline-factuality floor — if a must-cite fact comes back empty from web_search, do an ad-hoc search before falling back to ★. Order roughly matches generation wave order.

- **#market** — population, internet penetration, ad-market size, category market size: all 4 must cite (T1/T2). Trends rows: ≥4 of 6 must cite.
- **#consumers** — methodology source for segmentation must cite (DataReportal demographics or category survey); per-segment behavioural sensitivities can be ★.
- **#competitors** — direct competitor scale fact (MAU / users / cities / revenue): ≥2 of the listed competitors must cite. Indirect competitor scale: at least 1 must cite. Indirect "how they overlap" can be reasoned.
- **#brand-health** — if user has no tracker → whole section under `.hypothesis-badge` and benchmarks come from matched preset's "Brand-funnel benchmark" line; if tracker exists → wave source must cite.
- **#swot** — at least 1 `.ext` per quadrant (S/W/O/T); each strength fact must be sourced or marked ★.
- **#brand-pyramid** — internal; no must-cite. RTBs ideally cite (e.g. ranking, regulator stat) but ★ is acceptable for emotive RTBs.
- **#positioning** — internal logic; no must-cite. Axes from matched preset.
- **#audience-lifecycle / #comm-matrix** — internal logic; no must-cite.
- **#product-initiatives** — if a flagship references a partner / regulator / external launch (e.g. "DoTM compliance API"), that partner / event must cite.
- **#media** — channel reach numbers from DataReportal / GWI / WARC: ≥2 channel-reach values must cite. CAC / ROAS can be ★ in Mode 1.
- **#mmm** — internal budget; no must-cite (all hypothesis if user provides no spend data).
- **#regions** — population per region from national stats agency or DataReportal: ≥3 of regions must cite.
- **#comm-plan** — internal calendar.
- **#risks** — leading-signal source must cite for each Critical and High risk; mitigation cadence is internal.
- **#references** — IS the citation list; ≥8 entries; tier-balance T1+T2 ≥40% of total.
- **#business-goal** — at least 1 of: GDP / population / market-size from T1/T2 in the context block; KPI-tree numbers can be ★ in Mode 1.
- **#assumptions** — high-confidence assumptions must cite the leading-signal source (NRB / Department of Customs / DoTM / etc.); low-confidence can be ★.
- **#exec / #basics / #next-steps** — synthesis only. Citations come via `.tri` triangle links to evidence sections — no new citations needed.

## Voice and tone — single rule

Across all 20 sections, write **declarative, action-led prose**. Sentence length max 30 words; if you wrote a 40-word sentence, split it. No corporate hedging ("synergies", "leverage", "best-in-class"); no rhetorical questions; no MBA jargon. Concrete verbs over abstract nouns. The tone register stays the same from #exec through #references — a strategy doc reads like one author, not five.

## Token budget guardrails per wave

Realistic budget targets, calibrated from honest measurement of full end-to-end runs across multiple categories and modes. Two structural drivers explain why naïve estimates undershoot real runs by 2–3×: (1) `old_string` payloads in atomic Edits for 150–220-line sections (now mitigated by the >120-line split rule in Step 4); (2) PostToolUse preview-panel hook narration the runtime emits per Edit — skill-level "don't echo" rules can't fully override it.

| Wave | Sections | Mode 1 | Mode 2 | Mode 3 |
|---|---|---|---|---|
| 1 — Context | market, consumers, competitors, brand-health, swot | ≤55K | ≤75K | ≤95K |
| 2 — Brand | brand-pyramid, positioning, audience-lifecycle, comm-matrix | ≤30K | ≤40K | ≤55K |
| 3 — Execution | product-initiatives, media, mmm, regions, comm-plan | ≤60K | ≤90K | ≤120K |
| 4 — Governance | risks, references | ≤20K | ≤25K | ≤35K |
| 5 — Synthesis | business-goal, assumptions, exec, basics, next-steps | ≤25K | ≤30K | ≤40K |
| Interview + Research overhead | — | ≤10K | ≤40K | ≤75K |
| **Total target** | | **≤200K** | **≤300K** | **≤420K** |

Sessions running in 1M-context mode (large-context experimental setting) typically run 1.5–2× higher than these targets — that's a session-mode artefact (full transcript stays uncompressed in context), not a skill issue. Don't optimize against it. If a wave in a standard session exceeds its band by >25%, flag in the present-step summary so the user sees the cost signal.

## Anti-patterns — five hard rules

1. **No fact-looking number without `.ext`, `★ hyp`, or `[to be confirmed]` plate.** Any number must declare its state.
2. **No Exec / Basics / Next-steps before appendix is filled.** Synthesis without fact = platitude.
3. **No invented competitor MAU / segment sizes / KPI benchmarks without research backing.** If web_search returned nothing → `★ hyp` with explicit "no public data, category benchmark only" rationale.
4. **No wrapping demo prose in `<span class="ph">` to bypass placeholder checks.** `.ph` is for short numerics inside real prose. If you can't fill — use `[to be confirmed]` plate.
5. **No opt-out via leaving demo text.** If a section truly doesn't apply, replace its body with `<div class="card"><p class="intro-para text-mute"><em>Not applicable for this business — no [scope] in plan for this period.</em></p></div>`.

## Token discipline during generation

**Don't echo PostToolUse preview-panel hooks in user-facing text.** Every Edit triggers a "file is now visible in preview panel" reminder. Mention preview availability ONCE at the start of generation (after the first Edit), then stop repeating it — the user can already see the file. Each repetition costs ~30-50 narration tokens; over 40-50 edits per run that's 1.5-2.5K wasted tokens.

**Don't narrate every section completion.** Update TodoWrite at wave boundaries (Wave 1 done → Wave 2). Avoid "Section X done, moving to Y" announcements between sections within a wave.

## When in doubt

Open `references/prompting-notes.md` — opt-out pattern, mode-fit nudge, marker examples, and what "Research Sweep one batch" means concretely.
