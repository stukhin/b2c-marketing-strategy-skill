# Research sources

Deterministic catalog for the Research Sweep. The catalog is a **floor, not a ceiling** — if you spot a high-value source outside this list (recent regulator news, breaking story, niche specialist publication), add it. 1-3 ad-hoc additions per run is fine; 10+ defeats the speed-up.

**Total queries per Research Sweep:** 14-21. All in one parallel `web_search` batch.

---

## Tier 1 — Universal Digital DNA (always 9 queries)

Run these for every brief, regardless of category or market.

1. `DataReportal Digital [year] [country]` — population, internet penetration, social adoption, time spent, top platforms.
2. `GWI [country] media consumption [year]` — media mix per audience, online vs offline split, ad reception.
3. `Statista Outlook [country] [category]` — market size, projected growth, segment breakdown.
4. `Sensor Tower [Brand] [country] downloads MAU [year]` — app performance, top app peers.
5. `SimilarWeb [Brand].com traffic ranking [country]` — web traffic, top sources, audience overlap.
6. `World Bank [country] consumer spending GDP per capita` — economic backdrop.
7. `[Brand] vs [Top competitor 1] vs [Top competitor 2] Google Trends [country] 12 months` — search interest trajectory, momentum vs competitors, seasonality. Critical for `#brand-health` when no tracker exists and for `#market` trend direction.
8. `[Brand] App Store rating reviews [country] [year]` — current iOS rating, recent review sentiment, update cadence (signal of product velocity). Skim top reviews for pain points / praise themes that feed `#consumers` voice quotes and `#brand-health` attribute hypotheses.
9. `[Brand] Google Play rating reviews [country] [year]` — Android-side coverage. In emerging markets Android is the dominant platform, often a richer signal than App Store. Same usage as #8.

---

## Tier 2 — Macro & geo (always 2-4)

Pick based on country specifics.

- `[Country] central bank [year] inflation FX rate` — purchasing power, currency volatility (matters for regional or remittance plays).
- `[Country] national statistics agency [demographic data]` — official census / labor / consumer data (T1).
- `ITU [country] mobile penetration broadband` — connectivity baseline (matters for emerging markets).
- `IMF Datamapper [country] consumer confidence` — macro consumer signal.

---

## Tier 3 — Industry-specific (4-6 per matched category)

Match the brand's category via `industry-presets.md` (index) and the matched `presets/<category>.md`, then run the queries below for that category.

### 1. Classifieds marketplace
- `[Country] online classifieds market size leader share`
- `[Brand] [country] listings active sellers MAU`
- `[major classifieds incumbent] playbook revenue model [country]` *(comparison benchmark)*
- `[Country] used goods marketplace consumer survey`

### 2. Full-stack e-commerce
- `[Country] e-commerce GMV [year] forecast`
- `[Brand] take rate logistics fulfillment [country]`
- `[Country] online shopper survey checkout abandonment`
- `Statista [country] e-commerce categories share`

### 3. Jobs marketplace
- `[Country] online recruitment market size LinkedIn share`
- `[Brand] active job seekers employer accounts [country]`
- `[Country] white-collar vs blue-collar online job search`
- `[Country] gig economy labor market trends`

### 4. Real estate marketplace
- `[Country] online real estate listings volume agent share`
- `[Brand] property listings active agents [country]`
- `[Country] residential transaction volume [year]`
- `[Country] proptech investment landscape`

### 5. Fashion / resale marketplace
- `[Country] secondhand resale fashion market size [year]`
- `[major fashion-resale brand] [country] consumer adoption`
- `[Country] Gen Z fashion consumption survey`
- `[Country] secondhand authentication trust survey`

### 6. Digital banking & neo-wallet
- `[Country] neobank [leading brands] share customer count`
- `[Country] central bank fintech licensing regulation [year]`
- `[Country] consumer banking trust survey`
- `[Country] mobile wallet adoption GWI`

### 7. Payments & remittance
- `[Country] cross-border remittance volume corridors [year]`
- `[Brand] [country] payment volume merchant count`
- `[Country] central bank instant payment scheme adoption`
- `World Bank remittance prices [corridor] [year]`

### 8. Lending & BNPL
- `[Country] consumer credit penetration default rate`
- `[Country] BNPL Klarna Affirm [Brand] share growth`
- `[Country] regulator BNPL credit reporting rules`
- `[Country] underbanked population formal credit access`

### 9. Investment & trading consumer
- `[Country] retail investor count active brokerage [year]`
- `[Brand] AUM clients [country]`
- `[Country] regulator retail investor protection rules`
- `[Country] crypto trading adoption survey`

### 10. Ride-hail & mobility
- `[Country] ride-hail drivers daily trips [Brand] [year]`
- `[Brand] [country] take rate driver supply demographics`
- `[Country] taxi regulation ride-hail license framework`
- `[Country] urban mobility congestion EV adoption`
- *(optional, raises brand-health signal):* `[Country] [Brand] [Top competitor] customer review app store rating sentiment` — qualitative trust / safety / fair-pricing signals; helps replace ★ in #brand-health attributes when no internal tracker exists.

### 11. Quick-commerce / grocery
- `[Country] online grocery share q-commerce delivery time`
- `[Brand] [country] dark stores GMV growth`
- `[Country] consumer survey grocery online adoption`
- `[Country] cold chain logistics infrastructure`

### 12. Food delivery (restaurants)
- `[Country] food delivery market Uber Eats DoorDash [Brand] share`
- `[Brand] [country] restaurant partners orders per day`
- `[Country] consumer survey food ordering frequency`
- `[Country] takeaway vs dine-in spend split`

### 13. Subscription streaming
- `[Country] streaming subscribers Netflix Spotify [Brand] share`
- `[Brand] ARPU churn [country]`
- `[Country] consumer paid subscription stack survey`
- `[Country] piracy streaming substitution`

### 14. Edtech / online learning
- `[Country] online learning market K12 university Duolingo Coursera`
- `[Brand] [country] learners completion rate`
- `[Country] education spending consumer survey`
- `[Country] English language proficiency CEFR`

### 15. Travel / OTA
- `[Country] outbound inbound tourism volume [year]`
- `[Brand] [country] booking volume hotel air share`
- `[Country] travel consumer survey channel mix`
- `[Country] visa rules tourist arrivals`

### Adjacent / partial coverage

For telemed, insurtech, dating, gaming, social/UGC, crypto — search 3-4 queries combining the brand name + category-typical KPI + country regulator. Then fall back to Tier 1/7.

---

## Tier 4 — Country-specific (1-2 per matched country)

Per the matched market, consult the **regulator** + **national stats agency**.

- **Regulator:** `[Country] [regulator name] [category] [year]` — central bank, telecom regulator, competition authority, depending on category.
- **National stats:** `[Country] [stat agency name] [demographic / consumer / labor data]`.

If the country has a respected ad-trade publication (Sostav.ru for RU/CIS, Adobo Magazine for SEA, Campaign Asia, Campaign Middle East, Adweek for US/UK) — add 1 query for recent campaign news.

---

## Tier 5 — Competitor financials (always 1-2)

For each direct competitor:

- **Public:** `SEC EDGAR [Competitor] 10-K` / `[Competitor] HKEX annual report` / `[Competitor] IR latest quarter`
- **Private:** `[Competitor] [country] [year] revenue users news` — accept signal from news / investor blogs / Crunchbase / press releases. T3 source — qualitative claims only, no precise numbers without `★`.

---

## Tier 6 — Media + campaign intel (always 2)

- `WARC [country] [category] ad spend forecast [year]`
- `GroupM This Year Next Year [country] [year]` — total ad spend, channel split, growth.

Optional add-ons (1-2 if relevant):
- `Effie Awards [country] [category] [year-1] winners`
- `Cannes Lions [year-1] [country] winners`
- `Adweek [Brand] [country] campaign [year-1]`
- OOH-specific: `[Country] OOH spend Initiative Posterscope [year]`

---

## Tier 6.5 — Competitor communication examples (run when filling `#competitors` comm-examples block)

A targeted follow-up batch of 3-5 queries, separate from the main Research Sweep, run when populating the competitor communication examples sub-block. Without these, the comm-examples table fills with `★`-flagged "based on positioning history" copy, which is functionally invented content. With these, examples land with `.ext` citations to real campaigns.

**Approach: source-agnostic web search, recency-constrained.** Don't lock the search to a specific publication list — coverage of campaigns varies enormously by market (advertising press in some countries, agency newsrooms in others, social media accounts in others). Let general search surface whatever exists locally. Constrain by date so the examples reflect current positioning, not legacy creative.

For each named direct competitor (top 3-5 in `#competitors`), run:

- `[Competitor] [country] campaign [year-1..year]` — recent campaign coverage, any source
- `[Competitor] tagline [year]` — current positioning copy
- `[Competitor] [country] ad creative [year-1..year]` — visual / creative material

Cap examples to the **last 18-24 months**. A campaign from 2019 doesn't represent how the brand communicates today.

If a competitor returns nothing meaningful from these queries, mark the cell as a hypothesis (`★`) **and** add an inline note: `(no public campaign coverage found in last 24 months — inferred from positioning)`. An honest gap reads better than fabricated marketing language.

---

## Tier 7 — Out-of-category fallback

If brand is outside the 15 supported categories AND adjacent partial coverage doesn't fit:

- `McKinsey consumer survey [country] [year]`
- `BCG [category] outlook [year]`
- `Bain consumer goods [country]`

These give framework + hypothesis basis. Not enough for verified facts — output will be hypothesis-heavy. Set expectations with user accordingly.

---

## Source-tier classification — what to put in `title=`

When citing via `<a class="ext" href="..." title="...">`:

- **T1** — official / primary. World Bank, IMF, central bank, SEC EDGAR, IR pages, ITU, national statistics agency, regulator filings. **Cite as fact.**
- **T2** — research aggregator. DataReportal, GWI, Statista, Sensor Tower, SimilarWeb, McKinsey, BCG, Bain, WARC, GroupM. **Attribute the source.**
- **T3** — signal / editorial. Adweek, Adage, Bloomberg news article, niche publication, Reddit, AppFollow, Sostav, Adobo. **Qualitative claims only — never precise numbers.**

Format: `title="DataReportal Global Digital Report 2026 [T1]"` or `title="Statista Outlook E-commerce [Country] 2026 [T2]"` or `title="Adweek [Brand] [Region] campaign coverage 2025 [T3]"`.

If the source's tier is uncertain — default to T2.
