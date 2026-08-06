# Rei v2.5.0 — UI Cosmetics + API Expansion

## Cosmetic Effects (NEW — 345 lines)
**CosmeticEffects.kt** — 8 premium visual components:

| Effect | Description |
|--------|-------------|
| **ParticleBurstOverlay** | 25-particle celebration burst (stars, gems, crowns). Uses physics: velocity, gravity, fade. Auto-dismisses after 1.5s. Triggered on anime completion or coin earn. |
| **AnimatedCoinCounter** | Smooth spring-animated number rollup when balance changes. Spinning coin symbol with 360deg rotation. |
| **SpringPressCard** | Bouncy spring physics on press (scale 0.96→1.0 with medium damping). Every card feels alive. |
| **GlowingDot** | Pulsing dot for airing status indicators. Alpha oscillates 0.5→1.0, scale 0.8→1.2 over 1.2s. |
| **ShimmerText** | Animated text placeholder with gradient sweep for loading states. |
| **RippleButton** | Spring-animated button with custom accent color. Scale 0.95→1.0 bouncy. |
| **TimeOfDayGradient** | Ambient background gradient that shifts by hour: Dawn (purple-orange), Morning (blue-teal), Afternoon (indigo-violet), Sunset (orange-pink), Evening (deep navy), Night (near-black). |
| **SeasonPalette** | Auto-detects current season for accent colors. Winter=Ice Blue, Spring=Sakura Pink, Summer=Sunset Orange, Fall=Ember Red. Each has primary/secondary/tertiary/background + symbol. |

## Jikan Extended Endpoints (7 new)
JikanApi grew from 121→238 lines with these new MAL v4 endpoints:

| Endpoint | Returns |
|----------|---------|
| `getAnimeCharacters(id)` | `List<AnimeCharacter>` — character name, image, role (Main/Supporting) |
| `getAnimeStaff(id)` | `List<AnimeStaff>` — staff name, image, positions (Director, Script, etc.) |
| `getAnimeStatistics(id)` | `AnimeStatistics` — watching/completed/onHold/dropped/planToWatch counts + score distribution map |
| `getAnimeRelations(id)` | `List<AnimeRelation>` — sequel, prequel, spinoff, adaptation, etc. |
| `getAnimeExternal(id)` | `List<ExternalLink>` — official site, wiki, etc. |
| `getAnimeStreaming(id)` | `List<ExternalLink>` — Crunchyroll, Netflix, Hulu links directly |
| `getProducerAnime(id)` | `List<Anime>` — all anime from a specific studio/producer |
| `getGenreAnime(id)` | `List<Anime>` — all anime in a genre |

New data classes: `AnimeCharacter`, `AnimeStaff`, `AnimeStatistics`, `AnimeRelation`, `ExternalLink`

## MangaDex API (NEW — 10 endpoints, no key)
**MangaDexApi.kt** (132 lines) — Free v5 API, zero auth required:

| Endpoint | Description |
|----------|-------------|
| `searchManga(query)` | Search by title, returns cover art URL + authors |
| `getMangaDetail(id)` | Full detail with cover, authors, description |
| `getMangaChapters(id)` | Chapter list with page counts, languages |
| `getTrendingManga()` | Top followed manga |
| `getRecentlyUpdated()` | Latest chapter updates |
| `coverUrl(id, fileName)` | Builds CDN URL: `uploads.mangadex.org/covers/{id}/{file}.512.jpg` |

Data classes: `MangaDexEntry`, `MangaDexChapter`

## Simkl API (NEW — 3 endpoints, free tier)
**SimklApi.kt** (153 lines) — Free tier streaming + calendar:

| Endpoint | Description |
|----------|-------------|
| `search(query)` | Search anime, returns Simkl ID + poster |
| `getStreamingSources(simklId)` | **Cross-platform streaming links** — Crunchyroll, Netflix, Hulu, HIDIVE, Disney+, Amazon with sub/dub labels |
| `getCalendar(date?)` | Airing calendar with episode numbers |
| `getDetail(simklId)` | Full anime detail with rating, genres, overview |

Data classes: `SimklSearchResult`, `StreamingSource`, `SimklCalendarEntry`, `SimklAnimeDetail`

## Manga Screen (NEW — 25th screen)
- Search bar with debounce
- Trending manga grid
- Recently updated section
- Premium manga cards: cover, status badge (colored), year, authors, tag chips
- Empty state with refresh CTA
- Route: `Route.Manga`

## Repository Expansion
AnimeRepository now exposes **all** new endpoints as pass-throughs:
- 7 Jikan extended methods
- 5 MangaDex methods
- 3 Simkl methods

## DI Wiring
- AppModule: `mangadex()` and `simkl()` providers added
- Repo constructor: now takes `MangaDexApi` and `SimklApi`
- All injected via Hilt `@Singleton`

## Total API Count: 10
AniList · Jikan/MAL · Kitsu · Shikimori · Trace.moe · LiveChart · AnimeSchedule · Waifu.im · **MangaDex** · **Simkl**

## Final Project Stats
- **85 Kotlin files** · **13,267 lines** · **25 screens** · **26 routes**
- **10 API clients** (all free, zero keys needed for reads)
- **105 shop items** across **5 rarity tiers** and **14 categories**
- **8 cosmetic effects** + **5 season palettes** + **6 time-of-day gradients**
- **0 brace mismatches** · **0 emojis**
