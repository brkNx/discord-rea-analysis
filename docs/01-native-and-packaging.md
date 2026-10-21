# 01 — Native Binaries, Packaging, Electron Fuses & Managed Metadata (`Discord app-1.0.9264`)

## 1. Inspected Artifacts & REA Evidence Ledger

| Artifact Path | Format / Arch | Bytes | SHA-256 | REA Operation & `evidence_id` |
|---|---|---:|---|---|
| `C:\Users\brkN\AppData\Local\Discord\Discord.exe` | PE32+ (`x86_64`) | 641,976 | `afd01f3e2bb55540f9f5871fbcc24df22e329fd7101a1798bc1905af490e5b55` | `inspect_pe_resources`: `ev_5bda2e07a993d75ec70b091ce2b5f467374ff3e7f52948bb26647720d56ed316`<br>`inspect_rust_binary`: `ev_bf8ebcb459d16df51e2cbb604836d38b98a0b6a7448d91e5e15faa0d311e94b3` |
| `C:\Users\brkN\AppData\Local\Discord\Update.exe` | PE32 (`x86`) / CLI | 1,596,344 | `0a166a5da46cce77637fdab36b18f09dbc3b24fc839dbc451bb8a862a09fbab8` | `inspect_pe_resources`: `ev_d4c8a6436b8f9c5612ab9700c3cbc7de9282ecb01a7ea436874e921ab08ad9af`<br>`inspect_managed_artifact`: `ev_b941cbdc137bd89c4dd6b89da1282e1da44c8f40fa250e92b02d1f27f49e66c2` (summary: `ev_f49b289bcd30d2f25795e32053dec7e9e0bd4e48824beb369a46f65221e130e1`, refs: `ev_67db544c59f26c00630c8e4382ce136e4d81a648ff239f533d949050108d3cf1`, resources: `ev_5b1cb4f5879961be194fa0bf79b59cc4bdc22f9e4fb56582461f41ee5716d1ec`)<br>`inspect_managed_members`: `ev_296a557bc49ff371acda24705c720a9672c22882c719a9a0007ab903e05275bc` (summary: `ev_32f61fc87445afeaa39eaa87125beb25e005c20b386a589ead45e68795ff71f3`) |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\Discord.exe` | PE32+ (`x86_64`) | 219,700,152 | `a3b0cf011eb75f568963fd0942648455e125b030b4b578fec652dc0b4e747561` | `inspect_electron_fuses`: `ev_878d604fd2d9aeb4818d6f67c66e02bdf055c42b121c4b785906660d8170c639`<br>`inspect_pe_resources`: `ev_96ff7b5fb863762aa1a1f393e5cfed84813af65023b03ad1216bdeb63a1bb754` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\updater_launcher.exe` | PE32+ (`x86_64`) | 641,976 | `afd01f3e2bb55540f9f5871fbcc24df22e329fd7101a1798bc1905af490e5b55` | `inspect_pe_resources`: `ev_271abccad3d015cf9bf593be4a0a2ef25ff1e98912de9a8eb1c8e6ef364122f1`<br>`inspect_rust_binary`: `ev_22dbf508c88c6318a21ccbfda88edb2f98f5d2a32b7bbb5104a7935de92628c7` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\discord_wer.dll` | PE32+ (`x86_64`) | 451,512 | `4ee32819b9a3a76d98de1bbd595d5b9b2795c6b646358cf957261ed41f9d859d` | `inspect_pe_resources`: `ev_22db1e4dabaafe8e136ff9cb126cc38c8e0d4609ce2010cb13b4acb857e2e725` |
| `...\discord_hook-1\discord_hook\169f6c247b6c74\DiscordHook64.dll` | PE32+ (`x86_64`) | 1,294,776 | `873dd1a44e0d1cb68327018508ef5ab1a82e7ab4b4b068a178037360435c3cb5` | `inspect_pe_resources`: `ev_598ecb7fa1b12130b9dc03622042e9a1a27846a703c76704afa60fa1af9595ed` |
| `...\discord_modules-1\discord_modules\169f6c247b6c74\discord_aegis_x64.dll` | PE32+ (`x86_64`) | 492,472 | `92e3f3dc60260f4fa2c5c0f52469d20b0626f50984fa684251b8977fa09630d8` | `inspect_pe_resources`: `ev_211159c581a3380d5a793b422c957bc83c879e5b150ce0c89b1e553a1a2452df`<br>`inspect_rust_binary`: `ev_8f056370b75f8f5d19fe337c80db266477fe8ad1622aced54d8aa29e55ad4046` |
| `...\discord_modules-1\discord_modules\169f6c247b6c74\2\discord_game_sdk_x64.dll` | PE32+ (`x86_64`) | 3,783,512 | `f1296e896c7943ac644610ecbec607ead011ca9193d967a997766741ecd66ceb` | `inspect_pe_resources`: `ev_0e9c56c396a06333a1159992f793ca1dd4431fe3d51be8110c71dffcf32a1a29` |
| `...\discord_utils-1\discord_utils\DiscordSystemHelper.exe` | PE32+ (`x86_64`) | 2,247,552 | `e8192a5370052db40f545f83fb87088c662cbd816812231df525a6cea4551322` | `inspect_pe_resources`: `ev_a280ec26b3a99d1a7041cd9522c4902f4893bdbd690786ca9a91665ae1356232` |
| `...\discord_voice-1\discord_voice\gpu_encoder_helper.exe` | PE32+ (`x86_64`) | 926,136 | `6fd02608cd208ad1750e991b76bd9b331e287969f07f64c981594d40c68630e7` | `inspect_pe_resources`: `ev_e9a0f1f20f6309eb87934e55380c939ef9ffa20f2792ca6cbcb61c729a0ebc93` |
| `...\discord_voice-1\discord_voice\audio_effects_helper.exe` | PE32+ (`x86_64`) | 292,280 | `6db3203a3d5385b348fd5102688a72032d2a168f69bcf371d54074426775b4e7` | `inspect_pe_resources`: `ev_5eff519caa33202f86dcc3d4ea7bcc3d0b87973a5ebd726cfdedc481d07d481d` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\Microsoft.Gaming.XboxApp.XboxNetwork.winmd` | PE32 (`x86`) / WinMD | 2,048 | `dd76201ed745fb7231dc48eb644c2b9c0dc424cdff97b0ed001777ba8a2707fb` | `inspect_managed_artifact`: `ev_23ab07860b9e0d2b23ecb9e1907229556259c21b84c4402cbfa0abc2045ee008`<br>`inspect_managed_members`: `ev_c070a4db183edb207b98fb133e34d9e48ccef73f9d9c5fb0859316caeb8e2336` |
| `C:\Users\brkN\AppData\Local\Discord\app-1.0.9264\updater.node` | PE32+ (`x86_64`) | 4,550,072 | `c25eca24e0b53f591608c12f7bcb04bc14c5f6099547fb88316567d01527a196` | `inspect_rust_binary`: `ev_051ca978d1d97c0d125d3d358790d8efc19987a07c641d4b550141c22380e130` |
| `...\discord_dispatch-1\discord_dispatch\discord_dispatch.node` | PE32+ (`x86_64`) | 10,493,880 | `24eb096355fb6e68ede690918685da96d725077471bb4e837e7769a4c8e3e0f6` | `inspect_rust_binary`: `ev_9b868fa2c3c9a4a41e2a2da7d124f6eef0c79f9fdb61d7e355cc4c0fbdacf989` |
| `...\discord_cloudsync-1\discord_cloudsync\discord_cloudsync.node` | PE32+ (`x86_64`) | 4,432,824 | `e7337463cffd621b23ca14d19b9075d5c3599ceb66f3a8a508b0967019c285d4` | `inspect_rust_binary`: `ev_11579ad2fb67d585a619722145c0e788efd32146a2c61d4baad4c4c90b3daa4c` |
| `...\discord_voice-1\discord_voice\discord_voice.node` | PE32+ (`x86_64`) | 22,325,176 | `efb7feb15401c4301cf80d9275bf23b9b4845dc9e7b22e92991de066da88654f` | `inspect_rust_binary`: `ev_f7e86db7171003202ebf907977ce27d174b92d4df1ac3f6717ea3a012cc8d203` |

---

## 2. Electron Fuses (`app-1.0.9264\Discord.exe`)

- **Evidence ID**: `ev_878d604fd2d9aeb4818d6f67c66e02bdf055c42b121c4b785906660d8170c639`
- **SHA-256**: `a3b0cf011eb75f568963fd0942648455e125b030b4b578fec652dc0b4e747561`
- **Sentinel Offset**: `182144240` (`0x0ADB4AF0`), `sentinel_count: 1`, `complete: true`
- **Wire Version**: `1` | **Declared Length**: `9`

| Fuse Index | Fuse Name | State | Boolean | Raw Byte | Security / Runtime Implication |
|---:|---|---|---|---|---|
| 0 | `RunAsNode` | `disabled` | `false` | `48` (`'0'`) | `ELECTRON_RUN_AS_NODE` ignored (`not_honored`); blocks trivial living-off-the-land Node execution via env var |
| 1 | `EnableCookieEncryption` | `disabled` | `false` | `48` (`'0'`) | OS-level Electron cookie encryption fuse disabled |
| 2 | `EnableNodeOptionsEnvironmentVariable` | `enabled` | `true` | `49` (`'1'`) | `NODE_OPTIONS` environment variable is honored by the binary |
| 3 | `EnableNodeCliInspectArguments` | `disabled` | `false` | `48` (`'0'`) | `--inspect` / `--inspect-brk` CLI flags ignored (`not_honored`) |
| 4 | `EnableEmbeddedAsarIntegrityValidation` | `disabled` | `false` | `48` (`'0'`) | Embedded ASAR header integrity validation not enforced (`not_enforced`) |
| 5 | `OnlyLoadAppFromAsar` | `disabled` | `false` | `48` (`'0'`) | App loading not restricted exclusively to `app.asar` (`not_restricted_to_app_asar`) |
| 6 | `LoadBrowserProcessSpecificV8Snapshot` | `disabled` | `false` | `48` (`'0'`) | Standard `v8_context_snapshot.bin` used |
| 7 | `GrantFileProtocolExtraPrivileges` | `enabled` | `true` | `49` (`'1'`) | Grants `file://` extra privileges |
| 8 | `WasmTrapHandlers` | `enabled` | `true` | `49` (`'1'`) | Hardware out-of-bounds WebAssembly trap handlers enabled |

---

## 3. PE Resources, Version, Manifest & Icon Findings (`inspect_pe_resources`)

| Binary | PE Format (`machine`) | Resource Dir RVA / Size | Examined Entries / Resources | Resource Types Present (`type.id` : count, lang, size, SHA-256) | `RT_GROUP_ICON` (`type.id=14`) Details |
|---|---|---|---|---|---|
| `Discord.exe` (root stub) & `app-1.0.9264\updater_launcher.exe` *(byte-identical)* | `pe32-plus` (`0x8664`) | RVA `360448`, offset `342016`, `286,604` B | 15 entries / 6 resources | • `3` (`RT_ICON`): 4 icons (IDs 1–4, lang `1033`)<br>• `14` (`RT_GROUP_ICON`): ID `69`, lang `1033`, `62` B (`d49ae996...`)<br>• `16` (`RT_VERSION`): ID `1`, lang `1033`, `756` B (`8d8a2db4...`)<br>• No `RT_MANIFEST` (`24`) | 1 group (`resource_index: 4`, name ID `69`):<br>• 16×16 32bpp (`1,128` B, `5477aa40...`, `size_matches: true`)<br>• 32×32 32bpp (`4,264` B, `58eb4cc5...`, `size_matches: true`)<br>• 48×48 32bpp (`9,640` B, `628802d8...`, `size_matches: true`)<br>• 256×256 (`width: 0, height: 0`) 32bpp (`declared_bytes: 8232` vs actual payload `270,376` B, `8236fa18...`, **`size_matches: false`**) |
| `Update.exe` (root) | `pe32` (`0x014c`) | RVA `1597440`, offset `1583104`, `1,552` B | 6 entries / 2 resources | • `16` (`RT_VERSION`): ID `1`, lang `0`, `900` B (`d8e27921...`)<br>• `24` (`RT_MANIFEST`): ID `1`, lang `0`, `490` B (`539dc26a...`) | None (`icon_groups: []`) |
| `app-1.0.9264\Discord.exe` | `pe32-plus` (`0x8664`) | RVA `219734016`, offset `218221568`, `347,284` B | 108 entries / 51 resources | • `1` (`RT_CURSOR`): 23 cursors (IDs 1–23, lang `0`, `308`–`4,268` B)<br>• `3` (`RT_ICON`): 4 icons (IDs 1–4, lang `1033` — identical SHA-256s to `Discord.exe` stub & `DiscordSystemHelper.exe`)<br>• `12` (`RT_GROUP_CURSOR`): 21 cursor groups (IDs `50250`–`50270`, lang `0`, `20`–`34` B)<br>• `14` (`RT_GROUP_ICON`): ID `1`, lang `1033`, `62` B (`d49ae996...`)<br>• `16` (`RT_VERSION`): ID `1`, lang `1033`, `808` B (`a06f432d...`)<br>• `24` (`RT_MANIFEST`): ID `1`, lang `1033`, `1,425` B (`2545f65a...`) | 1 group (`resource_index: 48`, name ID `1`):<br>Identical 4 icon payloads to `Discord.exe` stub; 4th entry (`256×256`) also has `declared_bytes: 8232` vs actual `270,376` B (**`size_matches: false`**; `270376 mod 65536 = 8232` 16-bit truncation) |
| `DiscordSystemHelper.exe` | `pe32-plus` (`0x8664`) | RVA `1949696`, offset `1932800`, `286,704` B | 15 entries / 6 resources | • `3` (`RT_ICON`): 4 icons (IDs 1–4, lang `1033`, identical SHA-256s to `Discord.exe`)<br>• `14` (`RT_GROUP_ICON`): ID `1`, lang `1033`, `62` B (`c2f352fe...`)<br>• `16` (`RT_VERSION`): ID `1`, lang `1033`, `844` B (`67c9813a...`) | 1 group (`resource_index: 4`, name ID `1`):<br>Same 4 icon payloads, except 4th entry (`256×256`) declares full `270,376` bytes (**`size_matches: true`**) |
| `discord_wer.dll`, `DiscordHook64.dll`, `discord_aegis_x64.dll`, `discord_game_sdk_x64.dll`, `gpu_encoder_helper.exe`, `audio_effects_helper.exe` | `pe32-plus` (`0x8664`) | `null` | 0 / 0 | No `.rsrc` directory | None |

---

## 4. .NET / CLI Managed Metadata (`Update.exe` & `XboxNetwork.winmd`)

### 4.1 `Update.exe` (Squirrel.Windows Single-Binary Bundle via ILRepack)
- **Evidence IDs**: `ev_b941cbdc137bd89c4dd6b89da1282e1da44c8f40fa250e92b02d1f27f49e66c2`, `ev_296a557bc49ff371acda24705c720a9672c22882c719a9a0007ab903e05275bc`
- **Identity & CLI Header**:
  - Module Name: `Update.exe`, MVID: `9fab9409-1eb1-4b86-9cb9-74f40bae8a64`
  - Assembly Name: `Update.exe`, Version: `1.1.1.0`, Target Framework: `.NETFramework,Version=v4.5`, Metadata Version: `v4.0.30319`
  - Managed Architecture: `anycpu-prefer-32` (`il-only`, `32-bit-required`, `32-bit-preferred`), CLI Entry Point Token: `0x060000d5` (`Squirrel.Update.Program`)
- **Metadata Table Counts**:
  - `TypeDef`: `1,457` | `TypeRef`: `688` | `Field`: `5,866` | `MethodDef`: `10,658` | `Param`: `9,668` | `MemberRef`: `4,597` | `CustomAttribute`: `3,061` | `Property`: `2,335` | `NestedClass`: `552` | `ImplMap` (P/Invoke): `23` | `ModuleRef`: `10` | `AssemblyRef`: `25` | `ManifestResource`: `6`
  - Member Graph Counts: `call_edges`: `36,197`, `field_accesses`: `20,276`, `user_strings`: `2,080`
- **Embedded Manifest Resources (`ev_5b1cb4f5879961be194fa0bf79b59cc4bdc22f9e4fb56582461f41ee5716d1ec`)**:
  - `ILRepack.List` (`848` B, SHA-256 `6399c56b599d76b3117d115c81eb77f3cb103d26ec29323688468e28e026e56e`) — confirms single-binary merging via **ILRepack**
  - `Microsoft.Web.XmlTransform.SR.resources` (`5,929` B), `NuGet.CommonResources.resources` (`1,403` B), `NuGet.Resources.AnalysisResources.resources` (`3,803` B), `NuGet.Resources.NuGetResources.resources` (`15,310` B), `NuGet.Authoring.nuspec.xsd` (`6,470` B)
- **Merged Namespaces**:
  - `Squirrel.Update` (`Program`, `AnimatedGifWindow`, `CopStache`, `NativeMethods`, `UpdateAction`, `ProgressSource`, `SetupLogLogger`), `Squirrel`, `Squirrel.Shell`, `Squirrel.Bsdiff`, `Squirrel.Json`, `Mono.Options`, `WpfAnimatedGif`, `ICSharpCode.SharpZipLib.*`, `Microsoft.Web.XmlTransform`, `Splat`, `DeltaCompressionDotNet.MsDelta`, `MarkdownSharp`, `NuGet`, `Mono.Cecil.*`.

### 4.2 `Microsoft.Gaming.XboxApp.XboxNetwork.winmd`
- **Evidence IDs**: `ev_23ab07860b9e0d2b23ecb9e1907229556259c21b84c4402cbfa0abc2045ee008`, `ev_c070a4db183edb207b98fb133e34d9e48ccef73f9d9c5fb0859316caeb8e2336`
- **Identity**: WindowsRuntime 1.4 (`Flags: 512`), MVID `7e67e45f-4dab-415e-b6f6-311d1cc12f14`
- **Types (`0x02000002` `GameInterop`, `0x02000003` `IGameInterop`) & 4 Async WinRT Methods**:
  1. `IsGameInstalledAsync(u4, bool) -> IAsyncOperation<bool>`
  2. `LaunchGameAsync(u4) -> IAsyncOperation<bool>`
  3. `IsInviteTokenValidAsync(string) -> IAsyncOperation<bool>`
  4. `AcceptGameInviteAsync(string) -> IAsyncOperation<bool>`

---

## 5. Rust Binary Dependency & Compiler Metadata (`inspect_rust_binary`)

| Artifact | Evidence ID | SHA-256 | `.dep-v0` (`auditable_metadata`) | `rustc_commit` (Inferred from `/rustc/<40-hex>/` in rodata) | File Offset |
|---|---|---|---|---|---:|
| `app-1.0.9264\updater.node` | `ev_051ca978d1d97c0d125d3d358790d8efc19987a07c641d4b550141c22380e130` | `c25eca24e0b53f591608c12f7bcb04bc14c5f6099547fb88316567d01527a196` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `3,081,056` |
| `app-1.0.9264\updater_launcher.exe` | `ev_22dbf508c88c6318a21ccbfda88edb2f98f5d2a32b7bbb5104a7935de92628c7` | `afd01f3e2bb55540f9f5871fbcc24df22e329fd7101a1798bc1905af490e5b55` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `242,791` |
| `Discord.exe` (root stub launcher) | `ev_bf8ebcb459d16df51e2cbb604836d38b98a0b6a7448d91e5e15faa0d311e94b3` | `afd01f3e2bb55540f9f5871fbcc24df22e329fd7101a1798bc1905af490e5b55` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `242,791` |
| `...\discord_dispatch\discord_dispatch.node` | `ev_9b868fa2c3c9a4a41e2a2da7d124f6eef0c79f9fdb61d7e355cc4c0fbdacf989` | `24eb096355fb6e68ede690918685da96d725077471bb4e837e7769a4c8e3e0f6` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `7,694,429` |
| `...\discord_cloudsync\discord_cloudsync.node` | `ev_11579ad2fb67d585a619722145c0e788efd32146a2c61d4baad4c4c90b3daa4c` | `e7337463cffd621b23ca14d19b9075d5c3599ceb66f3a8a508b0967019c285d4` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `3,019,440` |
| `...\discord_voice\discord_voice.node` | `ev_f7e86db7171003202ebf907977ce27d174b92d4df1ac3f6717ea3a012cc8d203` | `efb7feb15401c4301cf80d9275bf23b9b4845dc9e7b22e92991de066da88654f` | `null` | `73dc9167f1cd099e525c9ade2e068d1907b78564` | `14,499,616` |
| `...\169f6c247b6c74\discord_aegis_x64.dll` | `ev_8f056370b75f8f5d19fe337c80db266477fe8ad1622aced54d8aa29e55ad4046` | `92e3f3dc60260f4fa2c5c0f52469d20b0626f50984fa684251b8977fa09630d8` | `null` | `null` | — |
