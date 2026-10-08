# Cities History — Faceless YouTube System

A research-backed system for a faceless **city-history** YouTube channel, modelled on what made
[@HistoryTabOpen](https://www.youtube.com/@HistoryTabOpen) (countries) go viral, and adapted to cities.

## Files

| File | What it is |
|---|---|
| **[`PIPELINE.md`](PIPELINE.md)** | **The master file.** Step-by-step pipeline for every video: finding viral topics (NexLev recipes), titles, research, script structure + prompts, editing/visual spec, thumbnails, description/tags, upload, analytics loop, automation, first 30 videos. |
| [`research/01-competitor-teardown-historytabopen.md`](research/01-competitor-teardown-historytabopen.md) | Full teardown of @HistoryTabOpen: stats, outliers, topic sources, title/script/description patterns, editing evidence, weaknesses. |
| [`research/02-city-niche-data.md`](research/02-city-niche-data.md) | 25+ city-history competitors, proven city outliers, gaps, RPM benchmarks, winning title formulas. |
| [`data/competitors.csv`](data/competitors.csv) | Competitor list with channel IDs (for NexLev outlier runs). |

## Key findings

1. **HistoryTabOpen** is 18 days old with 5.1K subs. One video (Israel, **1.07M views, 4.2x**) brings ~90% of its views. Topics come from **proven outliers of bigger channels** (This Is History: Japan 3.1M, Jerusalem 2.3M) plus **news-cycle subjects** (Israel, Iron Dome, Trump). Est. RPM ≈ $4.
2. **Their script formula:** dated cinematic cold open → paradox → question stack → "before X, before Y" rewind → geography → eras with cliffhangers → myth-busting → mid-roll CTA → Part-2 cliffhanger. ~145 words/min.
3. **The city niche is proven:** London 3.2M (Majestic Studios), Rome 9.2M, Jerusalem 2.3M and 657K (from a 4K-sub channel), Paris 1.5M, York 1M, Las Vegas Strip 646K, Brisbane 269K (5x).
4. **Warning:** the title "Entire History of X in N Minutes" is saturating. Dozens of new channels copy it with <1K views. **Angle, visual identity and accuracy** are what separate winners.
5. **Gaps right now:** Berlin (English), Dubai, Baghdad, Delhi (English) and more. See the research file.

## Roman Urdu mein khulasa (summary)

- `PIPELINE.md` aapki main file hai. Har video ke liye isko upar se neeche follow karein.
- Topics "scrape" karne ka tareeqa: NexLev mein competitors ke **outliers** nikalain (§2), phir YouTube par **gap test** karein (§2.4).
- Script ke liye §5 ka prompt use karein, phir **fact-check** ka prompt alag chat mein chalayein.
- Editing ke liye ek hi style choose karein (§6). Recommendation: Illustrated Atlas + kuch AI reconstruction shots.
- Pehli 30 videos ki list §13 mein hai. Har video se pehle gap test zaroor karein.

> Note: HistoryTabOpen's editing style is inferred from comments and metadata, because NexLev's video-watch quota ran out during research. Fill in the checklist in `PIPELINE.md` §6.6 after watching their first 2 minutes.
