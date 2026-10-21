# 02 — Bootstrapper (`app.asar`) & Core Engine (`core.asar`) Analysis (`Discord app-1.0.9264`)

## 1. Evidence & Artifact Ledger (Observations)

| Artifact / File Path | Size (Bytes) | SHA-256 Digest | REA Evidence IDs / Bundle Metadata |
|---|---:|---|---|
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\resources\build_info.json` | 159 | *(plaintext JSON)* | `releaseChannel: "stable"`, `version: "1.0.9264"`, `sentryDist: "stable-win-x64"`, `sentryRelease: "discord-desktop-169f6c247b6c747b6c557188146e687c2c5ba014"` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\resources\bootstrap\manifest.json` | 190 | *(plaintext JSON)* | Bundled module bootstrap versions (all `0`): `discord_desktop_core`, `discord_erlpack`, `discord_notifications`, `discord_premap`, `discord_spellcheck`, `discord_utils`, `discord_voice`, `discord_zstd` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\modules\discord_desktop_core-1\discord_desktop_core\package.json` | 91 | *(plaintext JSON)* | `{"name": "discord_desktop_core", "version": "0.0.0", "private": "true", "main": "index.js"}` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\modules\discord_desktop_core-1\discord_desktop_core\index.js` | 40 | *(plaintext JS)* | `module.exports = require('./core.asar');` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\resources\app.asar` | 3,635,916 | `0025c02c7241045a12464309604ec4d5b8ec7ed2ce43cce05d5dacb8f253f672` | Summary: `ev_baf8e88e459a4b6244dad5238633b4103de88751d776272c97724b3de8f12a57` \| Parent: `ev_406bd84efa4a93bc450cc6929a464e4a919e19cd9280740923522633c33d565d` \| View Digest: `6131ca8a19072dc421eb397e451a8ff6bb8f5135cbd335dca7af5a930ef0dbcc` |
| `app.asar -> package.json` | 883 | `30dfc02065c7e0a135e16351c129a118f1b1f40f764b85ea4d038e2c86117039` | `"name": "discord"`, `"main": "bundle.js"`, `devDependencies.electron: "42.7.1"` |
| `app.asar -> bundle.js` | 3,076,973 | `9ed39b34d873eaf237c19c8f81528bdb739a4250bb6cfc8a0e328688e1c99b7e` | Rspack/Webpack bundle (`977` modules, entrypoint `66321`, `debugId: a548be72-1823-5aa1-9328-b9104d2f45c7`) |
| `app.asar -> splashScreenPreload.js` | 12,516 | `428aa40cfb523f128d34d945631bdf3d254bd07c5530d1608b746ff5b0c4414a` | Rspack/Webpack bundle (`6` modules, entrypoint `85425`, `debugId: ba239403-b846-5b2f-9c3c-896517b6f98a`) |
| `app.asar -> splash/index.html` | 198 | `4bfe611cf9dc0882c51364b1d102bee34a151e9d1d881a250c8deaaab6cbd738` | Splash DOM shell (`#splash-mount`), loads `index.css`; also packages `splash/videos/connecting.webm` & `connecting.webp` and `data/quotes_copy.json` (`f0971e5e329cd5cc89f631c630d0c083e71e97f884fd50b1439c1d695c8b6ae6`) |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\modules\discord_desktop_core-1\discord_desktop_core\core.asar` | 1,832,667 | `922c22e53eceb3a62fdfa51240ebbbb55e8e75cbc03caa6b711f6193448fa48e` | Summary: `ev_014e81847fb0f93d9ff3ef45c21fd2bb11f71f94234ba59dcfaadfc10ee1ffb2` \| Parent: `ev_a1cebcf9a45b93e46a93385597d537ff5a8a952df9fa9ce69069fb22bd4f5211` \| View Digest: `3d29df5d2663a1533a5bfc6477385bdcec672df3adc3b042211e282c351cf366` \| Feature Trace (`DiscordNative`): `ev_6114a862c57c0317b7f43758925a8d631451ccba43de4788ac05f2fe3fca89c6` |
| `core.asar -> package.json` | 1,536 | `608d5064d66f20e1fd733a3ec5cd13d0934d182649d7680b1a9809115c6f8eeb` | `"name": "discord_desktop_core"`, `"main": "bundle.js"` |
| `core.asar -> bundle.js` | 439,665 | `af460603a22f6d1b6cf766150fa0a11f9120da167ef334fc57c03b0c14acfcbd` | Rspack/Webpack bundle (`114` modules, entrypoint `376`) |
| `core.asar -> mainScreenPreload.js` | 1,116,629 | `64bc74f002caa53777aa79b158de9083f99d37117f39da388b5d225f336797db` | Rspack/Webpack bundle (`387` modules, `debugId: 8fe54547-8366-5904-9790-bb08ab68e805`) |
| `core.asar -> frameViewPreload.js` | 21,149 | `e18f4e1e054253fe1a46d13f7c5832c3d652a94a66374f950d258eb3bb294b13` | Rspack/Webpack bundle (`5` modules, entrypoint `8331`, `debugId: 403ff30e-9c9a-5910-bd2c-d96f0bcf084c`) |
| `core.asar -> data/*` & `app/images/*` | *(assets)* | `cacert.pem`: `f290e6acaf904a4121424ca3ebdd7065278070feb628e8f8abbe01f74f9cd0f4`<br>`riotgames.pem`: `68b7a246187b9ff55a3bb8ac9bf0cde5ff3ead13ecab16df11ef3cecf9200b7e` | Root CA bundle (`cacert.pem`), Riot Games LCU CA (`riotgames.pem`), tray/badge/thumbar icons (`app/images/{badges,systemtray,thumbar}/*`) |

---

## 2. Bootstrapper Architecture & Startup Lifecycle (`resources\app.asar`)

### 2.1 Entrypoint & Mode Dispatch (`app.asar/bundle.js` module `66321`)
- Reads `../build_info.json` (`buildInfo`) and initializes path resolution (`paths.init(buildInfo)`, module `37948`), computing:
  - `userData`: `%APPDATA%\discord`
  - `modulePath`: `<exeDir>\modules` (on Win32 when `buildInfo.newUpdater` is unset) or `%APPDATA%\discord\<version>\modules`
  - `moduleDataPath`: `%APPDATA%\discord\module_data`
- Branches on `process.argv.includes("--overlay-host")`:
  1. **`"overlay-host"` mode**: Initializes `appSettings.init()` (`43731`), calls `updater.tryInitUpdater(buildInfo, "https://updates.discord.com/")` (`47421`) and `getUpdater()?.startCurrentVersionSync({allowObsoleteHost: true})`, sets `paths.setModulePath(...)`, and loads `requireNative("discord_overlay2/standalone_host.js")` (`30885`).
  2. **`"app"` mode (default)**: Requires main bootstrapper module `23151` (`bootstrap.js`).

### 2.2 Main Bootstrapper Sequence (`app.asar/bundle.js` module `23151`)
1. **TTI & Error/Crash Infrastructure**:
   - Records process start timestamp (`DesktopTTIAnalytics.trackMainAppTimeToInit()`, `45152`).
   - Initializes `errorHandler.init()` (`37079`), `appSettings.init()` (`43731`), and `blackbox.initialize(paths.getModuleDataPath(), buildInfo)` (`81598`), logging lifecycle events to `%APPDATA%\discord\module_data\crashlogs\*-events.log`.
   - Initializes Sentry (`crashReporterSetup.init(buildInfo, sentry)`, `62457` + `38748`):
     - Custom Minidump Stream Parser (`55264`): inspects minidump attachments (`0x504d444d`) for custom stream type `0xd15c0d01` (`DISCORD_CODE_INTEGRITY`, 76-byte record) and tags Sentry reports with `code_integrity: "clean" | "modified" | "unavailable"` and `code_integrity.distance` (`near` <=64B, `within_page` <=4096B, `distant`).
     - Filters client mod errors in `ignoreErrors` (`"BetterDiscord"`, `"VencordPatcher"`, `"mwittrien.github.io"`) and `denyUrls: [/betterdiscord:\/\//]`.
2. **GPU, Hardware Acceleration & Chromium Switches (`23151` & `63580`)**:
   - Appends Chromium switches (`autoplay-policy=no-user-gesture-required`, `disable-background-timer-throttling`, `disable-renderer-backgrounding`, `enable-h264-mf`, `enable-h264-mf-zero-copy`, `disable-features=WinRetrieveSuggestionsOnlyOnDemand,HardwareMediaKeyHandling,MediaSessionService,UseEcoQoSForBackgroundProcess,IntensiveWakeUpThrottling,AllowAggressiveThrottlingWithWebSocket`).
   - Checks `app.getGPUInfo("basic")` against a 67-entry NVIDIA device ID blocklist (`NV_HEVC_HARDCODED_DEVICE_LIST`) to disable accelerated HEVC decode when needed.
3. **Squirrel / Windows Installer Lifecycle & Single-Instance Lock (`27251`, `19192`)**:
   - Sets `app.setAppUserModelId("com.squirrel.Discord.Discord")`, handles `--squirrel-*` flags, and acquires `app.requestSingleInstanceLock()`.
4. **Privileged Custom Scheme (`46312` & `9157`)**:
   - Registers `disclip://` (`standard: true, secure: true, corsEnabled: true, supportFetchAPI: true, stream: true`).
5. **Ready Phase, Out-of-Band DNS Telemetry & Updater Handoff (`33745`, `47421`, `47924`, `53006`)**:
   - Encodes connection failure telemetry into base32 DNS labels (`<payload>.<uuid>.v1.dc-telemetry.net`) via `dns.lookup`.
   - Loads `updater.node` (`new Updater({ release_channel, platform: "win", repository_url: "https://updates.discord.com/", ... })`), ensures root `Discord.exe` matches `updater_launcher.exe`, initializes `splashScreen.initSplash`, and hands off to `discord_desktop_core` on `APP_SHOULD_LAUNCH`.

---

## 3. Core ASAR Architecture (`core.asar`)

### 3.1 Core Main-Process Initialization (`core.asar/bundle.js` module `376` `app/index.js`)
- Declares 38 supported desktop features (`appFeatures.init()`, `5976`), loads root CAs (`data/cacert.pem` and `data/riotgames.pem`), registers all `discord_native/browser/*` `ipcMain` handlers, binds the `disclip://` protocol handler (`7635`), and invokes `mainScreen.init()` (`5203`).

### 3.2 Main Window Security & Permission Policies (`core.asar/bundle.js` module `5203` `app/mainScreen.js`)
- **`BrowserWindow` `webPreferences`**:
  - `nodeIntegration: false`, `sandbox: false`, `contextIsolation: true`, `spellcheck: true`, `preload: path.join(__dirname, "mainScreenPreload.js")`.
  - `additionalArguments`: `["--enable-node-leakage-in-renderers"]` (plus `"--enable-native-premap"` when enabled).
  - `devTools`: disabled on `stable` unless `DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING` is set in `settings.json`.
- **Session Permission Handlers**:
  - `notifications`: Allowed iff origin matches `WEBAPP_ENDPOINT`.
  - `pointerLock`, `fullscreen`, `clipboard-sanitized-write`: Allowed for subframes (Activity iframes) or `WEBAPP_ENDPOINT`.
  - `media`: Allowed iff origin matches `WEBAPP_ENDPOINT` or `AllowedMediaOrigins` (`https://d3ogqhtsivkon3.cloudfront.net`, `https://family.k-id.com`, `https://age-verification-k-id.privately.swiss`).
- **Signed Staff DevTools Bundle Loader (`staffDevTools.js` module `4520`)**:
  - Gated behind `ENABLE_STAFF_DEV_TOOLS_BUNDLE` in `settings.json`; verifies an Ed25519 signature (`staff_dev_tools:v1:${version}:${hash}`) against hardcoded public key `MCowBQYDK2VwAyEAIoaTnSgNjloxqzHKGBXIp0F7JX3AaZKZCocqMBpSi/A=`.

---

## 4. Preload & ContextBridge Surface (`mainScreenPreload.js` & `frameViewPreload.js`)

- **Subframe Protection**: Both `mainScreenPreload.js` and `frameViewPreload.js` abort immediately if `!process.isMainFrame`.
- **Custom Electron Binding & Native Premap**:
  - Calls `electron.discord?.install("createDiscordStream")` (`6415`).
  - Loads `discord_premap.node` (`7328`) to pre-map `discord_voice`, `discord_zstd`, `discord_cloudsync`, and `discord_media`.
- **`DiscordNative` ContextBridge API (38 Top-Level Keys)**:
  - `isRenderer`, `setUncaughtExceptionHandler`, `nativeModules` (restricted to `/^discord_[a-z0-9_-]+$/` | `"erlpack"`, blocking `discord_premap`), `process` (restricted env vars), `os`, `app` (9 allowed `getPath` keys), `clipboard`, `ipc` (`RENDERER_IPC_SEND_WHITELIST` [30 channels], `RENDERER_IPC_INVOKE_WHITELIST` [14 channels]), `gpuSettings` (`RENDERER_SET_CHROMIUM_SWITCHES_ALLOWLIST = []`), `window`, `powerMonitor`, `ntpClock` (`time.cloudflare.com:123`), `spellCheck`, `crashReporter`, `desktopCapture`, `activityCapture`, `fileManager`, `clips`, `processUtils`, `powerSaveBlocker`, `frameDebugger`, `frameView`, `http` (pinned to `cacert.pem`), `accessibility`, `features`, `settings` (`RENDERER_SET_ALLOWLIST` [20 keys], excluding `WEBAPP_ENDPOINT`, `DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING`, and `ENABLE_STAFF_DEV_TOOLS_BUNDLE`), `userDataCache`, `thumbar`, `safeStorage`, `hardware`, `tracing`, `riotGames` (pinned to `riotgames.pem`), `cs2Gsi`, `dotaGsi`, `rocketLeagueStatsApi`, `webAuthn`, `gcEvents`, `sysimg`.
