# Rei 零 — Upgrade Notes (2026-08-06)

## Screens Polished

### HomeScreen — Premium Overhaul
- **Dynamic greeting** based on time of day (Good Morning/Afternoon/Evening/Late Night)
- **Notification badge** on bell icon (red dot for unread)
- **Premium Quick Actions** with colored icon circles (Schedule/Gallery/Stats/Random)
- **Continue Watching** now has play icon + "See All" button
- **Genre chips** upgraded to have colored circle icons inside each chip
- **Coin tip card** upgraded with CTA button (Shop ▸) instead of plain TextButton
- **Pull-to-refresh** now auto-dismisses after 1.5s

### ProfileScreen — Premium Overhaul
- **Online indicator** dot on avatar (green circle with border)
- **Bio text** shown below name (truncated to 120 chars)
- **Stats card** now glassmorphic with colored icon stats (Anime/Watched/Mean/Manga)
- **Coin + Streak card** now has vertical divider between the two columns
- **Quick Actions** now 6 buttons in 2 rows (Shop/Favorites/Reviews + Sync/Share/Settings)
- Each action button has its own color identity (purple/pink/yellow/green/blue/orange)
- **NotLoggedIn** state upgraded with icon circle, subtitle, and rounded buttons

### RandomScreen — Premium Overhaul
- **Roll button** promoted to FilledTonalButton in top bar with dice icon
- **Empty state** now has a large circular icon container, subtitle, and "Discover" CTA button
- **Loading state** has descriptive text "Finding something great..."
- **Card upgrades**: cover image now in Card with elevation, genres use colored chips with borders, added "Details" and "Track" action buttons at bottom

### SeasonalScreen — Premium Overhaul
- **Season tabs** now include Unicode season symbols (❄ ☘ ☀ ♆)
- **Season colors** applied to selected tab text (winter=blue, spring=green, summer=yellow, fall=orange)
- **Subtitle** shows current year
- **Sort chips** use proper FilterChipDefaults colors
- **Loading state** uses PremiumShimmer skeletons (6 rows)
- **Empty state** has icon + two-line message
- **Result count** shown as header ("24 anime this season")
- Items keyed by ID for proper recomposition

### CalendarScreen — Premium Polish
- **Today dot** now green on the day tab
- **Count header** shows "N anime airing on Daynames"
- **Empty state** upgraded with circular icon container
- **Time indicator** now in a Surface with conditional coloring (red if <6h, primary otherwise)
- **Score badge** now has 3-tier coloring (green ≥80, light-green ≥60, yellow otherwise)
- **Cards** keyed by anime ID

### StreamingScreen — Premium Polish
- **Platform quick filter row** (Crunchyroll/Netflix/Hulu/HIDIVE/Disney+/Amazon) with branded colors
- Each platform chip toggles on click with visual selection state
- **Loading state** now has spinner + descriptive text
- **Error state** upgraded with icon + title + retry button
- Items keyed by ID
- Cover images now in Card with proper elevation

### TrackingScreen — Premium Polish
- **Status tabs** now show colored dots (8dp circles) instead of Unicode symbols
- **Search button** added to top bar actions
- **Stats bar** now has 3 colored mini-stats (anime count=primary, eps=green, avg=yellow)
- **Cover images** have status dot overlay (colored circle at bottom-right with border)
- Status text uses color-coded status color directly

## New Features

### JikanApi — News Endpoint
- Added `getRecentNews()` returning `List<AnimeNews>` (top-scored anime as news)
- Added `getAnimeFull()` for full anime detail with all relations
- New `AnimeNews` data class with malId, title, imageUrl, score, synopsis, genres, status, episodes

### MediaListStatus — Icon Property
- Added `.icon` property to `MediaListStatus` enum
- Icons: CURRENT=▶, PLANNING=○, COMPLETED=✓, DROPPED=✕, PAUSED=⏸, REPEATING=↻

## Previous Fixes (from earlier session)
All 14 fixes from the previous session remain intact:
- AniListApi.kt extra brace, EconomyScreen.kt extra paren
- AppModule.kt DI providers (DAOs, DataStore, SyncService, MALAuth, MALListApi, AniListAuth)
- SyncService.kt circular dependency removed
- AnimeRepository.kt proper constructor injection for auth
- SettingsViewModel.kt working update overload
- SharedComponents.kt ContentScale naming conflict resolved
- Mipmap launcher icons created
- Gradle wrapper script created
