# Outside-the-box insight types

Seven patterns for surfacing what the document **doesn't say**. This is the audit skill's differentiation. Without these, the output is a fancy checklist. With them, it's a second pair of eyes.

Every insight follows the 3-line structure:
- **Finding** (1 line, what was noticed)
- **Why it matters** (1 line, what's at risk)
- **Suggested action** (1 line, concrete next step)

---

## Type 1 — Cross-document inconsistencies *(multi-doc only)*

**What to look for.** When the user uploads 2+ documents, scan for places where they disagree on a fact or implication.

**Common patterns.**
- Strategy deck names target audience "Gen Z" — brand book taglines speak to 35+
- Brand book positions as "premium" — investor deck positions as "value"
- Marketing plan budget says $5M — finance memo says $3.2M is approved
- Q1 brief lists 5 priorities — annual plan emphasizes 3 different ones

**Example finding.**
- **Finding.** Brand book positions GreenSM as "premium EV ride" while strategy deck describes the target user as "value-conscious daily commuters"
- **Why it matters.** Creative will land on premium voice, media planning will hit price-sensitive audiences — they won't reinforce each other
- **Suggested action.** Reconcile positioning: pick one (the strategy's target user logic is harder to change than the brand book's voice — adapt the voice)

**How to find.** For each document, extract key claims: target audience, positioning, pricing tier, value prop, KPIs, budget figures, geographic scope. Compare matrices. Surface mismatches.

---

## Type 2 — Hidden assumptions

**What to look for.** Claims in the document that imply unstated bets. These are the assumptions on which the strategy rests, but which were never named — meaning they're never stress-tested.

**Common patterns.**
- "Grow 50% YoY" → assumes category itself doesn't contract
- "Win Gen Z" → assumes Gen Z attention stays on this category (not displaced by AI / new platforms)
- "Performance media will deliver 2× ROAS" → assumes CPMs don't inflate beyond projection
- "Launch in Q2" → assumes regulatory / supply readiness in Q2
- "Capture 15% share by Y3" → assumes no new well-funded entrant

**Example finding.**
- **Finding.** The 50% Y1 MAU growth target implicitly assumes the ride-hail category itself grows 8%+ YoY in Almaty
- **Why it matters.** Category contraction (recession-driven, or modal shift to public transit) would make the growth target unreachable even with flawless execution
- **Suggested action.** Add a category-growth assumption to the assumptions section with a leading signal (Statista Outlook KZ ride-hail) and a breaker (what happens if it's flat?)

**How to find.** For each major claim, ask: "What has to be true outside this document for this claim to hold?" That's the hidden assumption. If the assumption is named in the assumptions section — fine. If it's not — surface it.

---

## Type 3 — Missed opportunities in the uploaded data

**What to look for.** Data is present in the document — segments, channels, customer-research findings, competitive intelligence — but the strategy doesn't act on parts of it.

**Common patterns.**
- Document lists 6 customer segments, strategy only addresses 3 — what about the other 3?
- Competitive analysis surfaces a vulnerability in incumbent — strategy doesn't lean into it
- Research shows a creative theme that resonates 2× better than current voice — strategy doesn't adopt it
- Past-campaign post-mortem identifies what worked — strategy doesn't institutionalize it

**Example finding.**
- **Finding.** The consumer research section identifies "Family households" as a 22% segment with high frequency and price sensitivity, but the strategy treats Auto-buyers and Pro-drivers as the only two priority segments
- **Why it matters.** Family households represent ~20% of the addressable market with distinct media habits (TV, family OOH) that the current plan ignores entirely
- **Suggested action.** Add Family households as the 3rd priority segment with a dedicated media+creative track, even at 15–20% of envelope

**How to find.** For each piece of data in the document, ask: "Is this data referenced in any strategic decision later?" If a piece of data is presented but not used to inform a choice, it's a missed opportunity.

---

## Type 4 — Reality gap via web_search *(Full audit only)*

**What to look for.** Document's claimed positioning, brand strengths, or market understanding compared to observable external signals.

**Sources to check.**
- App Store / Google Play reviews and ratings (consumer experience)
- Google Trends (search interest momentum, comparison to competitors)
- Recent press / news (positioning narrative in market)
- SimilarWeb / Sensor Tower (scale fact check)
- Reddit / category forums (qualitative consumer sentiment)

**Common patterns.**
- Document claims "trusted" — App Store reviews flag trust complaints
- Document claims "#1 in category" — SimilarWeb shows #3
- Document claims "growing brand" — Google Trends shows flat or declining search interest
- Document claims "Gen Z favorite" — Sensor Tower demographics show 35+ skew

**Example finding.**
- **Finding.** Document positions GreenSM as "the modern, reliable EV taxi". App Store reviews of the GSM Vietnam parent app (the closest available proxy) average 3.8 stars with recurring complaints about app crashes and driver no-shows
- **Why it matters.** Y1 launch will inherit the parent app — if the same UX issues land in Almaty without fix, "reliable" claim collapses within the first 1,000 reviews
- **Suggested action.** Add an explicit pre-launch QA gate to product roadmap; track app-store ratings as a brand-health metric from week 1, not waiting for tracker waves

**How to find.** Pull 3–5 most central strategic claims. Run targeted web_search per claim. Cite findings with `.ext`.

---

## Type 5 — Strategic alternatives not considered *(Full audit only)*

**What to look for.** What plays do comparable brands in this category use that the document doesn't mention?

**Where to find alternatives.**
- Category presets from the companion `b2c-marketing-strategy` skill (if accessible)
- Quick research on "[category] [region] new-entrant strategy case study"
- Press coverage of analogous-stage moves by category leaders

**Common patterns.**
- Ride-hail launch document doesn't consider corporate-account play that Bolt/Lyft used at the same stage
- Fintech document doesn't consider referral economics that drove Revolut's early growth
- Streaming document doesn't consider exclusive-content lever that defined Netflix vs Hulu split

**Example finding.**
- **Finding.** Strategy includes consumer brand building + driver acquisition but doesn't reference a corporate-account / B2B play. Bolt Riga, GoCar Vietnam, and inDrive Lima all used corporate-account programs to anchor predictable demand in months 6–18 post-launch
- **Why it matters.** Corporate accounts smooth Y1 revenue volatility and lock airport / hotel ranks — both critical for a 5,500-fleet operation in a 2.35M city
- **Suggested action.** Evaluate adding a corporate-account program by Q3; even if not in Y1, name it as a deliberate Y2 priority so the plan doesn't read as silent on a known lever

**How to find.** Match brand to category. Pull category-leader strategic moves at comparable stage. Compare to what's in the document. Surface gaps.

---

## Type 6 — Pattern detection / blind spots

**What to look for.** Recurring themes that imply a bias.

**Common patterns.**
- All 5 risks are about competitors — no supply / regulation / macro risks named (competitive-bias)
- Every initiative is a brand campaign — no performance / CRM bets (channel-bias)
- All segments are demographic ("Gen Z", "35+") — no behavioural ("frequent users", "lapsed", "deal-hunters") (segmentation-bias)
- All success metrics are awareness-based — no retention / monetization / unit economics metrics (funnel-bias)
- Document references same source 12 times — no triangulation (source-bias)

**Example finding.**
- **Finding.** All 5 risks in the risk register are competitive moves (Yandex Go price war, inDrive super-app expansion, Bolt entry, Uber re-entry, regional roll-up by incumbent). No risks from supply (driver liquidity), macro (FX, fuel, recession), or regulatory (P2P transport rules) sides
- **Why it matters.** Competitive-only risks imply the team sees the world as a duel with rivals. Real Y1 collapse risks for a new-entrant ride-hail are more often supply (drivers don't show up) or regulatory (license framework changes) than competitive
- **Suggested action.** Force-add 1 risk per non-competitive category: 1 supply risk + 1 macro risk + 1 regulatory risk. Even if probability is low, naming them shifts the team's mental model

**How to find.** Group all items in each section by an implicit category. If 80%+ fall into 1–2 categories, that's a bias.

---

## Type 7 — Benchmark outliers *(Full audit only)*

**What to look for.** Claimed metrics that are unusual for the category — either much higher or much lower than typical.

**Where to anchor.**
- Category presets from `b2c-marketing-strategy` (brand-funnel benchmarks, channel-mix benchmarks, KPI typical ranges)
- Public industry reports (WARC channel-spend, GWI consumer benchmarks)
- Comparable-brand performance data via Sensor Tower / SimilarWeb

**Common patterns.**
- KPI target 3× the category benchmark (e.g. "5% conversion in a category where 1.5% is leader-band")
- Budget allocation outlier (e.g. 60% to Brand when category mean is 25%)
- Timeline outlier (e.g. expecting Y1 unit-economics positive when category typical is Y3)

**Example finding.**
- **Finding.** Y1 aided awareness target of 35% in Almaty is well above the new-entrant ride-hail category band (typical Y1 = 15–25% for a well-resourced launch). Comparable cases: Bolt Riga Y1 aided 19%, inDrive Lima Y1 aided 22%
- **Why it matters.** The 35% target sets up a likely Y1 miss unless the brand has unusual built-in awareness from the parent (e.g. consumer name recognition spilling over). If miss occurs in Q2 data, H2 plans need a contingency
- **Suggested action.** Either revise target to category-benchmark band (20–25%) OR document the parent-brand spillover mechanism that justifies the 35% claim. Currently the number floats without justification

**How to find.** Pull each numeric KPI target from the document. Compare to category-benchmark range. Flag those outside the typical band — high or low.

---

## Mode handling

**Quick score mode runs insight types 1, 2, 3, 6.** These are the lowest-cost insights — no web_search required. They surface what's in the document but not visible at first reading.

**Full audit mode runs all 7.** Adds Reality gap (4), Strategic alternatives (5), and Benchmark outliers (7) — the three that require web research.

---

## Output discipline

- Max 9 insight cards in any output (3 if Quick score, 5–9 if Full audit). Quality over quantity. If you can't find 3 substantive insights, the document is either very strong or very thin — name that explicitly.
- Each card is **1 line finding + 1 line why-it-matters + 1 line action**, never longer.
- Order by impact: most-actionable insight first.
- Group by insight type with type labels visible (so the user can see the variety of lenses applied).
- Never invent. If you can't trace an insight to a specific document fact, a cross-doc gap, a web_search hit, or a category-benchmark comparison — don't write it.
