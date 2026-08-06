# Rei v2.4.0 — Session 4 Upgrade Notes

## Deep Link Handler (NEW)
- **DeepLinkHandler.kt** — Parses incoming intents from:
  - `anilist.co/anime/{id}` (HTTPS, autoVerify)
  - `myanimelist.net/anime/{id}` (HTTPS, autoVerify)
  - `www.myanimelist.net/anime/{id}` (HTTPS, autoVerify)
  - `rei://anime/{id}` (custom scheme)
  - `rei://anilist-auth-callback` (OAuth)
  - `rei://mal-auth-callback` (OAuth)
- **AndroidManifest.xml** — 3 new intent-filters with `android:autoVerify="true"` for AniList and MAL domains
- **MainActivity** — `handleIncomingDeepLink()` on `onCreate` and `onNewIntent`, sets `pendingDeepLinkId` state, navigates via `LaunchedEffect`
- **Share links** — `DeepLinkHandler.buildAnimeLink()`, `buildAnilistWebLink()`, `buildMalWebLink()` for shareable URLs

## AI Recommendation Engine (NEW — No API Key Required)
- **RecommendationEngine.kt** (309 lines) — On-device content-based filtering:
  - **Genre affinity (40% weight)** — Jaccard similarity weighted by user score
  - **Tag affinity (20% weight)** — Cross-references AniList tags from top 30 tracked anime
  - **Studio affinity (15% weight)** — Boost for studios the user already likes
  - **Format preference (10% weight)** — TV, movie, OVA preference matching
  - **Score alignment (10% weight)** — Recommends anime in similar quality tier
  - **Popularity decay (5% weight)** — Slight boost for hidden gems under 50K popularity
  - **Genre diversification** — Max 3 recommendations per primary genre to avoid repetition
  - **Dropped anime penalty** — Negative weight for genres in dropped anime
  - **"Because you liked X"** — `similarTo(animeId)` finds anime similar to a specific title
  - **100% on-device, zero API key, instant results**
- **RecommendationScreen.kt** (216 lines) — Premium UI:
  - "Powered by Rei AI" dark banner
  - Stats chips: N picks, avg match %, N genres
  - Rec cards with match % badge (color-coded), reason chips, score + genre chips
  - Shimmer loading, empty state with "Track More Anime" CTA
- **RecommendationViewModel.kt** — Calls `engine.recommend(limit=25)`
- **Route** — `Route.Recommendations`
- **HomeScreen** — "For You" quick action added (replaces Quiz in row 2)

## Mythic Rarity Tier (NEW — Ultra-Premium Shop)
- **5 rarity tiers**: Common → Rare → Epic → Legendary → **Mythic**
- **Mythic color**: Crimson red `#FF1744` — visually distinct from Legendary orange
- **10 Mythic items** (1,200–3,500 coins each):
  | Item | Cost | Description |
  |------|------|-------------|
  | Void Walker Theme | 2,000 | Animated void particles with crimson-black gradient |
  | Cosmic Avatar Ring | 1,800 | Orbiting celestial bodies around your avatar |
  | Phoenix Resurrection | 2,500 | Rising phoenix flame on completing anime |
  | Dragon Sovereign | 1,500 | Writhing dragon badge with animated scale shimmer |
  | Holographic Card | 1,200 | Rainbow holographic sheen that shifts with scroll |
  | Infinite Crown | 3,000 | Floating crown with golden particle aura |
  | Nebula Navigation | 1,600 | Living nebula background behind navigation |
  | Chromatic Glow | 1,400 | Every UI element pulses with rainbow chromatic shift |
  | Eternal Flame | 2,200 | Persistent flame animation on your profile |
  | Omniscient View | 3,500 | Unlock every visual feature simultaneously |
- **Mythic Ascension Bundle** — All 10 Mythic items for 4,999C (18,000C value = 72% discount)
- **ShopItem** expanded — `description: String` and `glowHex: Long` fields for premium items
- **EconomyScreen** — Mythic items render with:
  - Dark void background (`#1A0A2E`)
  - Crimson animated border (1.5dp)
  - Larger emoji (24sp) in white
  - Full description text (2 lines)
  - Crimson price badge with ❂ symbol
  - Crimson rarity badge with white text
- **Total shop items**: 105 (was 93)
- **14 categories** (was 13): added "Mythic" tab

## Quality & Polish
- **NetworkUtil.kt** (115 lines) — New utility:
  - `retryWithBackoff()` — Exponential backoff (1s→2s→4s) with configurable max retries
  - `flowWithRetry()` — Flow-based retry for repository flows
  - `ReiContentDescription` — 25+ accessibility content description constants for TalkBack
  - `NetworkState<T>` — Sealed class: Idle, Loading, Success, Error(isOffline, lastSyncMs)
  - `formatLastSync()` — Human-readable "last synced X ago" for error states
- **Accessibility** — Content description constants for: anime cards, score badges, hero carousel, all navigation, track/fav/share buttons, coin balance, streak, shop items, recommendations, quiz, genre chips, search bar, pull-to-refresh

## Final Project Stats
- **80 Kotlin files** · **12,284 lines** · **24 screens** · **25 routes** · **8 APIs** · **3 utilities**
- **105 shop items** across **5 rarity tiers** and **14 categories**
- **0 brace mismatches** · **0 emojis** · **5 Unicode rarity symbols**
