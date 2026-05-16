# Interview questions

Mode-aware question banks. Don't ask anything outside the spec. Asking too many questions in Mode 1 defeats the "fast" promise. Question selection is **prioritized by impact-per-question** — derived 2026-04-27 from the 40-question priority bank (`40-question-bank.md` for full list + rationale).

---

## Document uploads — Mode 2 and Mode 3

In Mode 2 and Mode 3, **ALWAYS** offer document upload **at the start of the interview**, immediately after Q1-Q2 (brand+market and use-case+stage are needed first to make the upload prompt useful — without those the doc parse has no anchor). The upload prompt is mandatory — never skip it in Mode 2/3.

> "Paste / attach any of: brand book, last year's marketing plan, latest annual report, investor deck, segmentation study, brand tracker waves, or any internal strategy doc. Even one file materially sharpens output and reduces the number of questions you'll be asked. Skip if unavailable."

**Do not scan the user's filesystem for documents on your own.** Only parse files that the user has **explicitly attached to the chat** or **explicitly named with a path**. Don't open Downloads / Documents / Desktop folders speculatively to find what "might be relevant" — this surfaces unrelated files, violates privacy expectations, and burns permission prompts on irrelevant reads. If the user said "skip, no documents" — respect that and proceed to the next question.

Then **parse uploaded content first**, then ask only the remaining questions whose answers aren't already in the docs. For partially-answered questions (doc has revenue band but not channel split), ask only the missing slice.

**Caveat — parent-level vs market-level documents.** Watch the document scope before deciding which questions to skip. If the upload is a **parent-level** corporate doc (10-K, group annual report, parent-brand investor deck, board-level strategy memo), it carries macro context but does not answer market-level questions about the specific country / brand-market combination at hand. In that case, parse the upload for **Research Sweep enrichment only** (parent metrics, group strategy, holding-level ESG and risk language), and **still ask Q3–Q9 in full** at the market level. Only **market-level** documents (in-country marketing plan, brand tracker for that market, segmentation study commissioned for that market, post-campaign report on local creative) can legitimately reduce the question count. When the document is somewhere in between (a regional report covering several countries), ask Q3–Q9 for the country at hand and use the doc to calibrate hypothesis ranges.

---

## Mode 1 — Fast draft (2 questions, no upload)

**Q1.** What's the brand and what market? (format: "[Brand] in [Country / Region]" — e.g. "[Marketplace brand] in [Region]", "[Food-delivery brand] in [Country]")

**Q2.** What's the use case for this strategy and what stage is the business at?
- *Use case examples:* annual plan, investment pitch, brand re-launch, GTM for new market, internal alignment doc.
- *Stage:* launch (first 12 months) / scale (growth phase) / mature (defending share) / decline (stabilising or rebuilding).

After these 2 — proceed to Confirm + Research Sweep + Generate. ~70-80% of the doc will be hypothesis-marked ★.

---

## Mode 2 — Guided (Q1-Q2 + optional upload + 7 fillers)

After Q1-Q2, run the upload prompt above. Parse any uploaded docs, then ask only the Q3-Q9 questions not already answered.

**Q3.** Customer segments — describe 3-5 distinct customer cohorts. For each: rough size (% of online-active or absolute), age band, JTBD ("they hire your product to ___"), price / trust / convenience sensitivity (high / medium / low), core geography, channels they actually use.

**Q4.** Marketing envelope for the year — either as % of revenue (e.g. "8-12%") or absolute (e.g. "$5-7M"). It's OK to give a band.

**Q5.** Top 3 priorities this year (channels you want to grow, segments to acquire, problems to solve).

**Q6.** Direct competitors — top 4-5 of them. For each: 1-line current positioning + main risk you see them creating for you in the next year.

**Q7.** Last year's actual channel mix + ROAS / CAC payback by major channel (Performance / Brand / Influence / CRM / etc.). Order-of-magnitude is fine.

**Q8.** Top 3-5 flagship product launches / feature rollouts in scope this year — name + 1-line what + target quarter + rough budget if known.

**Q9.** Brand pyramid foundations — essence (1 phrase compressed truth), purpose (1-sentence ambition for what the brand makes possible), 3-5 personality adjectives, 3-6 RTBs (reasons to believe — concrete facts with quantifiers).

After this — Confirm + Research + Generate. Hypothesis density: ~30% if docs covered most of Q3-Q9, ~50% if no docs.

---

## Mode 3 — Deep interview (Q1-Q2 + optional upload + 20 fillers)

After Q1-Q2, run the upload prompt above. Parse any uploaded docs, then ask only the questions not already answered. For partially-answered questions, ask only the missing slice.

**Q3-Q9** — same as Mode 2 (Customer segments / Envelope / Top 3 priorities / Direct competitors / Channel mix + ROAS / Flagship launches / Brand pyramid foundations).

**Q10.** Active brand tracker — do you run YouGov / Kantar / Ipsos / internal? If yes — top-line numbers latest wave (Top-of-mind, Aided awareness, Consideration, Trial, NPS). If no — say so, we'll mark `#brand-health` with `.hypothesis-badge` and calibrate against category preset's brand-funnel benchmark.

**Q11.** Northstar metric + 1-2 lagging KPIs you ultimately judge the year on. Add 2025 actuals + 2026 targets across funnel stages (Awareness → Acquisition → Activation → Retention → Monetization → Profitability).

**Q12.** Positioning statement — one-line: "We help [audience] who [problem] to [outcome]. Unlike [alternatives], we [unique mechanism]." Plus the functional axes you believe drive consumer choice in your category (the 2-axis frame for the positioning map).

**Q13.** Regional / city-tier strategy — where is share concentrated, where do you defend, where do you expand? Top metros / regions + cluster logic.

**Q14.** What did 2025 miss? One thing you'd fix in the previous year's strategy. (Drives the assumptions trigger logic + SWOT weaknesses.)

**Q15.** Risk tolerance — what would force you to re-plan mid-year? (Regulator move / channel cost spike / competitor acquisition / partner exit / macro shock.) Top 3-5 specific risks ranked by probability × impact.

**Q16.** Cultural / seasonal calendar in this market — major moments to lean into or work around (Ramadan / Lunar NY / monsoon / back-to-school / national holidays / cultural sensitivities).

**Q17.** Always-on streams — performance acquisition, driver/supplier recruitment, CRM lifecycle. Cadence + budget share for each.

**Q18.** Functional axes for positioning maps in your category (which 2 dimensions drive consumer choice — convenience × control / price × trust / library × discovery / etc.). If unsure, we'll use the matched preset's axes.

**Q19.** LTV : CAC, contribution margin, payback period — actuals where available. (Drives `#business-goal` profitability stage.)

**Q20.** T1 vs T2 vs T3 campaign tier split — how many 360° T1 campaigns vs T2 digital pushes vs T3 experiments planned for the year.

**Q21.** Voice-of-customer quotes per priority segment — pull 1-2 direct quotes from CS / NPS / qualitative sessions for each. Even rough paraphrases work.

**Q22 (final upload reminder).** Anything else you have as a file — past strategy docs, brand book, segmentation study, brand tracker waves, customer research deck — that you haven't already shared? If yes, attach now; we'll fold the data in before the Confirm step.

After Q22 — Confirm + Research + Generate. ~10-20% hypotheses, only where genuinely no data.

---

## Question-asking style

- Ask in **batches of 3-5** in Mode 2/3, not one at a time. Wastes turns.
- Allow "skip" / "don't know" — the answer becomes `[to be confirmed]` or `★ hyp`.
- Don't paraphrase questions to fish for richer answers. Ask what the spec says, accept what user gives.
- After the last question, ALWAYS run Step 2 — Confirm — before Research Sweep. User must approve the framing before web_search burns context on a wrong premise.

---

## How question selection was prioritized

The 7 Mode 2 questions and 20 Mode 3 questions were selected from a 40-question priority bank ranked by **impact-per-question** = (number of dashboard sections the answer unblocks) × (depth of `★ hyp` → fact replacement). The full 40-question list with rationale is preserved in `40-question-bank.md` for future re-prioritization. If user reports a Mode 2/3 run feeling "thin" in a specific area, check the bank for a question that targets that area and consider promoting it.
