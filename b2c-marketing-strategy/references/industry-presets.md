# Industry presets — index

Pick the matching category for the brand, then read **only that category's preset file**. Don't load this whole file plus a category file — the per-category file has everything you need.

Each preset = typical segments / channel split benchmark / KPI metrics / category risks / strategic distinctness.

These are **starting frameworks**. Real businesses deviate — adjust based on user's intel + Research Sweep findings.

## 16 supported categories

| # | Category | Examples | File |
|---|---|---|---|
| 1 | Classifieds marketplace | OLX, Kijiji, Quikr, Marktplaats | `presets/classifieds.md` |
| 2 | Full-stack e-commerce | Trendyol, Wildberries, Shein | `presets/ecommerce.md` |
| 3 | Jobs marketplace | LinkedIn, HeadHunter, Indeed | `presets/jobs.md` |
| 4 | Real estate marketplace | Cian, Zillow, Idealista | `presets/realestate.md` |
| 5 | Fashion / resale marketplace | Depop, Poshmark, ThredUp | `presets/fashion-resale.md` |
| 6 | Digital banking & neo-wallet | Revolut, N26, Monzo | `presets/digital-banking.md` |
| 7 | Payments & remittance | Wise, Remitly, WorldRemit | `presets/payments.md` |
| 8 | Lending & BNPL | Klarna, Affirm, Tabby | `presets/lending-bnpl.md` |
| 9 | Investment & trading consumer | Robinhood, eToro, Trade Republic | `presets/investment.md` |
| 10 | Ride-hail & mobility | Uber, Bolt, Grab, DiDi | `presets/ride-hail.md` |
| 11 | Quick-commerce / grocery | Getir, Gorillas, Flink, Instamart | `presets/q-commerce.md` |
| 12 | Food delivery (restaurants) | Uber Eats, DoorDash, Deliveroo, Glovo | `presets/food-delivery.md` |
| 13 | Subscription streaming | Netflix, Spotify, Disney+ | `presets/streaming.md` |
| 14 | Edtech / online learning | Duolingo, Coursera, Udemy | `presets/edtech.md` |
| 15 | Travel / OTA | Booking, Expedia, Airbnb | `presets/travel.md` |
| 16 | Media / content / publishing | The Athletic, Substack, Bloomberg consumer, Vox Media | `presets/media-content.md` |

## Outside the 15

- **Adjacent / partial coverage** (telemed, insurtech, dating, gaming, social/UGC, crypto) — see `presets/adjacent.md`
- **First-principles fallback** for everything else — see `presets/fallback.md`

## Routing logic

1. Match brand to one of the 16 categories above by primary product / monetisation model.
2. If 2 categories seem to fit (e.g. food delivery + q-commerce), pick the one that owns the largest share of GMV today and read that preset; flag the secondary in your fill notes.
3. If nothing fits, check `presets/adjacent.md`. If still no match, use `presets/fallback.md`.
4. Read **only the matched preset file** — not this index, not all categories.
