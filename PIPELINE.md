# CITY HISTORY — Video Production Pipeline (Master File)

> One file you can paste into Claude, ChatGPT, Notion or a Google Doc and run for **every** video.
> It was built from a NexLev teardown of @HistoryTabOpen and 25+ city-history competitors (see `research/`).
> Follow the steps top to bottom. Every step has: **Goal → Tools → Exact prompt/settings → Output → Done-check**.

---

## 0. Channel blueprint (decide once)

| Decision | Recommendation | Why (data) |
|---|---|---|
| Niche | **Cities only**: "the entire story of one city per video" | HistoryTabOpen is broad (countries + a weapon + a politician). City channels like Midtown History (4–5x outliers) and Majestic (London 3.2M) show a cities-only series works and is bingeable. |
| Language | English (main). Optional 2nd channel in Urdu/Hindi later | English city videos get higher RPM. Delhi/Lahore have 1M+ demand in Hindi/Urdu. |
| Length | **10–14 min** (≈1,400–2,000 words at 140–150 wpm) | HistoryTabOpen 8–12 min, Midtown 11–13 min. Gives enough room to cover the *full* founding→today arc, which HTO failed to do and got complaints for. |
| Cadence | 2–3 uploads/week to start, then scale to daily | HTO uploads every ~3 days. Consistency matters more than volume in the first 60 days. |
| Visual identity | Pick **ONE** style and never change it (see §6) | Comments on the 1M video praise "illustrated-book style" and "maps". A consistent look builds recognition. |
| Series name | e.g. "Cities of the World", "City Files", "One City, Every Century" | Playlists drive session time. |
| Channel name ideas | City Chronicles · Every Century · Old Streets · The City Files · Cityscope History · Urbis | Check availability on YouTube and as a domain. |

---

## 1. Pipeline overview

```
[1] FIND topic ──► [2] VALIDATE (Gap test) ──► [3] TITLE (3 options)
        │                                              │
        ▼                                              ▼
[4] RESEARCH fact sheet ──► [5] SCRIPT ──► [5b] FACT-CHECK
                                               │
        ┌──────────────────────────────────────┘
        ▼
[6] SHOT LIST + VISUALS (images, maps, motion) ──► [6b] VOICE ──► [6c] EDIT
        │
        ▼
[7] THUMBNAIL (2 variants) ──► [8] DESCRIPTION/TAGS/CHAPTERS ──► [9] UPLOAD ──► [10] 48h REVIEW → feed back into [1]
```

Track every video in one Google Sheet (columns in §11).

---

## 2. STEP 1 — Find viral city topics (where to "scrape" titles)

**Goal:** a list of city topics that have **already proven demand**. Never guess.

### 2.1 Sources, ranked

| # | Source | What you take from it | How |
|---|---|---|---|
| 1 | **NexLev → Channel Outliers** of competitors | Cities that hit 2x+ on competitor channels | Run on every channel in `data/competitors.csv` weekly |
| 2 | **NexLev → Faceless Outliers (semantic)** | Fresh viral city videos from small channels | Query below |
| 3 | **NexLev → Viral videos from small channels** | Breakouts in the last 90 days | Keyword "history" / "city" |
| 4 | **NexLev → Similar Channels** | New competitors appear every week | Run on your top 3 competitors monthly (level 3) |
| 5 | **YouTube search** (incognito) | Gap check: does a strong English video exist? | Search `entire history of {city}`, sort by view count |
| 6 | **Google Trends** (YouTube Search filter) | Seasonality and news spikes | Compare 5 cities at a time |
| 7 | **News** (Google News, Reuters, BBC) | Cities in headlines this week | HTO's biggest hit was a news-adjacent topic (Israel) |
| 8 | **Wikipedia page views** (pageviews.wmcloud.org) | Real public curiosity per city | Spikes = topic window |
| 9 | Competitor **comments** | "Do {city} next!" requests | NexLev → Video Comments on competitor outliers |

### 2.2 NexLev recipes (dashboard or via Claude + NexLev MCP)

**A. Competitor outliers (weekly)**
```
Tool: youtube_channel_outliers
channel_id: <from data/competitors.csv>
max_videos: 100
min_outlier_threshold: 1.5
→ copy every city/place title into the Idea Sheet with views + outlier score
```

**B. Semantic search for viral city videos**
```
Tool: faceless_outliers_videos
query: "the entire history of a famous city explained from founding to today"
videoType: long
minOutlierScore: 2
minViews: 50000
minUploadDate: <90 days ago>
languages: ["english"]
```
Also run these queries: `"what a city looked like in the year X, AI reconstruction"`, `"rise and fall of a once rich city"`, `"why nobody visits this city anymore"`, `"ancient lost city discovered"`, `"how a city was built engineering"`.

**C. Keyword search with outlier filter**
```
Tool: search_videos
query: "entire history of"   isExactMatch: true
minView: 100000  minLength: 300  startDate: <12 months ago>
sortBy: viewCount  englishOnly: true  limit: 100
→ filter out non-history results (games/anime). Keep cities, regions, empires.
```

**D. New competitors**
```
Tool: get_similar_channels   channelId: UC0ra0vN_iF5AwwU7G8q79rw (Majestic)   level: 3   async: true
→ poll get_similar_channels_status
Tool: search_niche_finder_channels
query: "history of cities documentary, how a city was founded and grew, city history explained with maps"
channelCreatedAfter: <6 months ago>   minOutlierScore: 1.5
```

**E. Ready-to-paste Claude prompt (with NexLev connected)**
```
Use NexLev. For each channel ID below, get outliers (min 1.5x, last 100 videos).
Then run faceless_outliers_videos for "entire history of a city" (long, last 90 days, min outlier 2).
Return ONE table: City | Best video title | Channel | Views | Outlier | Video age | Length.
Remove anything that is not about a city/town/ancient city. Sort by outlier.
Channels: UCcJyQ5dihoKAKwk2aRi-CQw, UC0ra0vN_iF5AwwU7G8q79rw, UC3qwLJfv34PEcI_OV5WHN0Q,
UCeaJ37P2B1QUGgsI0RyFugw, UC27rIYEr46EU4jFrKd98YBQ, UCAYM3gdjwLdNYRUTDoDEk7A, UCKWRnsyYbPFgWclCF0_yOTw
```

### 2.3 Idea scoring (score 1–5 each, keep ≥ 18/25)

| Criterion | 5 = |
|---|---|
| **Proven demand** | Same city has a 500K+ video anywhere, or 2x+ outlier on a small channel |
| **Gap** | No strong English 10–15 min video in the last 2 years |
| **Drama** | Fire, plague, siege, empire, collapse, rebirth, "shouldn't exist" |
| **RPM geography** | US/UK/EU/AUS city = 5, Middle East = 3, South Asia = 2 |
| **News/season hook** | City in headlines, anniversary, Olympics/World Cup host, major film/game |

### 2.4 Gap test (2 minutes per city)
1. YouTube incognito: `entire history of {city}` and `history of {city}`.
2. Count English videos 8–40 min with **>100K views uploaded in the last 24 months**.
   - 0 → **green** (make it).
   - 1–2 → **yellow** (make it with a different angle or title, e.g. "rise & fall", "in year X").
   - 3+ → **red** (skip unless you have a unique angle).
3. Check the top video's comments for complaints ("didn't cover X", "too long", "AI slop"). That is your angle.

---

## 3. STEP 2 — Title

**Goal:** 3 title options, then pick one. Under 70 characters, with the city name in the first 40.

### 3.1 Formulas that won in this niche

| # | Formula | Proven example |
|---|---|---|
| T1 | `The ENTIRE History of {City} in {N} Minutes` | London in 24 Minutes: 3.2M · Israel in 10 Minutes: 1.07M |
| T2 | `The Entire History of {City}: {Superlative hook}` | Nineveh: The Largest City on Earth, Erased From History |
| T3 | `{3-item hook}: The Entire History of {City}` | Vikings, Empire, Beatles and Beyond: The Entire History of Liverpool (14.8x) |
| T4 | `{City} Was {shocking fact}, {consequence}` | Seattle Was Raised 22 Feet in 1907: The Original City Is Still Underneath (86x) |
| T5 | `The Rise and Fall of {superlative}: {City}` | Rise and Fall of Europe's Richest Oil City: Aberdeen |
| T6 | `Why Nobody Wants to Visit {City} Anymore` | Blackpool 170K · Torbay 97K |
| T7 | `How {group} Secretly Ran {City}` | Brighton 131K · Tenerife 295K |
| T8 | `A Tour of {City} in {Year}: {superlative}` | Babylon in 570 BC (22.6x) |
| T9 | `{City} {Year}: {dramatic event} (AI Reconstruction)` | London 1300: The Apocalypse Happened in 1348 (1.27M) |
| T10 | `{City} Shouldn't Exist` / `The City That Refused to Die` | Venice, Istanbul variants |

### 3.2 Rules
- Use T1 for the "series" videos, because it is searchable. Use T2–T10 for browse virality. Mix about 50/50.
- **Do not say "ENTIRE" unless you cover founding → today.** HTO got a 103-like complaint for that.
- One CAPS word maximum. No clickbait you don't pay off in the first 60 seconds.
- Use YouTube "Test & Compare" (title/thumbnail A/B) when available.

### 3.3 Title prompt
```
You are a YouTube packaging strategist for a faceless city-history channel.
City: {CITY}. Key dramatic facts: {3-5 facts from fact sheet}.
Write 10 titles using these formulas: T1..T10 (from my list).
Rules: <70 chars, city name in first 40 chars, max 1 CAPS word, no lies.
Then pick the top 3 and say which thumbnail concept fits each (in 5 words).
```

---

## 4. STEP 3 — Research (fact sheet)

**Goal:** a 1–2 page fact sheet of **verified** facts with sources, before writing anything.

### 4.1 Sources
- **Spine:** Wikipedia "History of {City}" plus its *references section* (follow 3–5 refs).
- **Encyclopedic:** Britannica, World History Encyclopedia (worldhistory.org), UNESCO World Heritage pages.
- **Primary / academic (for the description citations):** JSTOR open articles, Google Scholar, university press books (cite author, year, title, publisher, as HTO does).
- **City sources:** official city archive or museum websites, the national library.
- **Maps (for visuals):** David Rumsey Map Collection, Old Maps Online, Library of Congress maps, GeaCron (historical borders), Wikimedia Commons (filter: public domain).
- **Paintings/engravings (for AI reconstruction style):** Wikimedia Commons, Met Museum Open Access, Rijksmuseum, British Library Flickr (public domain).

### 4.2 Fact-sheet template
```
CITY:
Founded: (date, by whom, why HERE: geography reason)
Name origins / old names:
Population curve: (founding → peak → today, 4-6 data points)
ERAS (5-8): name | dates | 2-3 key events | 1 vivid detail | source URL
TURNING POINTS (3): disasters, conquests, reinventions
MYTHS TO BUST (2-3): "popular belief X, actually Y" + source
SURPRISING STATS (3):
TODAY: one modern fact that ties back to the past
COLD-OPEN SCENE: exact date + time + place + person + sensory detail
CONTESTED POINTS: (present both sides neutrally)
CITATIONS (5): author, year, title, publisher/journal
```

### 4.3 Research prompt
```
Build a fact sheet for the history of {CITY} using the template below.
Only include facts you are confident are accurate. Mark any uncertain item with [VERIFY].
For each era give a source I can check (Wikipedia section or Britannica or academic book).
Find one exact, dated, cinematic moment for the cold open (date, time if known, person, place).
Find 2-3 popular myths about {CITY} and the correction.
Template: <paste 4.2>
```
➡️ Then **open and check every [VERIFY] item yourself**. Accuracy is the moat. History comment sections punish mistakes.

---

## 5. STEP 4 — Script

**Goal:** 1,400–2,000 words (10–14 min), retention-engineered, built on the HTO beat structure that produced a 1M-view video.

### 5.1 Beat sheet (timings for a 12-min video at ~145 wpm)

| # | Beat | Time | Words | What to write |
|---|---|---|---|---|
| 1 | **Cold open: a dated scene** | 0:00–0:40 | ~95 | "At {time} on {date}, {person} stood in {place}…" Present a pivotal moment (fire, siege, founding, coronation). Sensory detail plus stakes. |
| 2 | **The paradox** | 0:40–1:10 | ~70 | "And yet… {city} is still here." / "This is the strange thing about {city}…" A contradiction that makes the viewer need the story. |
| 3 | **Whole-arc teaser** | 1:10–1:40 | ~70 | One fast sentence per era ("Romans built it. Vikings burned it. Plague emptied it…"). Shows the payoff exists. |
| 4 | **Question stack (4–6 open loops)** | 1:40–2:10 | ~70 | "So how did a {humble origin} become {today}? Why did {X}? What happened when {Y}?" |
| 5 | **Rewind triad** | 2:10–2:20 | ~25 | "To understand that, we have to go back. Before {A}, before {B}, before {city} even had a name." |
| 6 | **Geography: why HERE** | 2:20–3:00 | ~95 | River, crossing, harbor, hill, trade route. **Map beat.** |
| 7 | **Eras 1…N** (5–7 eras) | 3:00–11:00 | ~1,150 | Each era: date → event → vivid detail → consequence → **forward-pull line** ("But something was coming…"). Add 1 myth-bust every 2–3 eras. |
| 7a | **Mid-roll CTA** (once) | ~40% mark | ~30 | "Quick one: if you're enjoying this, subscribe. We're doing a new city every {N} days. Now, back to {year}." |
| 8 | **Modern day + callback** | 11:00–11:40 | ~95 | Today's city in 3 facts, then call back to the cold-open scene ("that street where {person} stood in {year} is now…"). |
| 9 | **End-screen bridge** | 11:40–12:00 | ~45 | Tease the next city with an open loop: "But {city} wasn't the only city that refused to die. {Next city} was burned {N} times…" Then point to the end screen. **Never** "Part 2 coming soon" for cities. Finish the story. |

### 5.2 Style rules (from HTO + Majestic transcripts)
- Short declarative sentences. Mix 4-word punches with 20-word sentences.
- Rule of three ("invasion, fire, plague").
- **Present tense for scenes** ("The year is 43 AD. Four Roman legions have just landed…"), past tense for analysis.
- Every 60–90 seconds: a turn word ("But", "Then", "And yet") plus a stakes change.
- Numbers make it real: dates, populations, distances.
- Contested topics: say "according to tradition…" versus "archaeology shows…". Name all communities.
- No filler ("in this video we will…"). No "Hey guys" intro.
- Write for the ear. Read it out loud once before voicing.

### 5.3 Master script prompt
```
ROLE: You write scripts for a faceless YouTube channel about the history of cities.
Voice: cinematic, clear, slightly witty, never sensational. Target {WORDS} words (~145 wpm).

INPUT:
- City: {CITY}
- Title: {TITLE}
- Fact sheet: {PASTE FACT SHEET}

STRUCTURE (follow exactly, label each beat in [brackets] for the editor):
[COLD OPEN] dated cinematic scene, present tense, 80-100 words
[PARADOX] the contradiction that makes this city's story strange, 60-80 words
[ARC TEASER] one fast sentence per era, 60-80 words
[QUESTIONS] 4-6 open-loop questions the video will answer
[REWIND] "To understand that, we have to go back. Before…, before…, before…"
[GEOGRAPHY] why the city exists exactly here (map beat)
[ERA 1..N] 5-7 eras. Each: date → event → vivid detail → consequence → forward-pull last line.
   Insert 2-3 [MYTH] beats: "You might have heard… Actually…"
   Insert ONE [CTA] at ~40%: short subscribe line, then "Now, back to {year}."
[TODAY] 3 modern facts + callback to the cold-open scene
[NEXT] tease next city ({NEXT_CITY}) with an open loop, point to end screen

RULES:
- Only use facts from the fact sheet. If you need a fact that is not there, write [NEED FACT: …].
- Every claim with a number or date must come from the fact sheet.
- Short sentences. Rule of three. No "in this video". No "Part 2".
- Neutral, respectful language on religion, ethnicity, war.
- After the script, output: (a) word count, (b) estimated runtime at 145 wpm,
  (c) list of every [MYTH], date and number used, for fact-checking.
```

### 5.4 Fact-check prompt (run in a NEW chat)
```
You are a strict history fact-checker. Here is a YouTube script about {CITY}.
List every factual claim (dates, numbers, names, causal claims) in a table:
Claim | Correct? (Yes / No / Disputed / Unsure) | Correction | Best source.
Flag anything that is a popular myth stated as fact. Be harsh.
SCRIPT: {PASTE}
```
Fix every No, Disputed or Unsure before voicing.

### 5.5 Retention self-check (must all be YES)
- [ ] First sentence contains a date or a number, plus a person or a place
- [ ] Paradox is stated by 1:10
- [ ] At least 4 open loops are opened before 2:15
- [ ] Every era ends with a forward-pull line
- [ ] No section longer than 90 s without a "But/Then/And yet" turn
- [ ] The title promise is paid off (e.g. founding → today if "ENTIRE")
- [ ] Ending teases the next city (not "Part 2")

---

## 6. STEP 5 — Visuals and editing style

> ⚠️ HTO's editing was inferred from viewer comments ("illustrated-book style", "ancient-era maps", "what AI do you use for these graphics"), NexLev's `AI content: true` flag and its About page ("dynamic cartography"). NexLev's video-watch quota ran out during research. **Before your first video, watch HTO's Israel video (0:00–2:30) and Majestic's London video (0:00–2:00) and fill in §6.6.**

### 6.1 Choose ONE style

| | **Style A: Illustrated Atlas** (HTO-like) | **Style B: AI Reconstruction** (Majestic / Midtown-like) |
|---|---|---|
| Look | Painterly "history-book" illustrations plus animated historical maps | Photoreal "you are there" scenes of the city in year X, animated from old paintings and engravings |
| Proof | Israel 1.07M on an 18-day-old channel | London 3.2M, Las Vegas 646K (9.5x), Brisbane 269K (5x) |
| Effort per video | Medium (stills + map animation) | Higher (image-to-video clips) |
| Policy | Low risk (clearly illustration) | Must tick YouTube's **"altered or synthetic content"** disclosure for realistic scenes |
| Best for | Ancient and medieval eras, borders, empires | Cities with rich 1600–1950 visual records (London, Paris, NYC, Istanbul) |

**Recommendation:** **Style A as the base, plus 3–6 Style-B "reconstruction" hero shots per video** (cold open, 2–3 turning points, ending). That gives a unique hybrid look that the same-format clones don't have.

### 6.2 Visual spec (Style A base)

| Element | Spec |
|---|---|
| Aspect / res | 16:9, 1920×1080 minimum (4K if possible; "4K" in title/badge helps in this niche) |
| Image style block (put in every prompt) | `detailed storybook illustration, painterly gouache texture, warm parchment tones, soft cinematic lighting, historically accurate clothing and architecture, no text, no watermark, 16:9` |
| Consistency | Same style block, same seed/style reference image, same 3–4 colour palette for all videos |
| Shot length | Stills held **3–5 s** each (≈ 1 visual per sentence). Faster (2–3 s) during wars and disasters; slower (5–7 s) for reflective lines |
| Motion on stills | Slow push-in or pan (Ken Burns) 105–115% scale; subtle parallax (separate foreground layer) for hero shots |
| Maps | Every era starts with a map beat: zoom to city → show borders/rulers at that date (colour-coded) → animate expansion, routes, sieges with arrows. Parchment map texture to match the illustrations |
| On-screen text | Only **dates, place names, numbers** (e.g. "LONDINIUM · 43 AD"). One serif font (e.g. Cinzel, Trajan-style) + one clean sans for small labels. White/cream with soft shadow |
| Transitions | Mostly hard cuts. Cross-dissolve between eras. A "map zoom" between geographic jumps. Avoid flashy CapCut presets |
| Colour grade | Warm, slight vignette, light film grain (unifies AI images) |
| Music | Orchestral/ambient history bed at −22 to −28 LUFS under the voice. Change track at each era. A rise at the paradox and at turning points |
| SFX | Low whoosh on map zooms, rumble on disasters, crowd/market ambience under city scenes, quiet page-turn between eras |
| Captions | Upload a proper .srt (HTO has none; easy win) |

### 6.3 Shot-list prompt (turns the script into an edit plan)
```
Turn this script into a shot list for a faceless history video.
Style A = illustrated storybook still; MAP = animated historical map; RECON = photoreal AI reconstruction hero shot; TEXT = date/place title card.
For EVERY sentence output one row:
# | Script line | Shot type | Visual description (subject, era-accurate details, camera move) | Duration (s) | On-screen text | SFX
Rules: 1 visual per sentence, 3-5 s each, a MAP at the start of each era, 3-6 RECON hero shots total
(cold open, turning points, ending), no anachronisms, describe clothing/architecture for the exact year.
SCRIPT: {PASTE}
```

### 6.4 Image-prompt template (per shot)
```
{Visual description from shot list}, {CITY} in {YEAR}, {specific architecture/clothing/objects of that year},
detailed storybook illustration, painterly gouache texture, warm parchment tones, soft cinematic lighting,
historically accurate, no text, no watermark, 16:9
```
Recon hero shot: replace the style block with `photorealistic, cinematic 35mm, natural light, period-accurate, documentary still, 16:9`. If you have a public-domain painting of that exact scene, use it as the image reference.

### 6.5 Tool options (pick any; all have equivalents)
- Images: Midjourney / Flux / Ideogram / Google Imagen (use one consistently)
- Image to video (hero shots): Kling / Runway / Google Veo / Luma
- Maps: MapChart.net (fast), QGIS (precise), GeaCron (historical borders reference), After Effects or CapCut keyframes for animation, Google Earth Studio for modern-day flyovers
- Edit: CapCut / Premiere Pro / DaVinci Resolve
- Voice (not analyzed here): any natural narrator voice; keep the same voice forever
- Music/SFX: YouTube Audio Library, Epidemic Sound, Artlist

### 6.6 Verify-by-watching checklist (fill once, then lock your style)
Watch HTO Israel 0:00–2:30 and Majestic London 0:00–2:00. Write down:
- [ ] Average seconds per visual: ___
- [ ] % illustrations / maps / AI video / stock / archival: ___
- [ ] Camera motion on stills (zoom %, pan direction): ___
- [ ] Map style (colours, borders, labels, arrows): ___
- [ ] Text overlay font/colour/position: ___
- [ ] Music style and when it changes: ___
- [ ] SFX used: ___
- [ ] What happens visually in the first 5 seconds: ___
Then add these to §6.2 so every editor follows the same spec.

---

## 7. STEP 6 — Thumbnail

**Goal:** 2 variants per video for A/B testing. Readable at phone size.

### 7.1 Rules
- **One focal subject**: the city's most iconic landmark/skyline OR a dramatic moment (fire, siege, plague). The landmark must be recognisable in 1 second.
- **Era contrast** works best for cities: left half = ancient/medieval city, right half = modern skyline (or burning vs rebuilt).
- **Text: 0–3 words**, never repeating the title (e.g. "2,000 YEARS", "BURNED 7 TIMES", "43 AD → TODAY"). Big, bold, high-contrast.
- Same visual style as the video (illustrated or recon) plus a consistent brand element (e.g. thin gold frame or a small map pin icon in the same corner).
- High saturation, a warm vs cool contrast (fire orange vs night blue).
- Test at 160×90 px: can you still read it and recognise the city?

### 7.2 Three templates
1. **Then → Now split:** `[ancient {city}] | [modern {city}]` + text "2,000 YEARS".
2. **Drama moment:** `{city} burning / under siege / flooded` + text "{N} TIMES".
3. **Map hero:** stylised old map of the city with a glowing route/border + landmark cut-out + text "{YEAR}".

### 7.3 Prompt
```
Design 2 YouTube thumbnail concepts for "{TITLE}".
Audience: history fans, mobile. Style must match: {Style A or B block}.
For each: composition (left/right/centre), focal subject, background, colour contrast,
2-3 word text overlay (not repeating title), and a 1-line image-generation prompt.
```

### 7.4 NexLev thumbnail research
- `get_similar_thumbnails` (text): e.g. "ancient city vs modern skyline split thumbnail", to see what is already working and avoid copying exactly.
- `generate_thumbnail` / `edit_thumbnail` (NexLev) can draft variants from your title.
- Save winners to a NexLev **Swipe File** folder "City thumbnails".

---

## 8. STEP 7 — Description, tags, chapters

### 8.1 Description template (modelled on HTO's 1M-view video)
```
The history of {CITY}: {one-line hook with main keyword}. This {CITY} history documentary covers
{era 1}, {era 2}, {era 3} and {modern day} — {N} years in {M} minutes.

{One "wow" stat paragraph: oldest/largest/first fact.}

In this video: {comma list of 15-25 named entities: founders, rulers, battles, buildings, disasters,
districts, famous people, events} … and how {CITY} became the city it is today.

{2-3 sentence dramatic closing: "Rome could not hold it. Fire could not erase it…"}

This documentary is based on archaeological research, academic sources and primary historical records.

CHAPTERS
0:00 {Cold-open scene name}
0:40 Why {CITY} is strange
2:20 Why {CITY} exists here
{mm:ss} {Era 1}
{mm:ss} {Era 2}
…
{mm:ss} {CITY} today

SOURCES
{Author (Year), "Title," Publisher/Journal}  ×5

#{CityHistory} #History{City} #{City} #History #Documentary #HistoryDocumentary #WorldHistory #{Country}History #CityHistory #{Era} #{Landmark}
```

### 8.2 Tags (15–25)
`history of {city}`, `{city} history`, `entire history of {city}`, `{city} documentary`, `history of {city} in 10 minutes`, `{city} explained`, `{old name of city}`, `{country} history`, `{key era} {city}`, `{landmark}`, `animated history`, `history summarized`, `city history`, `documentary`, plus **common misspellings** of the city (HTO used "isreal"; e.g. "istambul", "jerusalam").

### 8.3 Prompt
```
Write the YouTube description, 20 tags and chapters for this video using my template.
Title: {TITLE}. Script with [beat labels]: {PASTE}. Runtime: {mm:ss}. Fact sheet citations: {PASTE}.
Put the main keyword "{city} history" in the first 150 characters. Include common misspellings in tags.
```

---

## 9. STEP 8 — Upload checklist

- [ ] File named `{city}-history.mp4`
- [ ] Title (from §3), description, tags, chapters (from §8)
- [ ] Thumbnail A uploaded; B queued in Test & Compare
- [ ] **Altered/synthetic content disclosure = YES** if any realistic AI reconstruction is used
- [ ] Captions: upload the edited .srt
- [ ] Category: Education. Language set. Made for kids: No
- [ ] Playlist: "Cities of the World" (+ continent playlist)
- [ ] End screen: next-city video + subscribe; cards at the [NEXT] tease
- [ ] Pinned comment: a question that sparks debate ("Which era of {city} would you visit?") + "Which city next?"
- [ ] Publish at a consistent time (US afternoon / UK evening works for English history; test with your Analytics "When your viewers are on YouTube")
- [ ] Add the row to the tracking sheet

---

## 10. STEP 9 — 48-hour review loop

| Check | Where | Healthy | If bad |
|---|---|---|---|
| CTR | YouTube Studio | ≥ 5–6% | Swap thumbnail (B) then title (alt from §3) |
| Avg view duration | Studio → retention | ≥ 40% at 12 min | Find the drop point and fix that beat type in the next script |
| First 30 s retention | Studio | ≥ 70% | Strengthen the cold open (more specific scene, faster first visual) |
| Views vs channel avg | NexLev `get_my_top_videos` / `get_my_video_analytics` | ≥ 1x | – |
| Traffic sources | NexLev `get_my_traffic_sources` | Browse + Suggested growing | If only Search, make the titles/thumbnails more browse-y (T2–T10) |
| Audience geo | NexLev `get_my_geography_report` | – | Lean topics toward high-RPM geos |

**Double-down rule:** any video at ≥2x your channel average → make 2–3 follow-ups in the same region or angle within 2 weeks (e.g. London hits → Manchester, York, Liverpool; "Why nobody visits X" hits → 3 more).

---

## 11. Automation blueprint

### 11.1 Tracking sheet (Google Sheets: one row per video)
`ID | City | Score(/25) | Gap(G/Y/R) | Angle | Title A | Title B | Status | FactSheet link | Script link | Word count | Shot list link | Voice file | Edit file | Thumb A | Thumb B | Description | Upload date | URL | Views 48h | CTR | AVD% | Outlier | Notes`

Status flow: `Idea → Validated → Researched → Scripted → Fact-checked → Voiced → Visuals → Edited → Thumb → Scheduled → Live → Reviewed`

### 11.2 What to automate and what stays human

| Step | Automate? | How |
|---|---|---|
| Idea finding (§2) | ✅ weekly | Claude + NexLev MCP prompt §2.2-E on a schedule, appending to the sheet |
| Scoring + gap test | ⚠️ semi | Claude scores; **you** confirm the gap in incognito YouTube |
| Fact sheet | ✅ draft | Claude prompt §4.3 → **human verifies [VERIFY]** |
| Script | ✅ draft | Prompt §5.3 → fact-check prompt §5.4 in a separate chat → **human edit pass** |
| Shot list + image prompts | ✅ | Prompt §6.3 + template §6.4 |
| Images / video clips | ✅ batch | Image tool API or batch mode using the prompts column |
| Voice | ✅ | TTS API from the final script |
| Maps | ⚠️ semi | Reusable map templates per region; human adjusts borders |
| Edit | ⚠️ semi | Template project (intro, text styles, music beds, SFX). The editor drops in assets |
| Thumbnail | ⚠️ semi | Prompt §7.3 → generate → human picks and fixes the text |
| Description/tags | ✅ | Prompt §8.3 |
| Upload | ✅ | YouTube Data API or scheduled upload in Studio |
| 48h review | ✅ | Claude + NexLev "my channel" tools → writes CTR/AVD back to the sheet |

### 11.3 Example no-code flow (n8n / Make / Zapier)
```
Trigger (Mon 9am) ─► Claude+NexLev: find outliers → append ideas to Sheet
Sheet row Status=Validated ─► Claude: fact sheet → Google Doc → Status=Researched (notify you)
You set Status=Approved ─► Claude: script → 2nd Claude: fact-check → Doc → notify
You set Status=Script OK ─► Claude: shot list + image prompts → Sheet tab
                         ─► Image API: generate all stills → Drive folder
                         ─► TTS API: voice.mp3 → Drive
                         ─► Claude: description/tags/chapters → Sheet
Editor finishes ─► Status=Edited ─► Claude: thumbnail concepts → Image API → Drive
You approve ─► YouTube API: scheduled upload with metadata
+48h ─► Claude+NexLev: pull analytics → write CTR/AVD/outlier → if ≥2x, create 3 follow-up ideas
```

---

## 12. Policy and risk (read once)

- **Inauthentic / mass-produced content:** YouTube can demonetize channels that look template-spammed. Protect yourself with human fact-checking, original script structure, a consistent narrator and custom maps, and by not uploading near-identical videos.
- **AI disclosure:** tick "altered or synthetic content" for realistic AI scenes of real places and events.
- **Copyright:** use competitors for *topics and structure only*. Never copy their script text, footage, maps or music. Use public-domain paintings and maps (check the licence on Wikimedia/museum pages).
- **Contested cities** (Jerusalem, Gaza, Kashmir, Taipei, Kyiv…): neutral wording, both narratives, no graphic violence. This keeps ads on and comments civil.
- **Accuracy:** one viral factual error can dominate the comments. The fact-check step is not optional.

---

## 13. First 30 videos (starter queue)

Order = mix of proven demand, gaps and high-RPM geographies. Re-validate each with §2.4 before scripting.

| # | City | Suggested title angle | Why |
|---|---|---|---|
| 1 | London | T1 "The ENTIRE History of London in 12 Minutes" | 3.2M + 1.5M proven; high RPM |
| 2 | Jerusalem | T2 "…: The Most Fought-Over City on Earth" | 2.3M + 657K (small channel) |
| 3 | Rome | T1 | 9.2M proven |
| 4 | New York | T4 "New York Was Once Called New Amsterdam…" / T1 | 1.27M; US RPM |
| 5 | Istanbul | T10 "The City That Refused to Die" | 470K + 630K; many weak clones, so differentiate |
| 6 | Paris | T1 | 1.5M + 949K |
| 7 | Berlin | T1 (English) | **Gap**: German version 250K, English <30K |
| 8 | Dubai | T5 "From Pearl Village to Megacity" | **Gap**: demand (1.2M Al Maktoum doc), weak supply |
| 9 | Baghdad | T5 "The Greatest City on Earth, and How It Fell" | **Gap**: only Cogito (4 yrs old) |
| 10 | Venice | T10 "Venice Shouldn't Exist" | 1.45M Epic History; Thomas 474K |
| 11 | Babylon | T8 "A Tour of Babylon in 570 BC" | 478K (22.6x), 517K |
| 12 | Las Vegas | T1 / "From Desert to Sin City" | 646K (9.5x) |
| 13 | Los Angeles | T1 | 223K (4.1x) |
| 14 | Constantinople 1453 | T9 | Proven drama |
| 15 | Pompeii | T9 "Pompeii: The Day Before" | 648K |
| 16 | Tokyo / Edo | T4 "Tokyo Was Destroyed Twice in 22 Years" | Japan topic proven on HTO |
| 17 | Mexico City / Tenochtitlan | T4 "Built on a Lake" | 1.39M (ES) |
| 18 | Delhi | T2 "The City Destroyed and Rebuilt 7 Times" | Hindi 1–4M; English gap |
| 19 | Cairo | T1 | Check gap |
| 20 | Alexandria | T5 "Rise and Fall of the Ancient World's Greatest Library City" | Check gap |
| 21 | Chicago | T4 "Chicago Burned Down in 1871, and Came Back Taller" | US RPM |
| 22 | Sydney | T1 | 123K (Midtown) |
| 23 | Edinburgh | T1 | UK RPM, check gap |
| 24 | St Petersburg | T4 "A City Built on a Swamp by Order of One Man" | Check gap |
| 25 | Athens | T1 | Check gap |
| 26 | Lahore | T1 | Test for an Urdu/Hindi spin-off |
| 27 | Detroit | T5 "Rise and Fall of America's Motor City" | Paul McAllister angle proven |
| 28 | Seattle | T4 (underground city) | 468K (86x) |
| 29 | Hong Kong | T5 | Check gap |
| 30 | Damascus | T2 "The Oldest Continuously Inhabited City?" | Check gap; keep it balanced |

---

## 14. Per-video checklist (copy for every video)

```
VIDEO #__  CITY: ________   STYLE: A / A+B
[ ] Idea score ≥18/25        [ ] Gap test: G / Y / R
[ ] 3 titles written → chosen: __________________
[ ] Fact sheet done, all [VERIFY] checked, 5 citations
[ ] Script (beat labels) ____ words ≈ __ min
[ ] Fact-check table: 0 No/Disputed left
[ ] Retention self-check 7/7
[ ] Shot list + image prompts
[ ] Visuals generated (stills __ / maps __ / recon __)
[ ] Voice recorded / generated
[ ] Edit to spec §6.2, captions .srt
[ ] Thumbnails A + B (tested at 160×90)
[ ] Description + chapters + 5 sources + tags (with misspellings)
[ ] AI disclosure set correctly
[ ] End screen + playlist + pinned comment
[ ] Scheduled ____  → 48h review logged
```
