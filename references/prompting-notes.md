# Prompting notes

Quick reference for marker usage, opt-out pattern, mode-fit nudge, and Research Sweep mechanics.

---

## The three markers — when each one fires

Every fillable line lands in one of three states. Never leave a fact-looking line without one of these plates.

### Verified — `.ext` blue dot

```html
[Claim or number]<a class="ext" href="https://datareportal.com/digital-2026" title="DataReportal Global Digital Report 2026 [T1]"></a>
```

- **T1** — official / primary source (regulator, IR filings, World Bank, ITU, national statistics agency). Cite numbers as fact.
- **T2** — research aggregator (Statista, GWI, McKinsey, Sensor Tower, SimilarWeb). Attribute source name when citing precisely.
- **T3** — signal / editorial (Adweek, Bloomberg news article, niche publication, Reddit, AppFollow). Qualitative claims only — never precise numbers.

The `.ext` link content is always EMPTY — the source name surfaces in the `title` tooltip. Don't write source name inline; it overflows the 7px circle styling.

### Hypothesis — `★ hyp` superscript star

```html
$<strong>5-7M</strong><span class="hyp">★</span>
```

The star is small (9px), gray, superscript. **Visual marker only — no hover behaviour, no tooltip.** Place it immediately after the value it qualifies, no space. Use for:
- Numbers Bob inferred from category benchmarks
- Predicted competitor reactions
- Sensitivity profiles where no segment study exists
- KPI targets where no public data confirms them

The rationale for the hypothesis lives in the surrounding prose (e.g. "calibrated against [category]-leader benchmark band"), not on the marker itself. Earlier versions carried a `data-hyp="<rationale>"` attribute for a hover plate; that was removed because the plate was rarely read and the cursor change distracted from the document. Don't add `data-hyp` — it's inert.

### To be confirmed — `[to be confirmed]` plate

```html
<span class="tbd">[to be confirmed]</span>
```

Use for genuinely-internal facts the user did not provide and Bob cannot benchmark:
- Specific people / role names
- Internal tracker setup ("we use BrandIndex monthly waves" yes/no)
- Past campaign results
- Any operational specific that has no public proxy

In Mode 1 / 2 this is common. In Mode 3 — only if user explicitly said "don't know" or "not yet decided".

---

## Opt-out pattern for sections that don't apply

Some sections legitimately don't apply to certain businesses:

- `#mmm` (channel-level MMM) — if marketing budget is under $500K, MMM is over-engineered. Replace section body with the opt-out card.
- `#regions` — if business operates in only one city / one metro area without regional differentiation, opt out.
- `#brand-health` — if no tracker exists AND user is not committing to one, flag the whole section as `.hypothesis-badge` rather than fabricating numbers.
- `#product-initiatives` — if there are no flagship launches in scope this year, opt out.

Opt-out markup (replaces the section's content, KEEPS heading + sidebar nav entry):

```html
<section id="mmm">
  <div class="heading-row">
    <h3 class="h-sub">Budget allocation (MMM view)</h3>
  </div>
  <div class="card">
    <p class="intro-para text-mute">
      <em>Not applicable for this business — marketing envelope below the threshold where channel-level MMM yields actionable signal. See Media approach for channel-level decisions.</em>
    </p>
  </div>
</section>
```

**Don't fabricate.** Better an honest opt-out than a fake table that nobody trusts.

---

## Mode-fit nudge logic

After user answers Q1-Q2, before continuing the interview, check:

- Is the brand a **household name** in a major market with rich public coverage? (Mature operator in a tier-1 market with active press / DataReportal / Sensor Tower coverage — yes. Some unknown $5M B2C app — no.)
- Has the user shown **time pressure** in their initial framing? ("quickly", "fast draft", "for tomorrow's meeting" — pushes Mode 1.)

If both — **nudge once**:

> Notice [Brand] in [market] has substantial public coverage — DataReportal, public app data, recent news. Guided or Deep with even one document (your last marketing plan, an investor deck) gives sharper output than Fast. Want to pause for an upload, or proceed Fast?

Don't push twice. Don't lecture. User's choice stands either way.

---

## Research Sweep — what "one batch" means concretely

ALL `web_search` calls go in **a single tool-use message**. Not "12 sequential web_search'es". Not "search a few, then search more later". One batch.

Concretely: in your tool message, include 12-18 `web_search` calls in one shot:

```
[message with parallel tool calls]
  web_search(query="DataReportal 2026 [country] digital adoption")
  web_search(query="GWI 2026 [country] media consumption habits")
  web_search(query="[Brand] active users 2026 SimilarWeb")
  web_search(query="[Brand] [country] launch news 2025")
  web_search(query="[competitor] [country] MAU 2026")
  ... 7-13 more ...
[/message]
```

Why this matters: scattered web_search'es through the build burn 30-50% more context AND give Bob the option to "I'll just search this one thing now mid-section" — which is the slippery slope HANDOFF describes. Front-load everything, generate from in-context findings only.

After the batch returns: skim, extract key data points, note them inline as you fill the doc. Don't summarize the search results into a separate doc — that's wasted context.

---

## Confirm step — what's actually in scope

After interview, write ONE LINE:

```
[Brand] · [stage] · [primary market] · [use case]. OK to proceed?
```

Example shape: `[Brand] · [stage: launch / scale / mature / decline] · [primary market] · [use case: annual plan / GTM / re-launch / investor pitch]. OK to proceed?`

If user corrects framing — incorporate, re-confirm. Only once user OKs explicitly do you call Research Sweep. **Never** burn web_search budget on a wrong premise.

---

## What to do if a section truly has no data

Mode 1, brand the user named is obscure, web_search returns near-nothing, user can't help further:

- Flag the section with `.hypothesis-badge` at the top (a yellow ⚠ pill saying "Hypothesis-baseline — no verified data available; replace when tracker / research lands").
- Fill the section with `★ hyp` markers throughout; explain the analogous benchmark used in the surrounding prose, not on the marker.
- In the final summary to user, name this section explicitly as "hypothesis-heavy, recommend you commission [tracker / research] to validate".

Don't fake it. Don't skip it (sidebar nav must match). The badge + ★ markers + summary callout makes the limitation visible, which is the honest thing.

---

## HTML structural integrity check

After generation, eye-check that:
- Every sidebar `nav-item` `data-target="..."` matches a real `<section id="...">` in the document body (otherwise the nav scrolls to nothing).
- Every `<a class="tri" href="#...">` evidence link points to an existing section.
- The first 4 themes-swatch buttons in sidebar (Vichy / Steel / Opaline / Mono) work — click and check theme switches.
- The "Download .docx" button in sidebar fires (loads docx.js from unpkg, generates client-side).

If any of those break — you most likely deleted a section by accident. Re-check; restore if needed.
