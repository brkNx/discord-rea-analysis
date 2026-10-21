# 04 — Security Architecture, IPC Allowlists & Attack Surface (`Discord app-1.0.9264`)

## 1. Defense-in-Depth Architecture Overview

Discord Desktop runs a remote web application (`https://discord.com/app`) inside an Electron `BrowserWindow` where **`sandbox: false`** is intentionally set so that `mainScreenPreload.js` can load high-performance native C++/Rust `.node` addons (`discord_voice`, `discord_utils`, `discord_zstd`, `discord_erlpack`, `discord_krisp`) directly inside the renderer process.

Because a renderer XSS or compromised third-party activity subframe could otherwise attempt to reach Node.js primitives when `sandbox: false` is set, Discord implements a **6-layer security membrane**:

| Layer | Mechanism | Source Location | Security Enforcement |
|---|---|---|---|
| **1. Subframe Preload Guard** | `process.isMainFrame` assertion | `mainScreenPreload.js`, `frameViewPreload.js` (`8331`) | `if (!process.isMainFrame) throw Error("Preload scripts should not be running in a subframe");` |
| **2. Context Isolation** | Isolated V8 world | `mainScreen.js` (`5203`), `host.js:70-83` | `contextIsolation: true`, `nodeIntegration: false`, `enableRemoteModule: false` |
| **3. Native Module Regex & Blocklist** | `DiscordNative.nativeModules.requireModule` | `mainScreenPreload.js` (`7927`) | Restricts module names to `/^discord_[a-z0-9_-]+$/` or `"erlpack"` and blocks `PRELOAD_ONLY_MODULES = Set(["discord_premap"])` |
| **4. IPC Send/Invoke Whitelists** | `RENDERER_IPC_SEND_WHITELIST` & `INVOKE_WHITELIST` | `mainScreenPreload.js` (`8981`) | Rejects any IPC event not in the 30 `send` or 14 `invoke` allowlisted events (`throw Error("cannot send/invoke this event")`) |
| **5. Settings Write Allowlist** | `RENDERER_SET_ALLOWLIST` | `mainScreenPreload.js` (`6434`) & `core.asar` (`7683`) | Blocks renderer writes to `WEBAPP_ENDPOINT`, `DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING`, and `ENABLE_STAFF_DEV_TOOLS_BUNDLE` |
| **6. External URL & Navigation Guards** | `saferShellOpenExternal` & `will-navigate` | `core.asar` (`1331`, `5203`) & `discord_overlay2\securityUtils.js` | Blocks 14 dangerous URI schemes + single-letter Windows drive schemes (`C:\...`); restricts `will-navigate` to `WEBAPP_ENDPOINT` |

---

## 2. Complete `DiscordNative.ipc` Whitelists (`mainScreenPreload.js` Module `8981`)

### 2.1 `RENDERER_IPC_SEND_WHITELIST` (30 Allowed `ipcRenderer.send` Channels)

| # | Constant Name (`IPCEvents.*`) | Wire Channel Name (`DISCORD_*`) | Subsystem Purpose |
|---:|---|---|---|
| 1 | `APP_ASYNC_INDEX_TSX_LOADED` | `DISCORD_APP_ASYNC_INDEX_TSX_LOADED` | Signals async webapp bundle load (also triggers `staffDevTools` check) |
| 2 | `APP_BADGE_SET` | `DISCORD_APP_BADGE_SET` | Updates taskbar/dock unread badge count |
| 3 | `APP_FIRST_RENDER_AFTER_READY_PAYLOAD` | `DISCORD_APP_FIRST_RENDER_AFTER_READY_PAYLOAD` | TTI telemetry milestone |
| 4 | `APP_GET_ANALYTICS_EVENTS` | `DISCORD_APP_GET_ANALYTICS_EVENTS` | Flushes queued main-process analytics events |
| 5 | `APP_MAIN_BUNDLE_STATS` | `DISCORD_APP_MAIN_BUNDLE_STATS` | Reports bundle load metrics |
| 6 | `APP_PAINT_METRICS` | `DISCORD_APP_PAINT_METRICS` | Reports renderer paint timing |
| 7 | `APP_LOADED` | `DISCORD_APP_LOADED` | Signals initial DOM load |
| 8 | `APP_PUSH_ANALYTICS` | `DISCORD_APP_PUSH_ANALYTICS` | Appends analytics events |
| 9 | `APP_VIEWED` | `DISCORD_APP_VIEWED` | Signals main window viewed |
| 10 | `CHECK_FOR_UPDATES` | `DISCORD_CHECK_FOR_UPDATES` | Triggers manual update check |
| 11 | `NAVIGATION_HISTORY_CLEAR` | `DISCORD_NAVIGATION_HISTORY_CLEAR` | Clears `webContents` navigation history |
| 12 | `NOTIFICATION_CLOSE` | `DISCORD_NOTIFICATION_CLOSE` | Dismisses desktop notification |
| 13 | `NOTIFICATION_SHOW` | `DISCORD_NOTIFICATION_SHOW` | Displays desktop notification |
| 14 | `NOTIFICATIONS_CLEAR` | `DISCORD_NOTIFICATIONS_CLEAR` | Clears all desktop notifications |
| 15 | `OPEN_EXTERNAL_URL` | `DISCORD_OPEN_EXTERNAL_URL` | Passes URL to `saferShellOpenExternal` |
| 16 | `QUIT_AND_INSTALL` | `DISCORD_QUIT_AND_INSTALL` | Restarts app to apply staged update |
| 17 | `SETTINGS_UPDATE_BACKGROUND_COLOR` | `DISCORD_SETTINGS_UPDATE_BACKGROUND_COLOR` | Updates `BrowserWindow` background color |
| 18 | `SYSTEM_TRAY_SET_ICON` | `DISCORD_SYSTEM_TRAY_SET_ICON` | Updates tray icon state (`UNREAD`, `MENTIONED`, `CONNECTED`, etc.) |
| 19 | `SYSTEM_TRAY_SET_APPLICATIONS` | `DISCORD_SYSTEM_TRAY_SET_APPLICATIONS` | Populates tray quick-launch application list |
| 20 | `SYSTEM_TRAY_SET_STATES` | `DISCORD_SYSTEM_TRAY_SET_STATES` | Updates mute/deafen tray menu checkmarks |
| 21 | `SYSTEM_TRAY_SET_STRINGS` | `DISCORD_SYSTEM_TRAY_SET_STRINGS` | Updates localized tray menu labels |
| 22 | `THUMBAR_BUTTONS_UPDATE` | `DISCORD_THUMBAR_BUTTONS_UPDATE` | Updates Windows taskbar thumbnail buttons |
| 23 | `TOGGLE_MINIMIZE_TO_TRAY` | `DISCORD_TOGGLE_MINIMIZE_TO_TRAY` | Toggles close-to-tray behavior |
| 24 | `TOGGLE_OPEN_ON_STARTUP` | `DISCORD_TOGGLE_OPEN_ON_STARTUP` | Toggles OS startup registration |
| 25 | `TOGGLE_START_MINIMIZED` | `DISCORD_TOGGLE_START_MINIMIZED` | Toggles minimized startup flag |
| 26 | `UPDATE_OPEN_ON_STARTUP` | `DISCORD_UPDATE_OPEN_ON_STARTUP` | Refreshes startup registry entry |
| 27 | `UPDATER_HISTORY_QUERY_AND_TRUNCATE` | `DISCORD_UPDATER_HISTORY_QUERY_AND_TRUNCATE` | Reads and truncates updater telemetry history |
| 28 | `UPDATED_QUOTES` | `DISCORD_UPDATED_QUOTES` | Updates cached splash screen quotes |
| 29 | `WINDOW_SET_TRAFFIC_LIGHT_APPEARANCE` | `DISCORD_WINDOW_SET_TRAFFIC_LIGHT_APPEARANCE` | macOS traffic light theme control |
| 30 | `WINDOW_SET_TRAFFIC_LIGHT_POSITION` | `DISCORD_WINDOW_SET_TRAFFIC_LIGHT_POSITION` | macOS traffic light coordinate control |

### 2.2 `RENDERER_IPC_INVOKE_WHITELIST` (14 Allowed `ipcRenderer.invoke` Channels)

| # | Constant Name (`IPCEvents.*`) | Wire Channel Name (`DISCORD_*`) | Subsystem Purpose |
|---:|---|---|---|
| 1 | `APP_GET_ANALYTICS_EVENTS` | `DISCORD_APP_GET_ANALYTICS_EVENTS` | Returns buffered main-process analytics |
| 2 | `GET_MOUSE_COORDINATES` | `DISCORD_GET_MOUSE_COORDINATES` | Queries `screen.getCursorScreenPoint()` |
| 3 | `INTENTS_SET_ACTIVITY` | `DISCORD_INTENTS_SET_ACTIVITY` | Registers macOS/OS user activity intent |
| 4 | `INTENTS_RESIGN_ACTIVITY` | `DISCORD_INTENTS_RESIGN_ACTIVITY` | Clears registered user activity intent |
| 5 | `NOTIFICATIONS_GET_AUTHORIZATION` | `DISCORD_NOTIFICATIONS_GET_AUTHORIZATION` | Queries OS notification permission status |
| 6 | `NOTIFICATIONS_GET_SETTINGS` | `DISCORD_NOTIFICATIONS_GET_SETTINGS` | Queries OS notification settings |
| 7 | `NOTIFICATIONS_GET_MODULE_STATUS` | `DISCORD_NOTIFICATIONS_GET_MODULE_STATUS` | Queries native notifications module state |
| 8 | `NOTIFICATIONS_SEND_NOTIFICATION` | `DISCORD_NOTIFICATIONS_SEND_NOTIFICATION` | Dispatches native/ToastGeneric notification |
| 9 | `NOTIFICATIONS_REMOVE_NOTIFICATIONS` | `DISCORD_NOTIFICATIONS_REMOVE_NOTIFICATIONS` | Removes specific notifications by ID |
| 10 | `NOTIFICATIONS_REMOVE_ALL_NOTIFICATIONS` | `DISCORD_NOTIFICATIONS_REMOVE_ALL_NOTIFICATIONS` | Clears all active notifications |
| 11 | `SEARCH_INDEX_DOMAINS` | `DISCORD_SEARCH_INDEX_DOMAINS` | Indexes domains in OS spotlight/search |
| 12 | `SEARCH_CLEAR_INDEX` | `DISCORD_SEARCH_CLEAR_INDEX` | Clears OS search index |
| 13 | `SEARCH_DELETE_DOMAINS` | `DISCORD_SEARCH_DELETE_DOMAINS` | Deletes domains from OS search index |
| 14 | `SEARCH_DELETE_ITEMS` | `DISCORD_SEARCH_DELETE_ITEMS` | Deletes specific items from OS search index |

---

## 3. `DiscordNative.settings.set` Allowlist vs. Protected Keys (`settings.json`)

| Category | Keys | Security Significance |
|---|---|---|
| **Renderer-Writable (`RENDERER_SET_ALLOWLIST` — 20 Keys)** | `ALWAYS_ALLOW_UPDATES`, `SKIP_HOST_UPDATE`, `SKIP_MODULE_UPDATE`, `OPEN_ON_STARTUP`, `START_MINIMIZED`, `MINIMIZE_TO_TRAY`, `BACKGROUND_COLOR`, `offloadAdmControls`, `enableHardwareAcceleration`, `chromiumSwitches` *(further filtered against `validSwitches` in `app.asar` `63580`)*, `asyncVideoInputDeviceInit`, `enableLibopenhevc`, `enableOpusHardwareRetrying`, `useExperimentalCapturer`, `useGraphicsCapture`, `useQuartzCapturer`, `allowScreenCaptureKitSystemAudio`, `COMPACT_TRAY`, `DESKTOP_PREWARM_DNS_TCP`, `DESKTOP_NATIVE_PREMAP` | Safe UI/media/startup toggles |
| **Protected / Main-Process or Manual Disk Edit Only** | • `WEBAPP_ENDPOINT`<br>• `DANGEROUS_ENABLE_DEVTOOLS_ONLY_ENABLE_IF_YOU_KNOW_WHAT_YOURE_DOING`<br>• `ENABLE_STAFF_DEV_TOOLS_BUNDLE`<br>• `UPDATE_ENDPOINT` / `NEW_UPDATE_ENDPOINT` | Prevents an XSS payload in the renderer from redirecting the startup origin (`WEBAPP_ENDPOINT`) or enabling DevTools persistently |

---

## 4. Custom Crashpad `0xd15c0d01` (`DISCORD_CODE_INTEGRITY`) Minidump Stream

Both `app.asar/bundle.js` (`55264`) and `core.asar/mainScreenPreload.js` (`278` + `5264`) implement a binary parser for a custom Crashpad minidump stream with magic ID **`0xd15c0d01`**:

- **Record Size**: `76` bytes (`19` × `uint32`)
- **Purpose**: Detects in-memory code modifications (hooks, client mods, or memory corruption) in executable pages (`MEM_COMMIT` + `PAGE_EXECUTE_ANY = 0xF0`) at crash time and tags Sentry crash reports:
  - `code_integrity`: `"clean"` | `"modified"` | `"unavailable"`
  - `code_integrity.distance`:
    - `"near"` (modification within `<= 64` bytes of crash instruction pointer / stack)
    - `"within_page"` (modification within `<= 4096` bytes)
    - `"distant"` (modification in module but `> 4096` bytes away)

---

## 5. Ed25519 Signed Staff DevTools Loader (`core.asar/bundle.js` Module `4520`)

When `settings.get("ENABLE_STAFF_DEV_TOOLS_BUNDLE", false)` is enabled in `%APPDATA%\discord\settings.json`:
1. Extracts the encrypted token (`dQw4w9WgXcQ:<base64>`) from `%APPDATA%\discord\Local Storage\leveldb\*.ldb|*.log` and decrypts it via `electron.safeStorage.decryptString` (Windows DPAPI).
2. Queries `${WEBAPP_ENDPOINT}/__development/staff_dev_tools/version` and downloads `/module?hash=${hash}`.
3. Verifies an **Ed25519 signature** over the canonical string `staff_dev_tools:v1:${version}:${hash}` using the hardcoded SPKI public key:
   ```text
   MCowBQYDK2VwAyEAIoaTnSgNjloxqzHKGBXIp0F7JX3AaZKZCocqMBpSi/A=
   ```
4. Only if `crypto.verify(null, Buffer.from("staff_dev_tools:v1:..."), publicKey, signature)` succeeds does it extract the zip to `%APPDATA%\discord\staff_dev_tools\<version>` and execute `require(indexPath).initialize(...)`.

---

## 6. Post-Audit Security Findings & Local Attack Surface Notes

| Finding | Severity (Local Scope) | Evidence ID / Location | Technical Detail |
|---|---|---|---|
| **1. `EnableEmbeddedAsarIntegrityValidation` & `OnlyLoadAppFromAsar` Disabled** | Informational / Local Tampering | `ev_878d604fd2d9aeb4818d6f67c66e02bdf055c42b121c4b785906660d8170c639` (`app-1.0.9264\Discord.exe` Fuses `4` & `5`) | `Discord.exe` does not enforce cryptographic ASAR header integrity validation at the Electron fuse level (`raw_byte: 48`). Processes running as the same Windows user can modify `app.asar` or `core.asar` on disk. Discord relies on `0xd15c0d01` Crashpad telemetry and Sentry `ignoreErrors`/`denyUrls` to filter modified clients rather than blocking startup. |
| **2. `EnableNodeOptionsEnvironmentVariable` Enabled** | Informational / Local Env Injection | `ev_878d604fd2d9aeb4818d6f67c66e02bdf055c42b121c4b785906660d8170c639` (Fuse `2` = `true`) | Unlike `RunAsNode` (`false`) and `EnableNodeCliInspectArguments` (`false`), `EnableNodeOptionsEnvironmentVariable` is enabled (`raw_byte: 49`), allowing local user-level processes to pass `NODE_OPTIONS` when spawning `Discord.exe`. |
| **3. `GRPICONDIRENTRY` 16-Bit Size Truncation in PE Resources** | Cosmetic / Build Tooling Quirk | `ev_96ff7b5f...` (`Discord.exe`) & `ev_271abcca...` (`updater_launcher.exe`) | Icon ID `4` (256×256 32bpp, actual `RT_ICON` payload `270,376` bytes, SHA-256 `8236fa18...`) has `declared_bytes: 8232` (`270376 mod 65536 = 8232`) in `RT_GROUP_ICON`, whereas `DiscordSystemHelper.exe` (`ev_a280ec26...`) records the full `270,376` bytes (`size_matches: true`). |
