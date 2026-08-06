# Rei 零 — Fixes Applied (2026-08-06)

## Critical Compilation Fixes

### 1. AniListApi.kt — Extra closing brace
- **File**: `app/src/main/java/com/rei/app/data/remote/anilist/AniListApi.kt`
- **Issue**: Extra `}` after `getUserAnimeList()` function (129 open / 130 close)
- **Fix**: Removed the extra closing brace

### 2. EconomyScreen.kt — Extra closing paren
- **File**: `app/src/main/java/com/rei/app/ui/screens/economy/EconomyScreen.kt`
- **Issue**: Extra `)` in "Your balance" Row inside purchase dialog (197 open / 198 close)
- **Fix**: Removed the extra `)`

## Dependency Injection Fixes

### 3. AppModule.kt — Missing DAO providers
- **Issue**: `EconomyManager` requires `EconomyDao` via `@Inject constructor`, but no `@Provides` method existed for any Room DAOs
- **Fix**: Added `@Provides` methods for all 4 DAOs: `animeDao`, `trackingDao`, `episodeDao`, `economyDao`

### 4. AppModule.kt — Missing DataStore provider
- **Issue**: `AniListAuth` and `MALAuth` require `DataStore<Preferences>` via `@Inject constructor`, but no `@Provides` existed
- **Fix**: Added `@Provides @Singleton fun dataStore(...)` returning `ctx.dataStore`

### 5. AppModule.kt — AnimeRepository missing dependencies
- **Issue**: `repo()` method didn't pass `SyncService`, `MALAuth`, `MALListApi`, or `AniListAuth`, so sync/auth features were always null
- **Fix**: Added all 4 as parameters and passed them to `AnimeRepository` constructor

### 6. AppModule.kt — Duplicate ReiDataStore binding
- **Issue**: `ReiDataStore` had both `@Inject constructor` AND a `@Provides` method, causing a Hilt duplicate binding conflict
- **Fix**: Removed the `@Provides themeStore()` method; let Hilt use `@Inject constructor` with the new `DataStore<Preferences>` provider

### 7. AnimeRepository.kt — Null anilistAuth field
- **Issue**: `private val anilistAuth: AniListAuth? = null` was always null, making `isAnilistLoggedIn()` always return false
- **Fix**: Added `anilistAuth` as proper constructor parameter with DI injection

### 8. SyncService.kt — Circular dependency
- **Issue**: `SyncService` depended on `AnimeRepository`, which depended on `SyncService` — Hilt can't resolve circular deps
- **Fix**: Removed unused `repo` parameter from `SyncService` constructor

## Type Reference Fixes

### 9. AnimeRepository.kt — Incorrect SyncResult path
- **Issue**: `com.rei.app.data.sync.SyncResult` should be `com.rei.app.data.sync.SyncService.SyncResult` (nested class)
- **Fix**: Corrected to `com.rei.app.data.sync.SyncService.SyncResult`

### 10. SettingsViewModel.kt — Broken update overload
- **Issue**: `fun update(transform: ReiConfig.(Boolean) -> ReiConfig) = tvm.update { c -> c }` ignored the transform parameter entirely
- **Fix**: Changed to `tvm.update { c -> c.transform(true) }`

## Naming Conflict Fixes

### 11. SharedComponents.kt — ContentScale naming conflict
- **Issue**: Both `com.rei.app.ui.theme.ContentScale` (our enum) and `androidx.compose.ui.layout.ContentScale` (Compose) were in scope via wildcard import
- **Fix**: Replaced `import com.rei.app.ui.theme.*` with explicit imports for only the needed types

## Dead Code Fixes

### 12. AnimeRepository.kt — Unused Flow result
- **Issue**: `toggleEpisodeWatched()` called `db.episodeDao().getEpisodes(mediaId)` which returns a `Flow`, but the result was unused
- **Fix**: Removed the dead code line

## Missing Resource Fixes

### 13. Mipmap launcher icons
- **Issue**: Manifest references `@mipmap/ic_launcher` and `@mipmap/ic_launcher_round`, but no mipmap directories existed
- **Fix**: Created `mipmap-anydpi-v26/ic_launcher.xml` and `ic_launcher_round.xml` using adaptive icon format referencing existing foreground/background drawables

### 14. Gradle wrapper script
- **Issue**: `gradlew` script was missing
- **Fix**: Created `gradlew` shell script with proper Gradle 8.5 wrapper configuration

## Validation Results
- 68 Kotlin files, 10,029 total lines
- All braces/parens/brackets balanced across all files
- Zero emojis in any .kt file
- All key files present and accounted for
- No encoding artifacts
