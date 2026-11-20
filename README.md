<p align="center">
  <img src="https://i.postimg.cc/qRXyX95T/banner.png" alt="CP2077 RedMod & Log Inspector — RED4ext, CET & redscript Diagnostics for Windows" width="100%">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Platform-Windows%2010%2F11%20x64-0078D6?style=flat-square&logo=windows" alt="Platform: Windows 10/11 x64">
  <img src="https://img.shields.io/badge/Mode-Log%20%26%20Mod%20Audit-2EA44F?style=flat-square" alt="Mode">
  <img src="https://img.shields.io/badge/Release-cp2077--mod--check--26.11a-5391FE?style=flat-square" alt="Release Tag">
  <img src="https://img.shields.io/badge/License-MIT-8B5CF6?style=flat-square" alt="License: MIT">
</p>

# CP2077 RedMod & Log Inspector — RED4ext, CET & redscript Diagnostics for Windows

**Cyberpunk 2077 flatlining immediately after a game update? 'Redscript compilation failed' popup blocking startup, or custom ArchiveXL items failing to appear? Inspect your framework logs before wiping your mod folder.**

CP2077 RedMod & Log Inspector focuses on how core Cyberpunk 2077 modding frameworks report version mismatches: **when `RED4ext`, `Cyber Engine Tweaks`, `ArchiveXL`, `TweakXL`, or `redscript` lag behind a game patch**, their log files record the exact failing DLL or `.reds` script.

This project is aimed at **Cyberpunk 2077 on Windows (Steam)**. It concerns `red4ext\logs`, `r6\logs`, `cyber_engine_tweaks.log`, `archive\pc\mod` layout, and local save backups—not redistributing frameworks or modifying binaries.

## Quick Symptom & Cause Reference

| Symptom / Error | Likely Cause | Recommended Check |
| --- | --- | --- |
| Game flatlines on boot right after patch | Outdated `RED4ext` or `Cyber Engine Tweaks` (`version.dll`) hook | Check `red4ext\logs\red4ext.log` and `cyber_engine_tweaks.log` |
| 'Redscript compilation failed' popup on launch | Incompatible `.reds` script or missing framework dependency | Inspect `r6\logs\redscript_r6.log` for the exact failing file |
| Custom clothing, cars, or Atelier missing | Missing `.xl` manifest in `archive\pc\mod` or outdated `ArchiveXL` | Verify paired `.archive` + `.xl` files and `ArchiveXL` version |
| Need clean checkpoint before overhaul mods | Persistent inventory/cyberware changes written into save files | Back up `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077` |

## Download for Windows

[**Download CP2077ModCheck.zip — Latest GitHub Release (Windows x64)**](../../releases/latest/download/CP2077ModCheck.zip)  
[View All Releases & Changelog](../../releases) · [Report an Issue](../../issues)

### Why run `CP2077ModCheck.exe`? (Automatic checks in ~2 seconds)

- Parses `red4ext.log`, `redscript_r6.log`, and `cyber_engine_tweaks.log` to highlight exact error lines.
- Audits `archive\pc\mod` and counts paired `.archive` and `.xl` asset manifests.
- Detects root DLL loaders in `bin\x64` (`version.dll`, `winmm.dll`) that hook `Cyberpunk2077.exe`.
- Locates `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077` and counts active save slots.

### Sample Diagnostic Output

```text
[CP2077ModCheck] Auditing Cyberpunk 2077 frameworks & logs...
[OK]   Game Install     : D:\SteamLibrary\steamapps\common\Cyberpunk 2077
[OK]   archive\pc\mod   : 38 .archive files, 19 .xl manifests
[ERROR] redscript_r6.log: Line 142 - Failed to resolve dependency 'Codeware' in VirtualAtelier.reds
[OK]   red4ext.log      : Loaded cleanly (0 errors)
[OK]   Saved Games      : C:\Users\Player\Saved Games\CD Projekt Red\Cyberpunk 2077 (14 save slots)
```

The release ZIP is named `CP2077ModCheck.zip`; the Windows executable inside is named `CP2077ModCheck.exe`.

1. Close Cyberpunk 2077 before moving or checking game files.
2. Click **Download CP2077ModCheck.zip** above.
3. Right-click the downloaded archive and choose **Extract All**.
4. Open the extracted folder and run `CP2077ModCheck.exe`.
5. Review the reported locations and status checks before making any changes to your files.
6. Restart Cyberpunk 2077 and verify behavior after applying any recommended fixes.

**Requirements:** Windows 10 or 11 (64-bit), a Windows Steam installation of Cyberpunk 2077, and access to `Saved Games`.

### Verification & Windows SmartScreen Note

- **Read-only & standard user permissions:** `CP2077ModCheck.exe` runs without administrator rights and does not modify game binaries, system registry keys, or Windows Defender settings.
- **Windows SmartScreen:** Because this is an independent open-source utility without a commercial EV code-signing certificate, Windows SmartScreen may show *"Windows protected your PC"* on first launch. Click **More info → Run anyway** after verifying the release checksum on the [Releases](../../releases) page.

## Problems this project focuses on

### Game flatlines on launch immediately after a patch

Outdated core DLL loaders (`RED4ext` or `Cyber Engine Tweaks` `version.dll`) hook into `Cyberpunk2077.exe` at startup. Checking `red4ext\logs\red4ext.log` and `cyber_engine_tweaks.log` reveals version mismatches immediately.

### 'Redscript compilation failed' error dialog

When a `.reds` script mod references a missing dependency or outdated game class, `r6\logs\redscript_r6.log` logs the exact file and line number causing compilation to abort.

### Custom clothing, vehicles, or Atelier stores missing

Asset mods require both `.archive` and `.xl` manifests inside `archive\pc\mod\`, plus up-to-date `ArchiveXL` and `TweakXL` plugins.

### Need safe save backups before testing overhaul mods

Local saves reside in `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077`. Backing up this directory protects clean checkpoints before adding persistent gameplay mods.

## Where Cyberpunk 2077 files normally belong

On Windows Steam setups, mod logs and archives sit inside the game folder while saves live in Saved Games:

```text
%ProgramFiles(x86)%\Steam\steamapps\common\Cyberpunk 2077\
```

Inside that structure, key files and subdirectories typically look like this:

```text
Cyberpunk 2077\
├── archive\
│   └── pc\
│       └── mod\
├── bin\
│   └── x64\
│       └── plugins\
│           └── cyber_engine_tweaks\
│               └── cyber_engine_tweaks.log
├── r6\
│   └── logs\
│       └── redscript_r6.log
└── red4ext\
    └── logs\
        └── red4ext.log

%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077\
```

If you use REDmod (`mods\` folder), remember to enable mods in REDlauncher or pass `-modded` so deployed REDmod archives load alongside `archive\pc\mod`.

## A practical troubleshooting checklist

If your issue still persists, work through these checks in order:

1. **Close Cyberpunk 2077 and REDlauncher.** Verify `Cyberpunk2077.exe` has exited completely in Task Manager.
2. **Back up `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077`.** Copy your save slots to a dated backup folder in Documents before updating frameworks.
3. **Read `red4ext\logs\red4ext.log`.** Check for plugin load failures or game-version mismatch warnings.
4. **Read `r6\logs\redscript_r6.log`.** Identify any `.reds` script throwing compilation errors after a patch.
5. **Verify core frameworks match the current game patch.** Ensure `RED4ext`, `Cyber Engine Tweaks`, `ArchiveXL`, `TweakXL`, and `redscript` are all updated for the same game build.
6. **Check `archive\pc\mod` for orphaned `.xl` or `.archive` files.** Temporarily move recently added archives out of `archive\pc\mod` if crashes persist after framework updates.
7. **Launch via Steam and test a clean save.** Confirm the CET overlay and scripts initialize cleanly before saving over your main playthrough.

### Before moving any files

Keep a dated backup of `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077` before removing overhaul or cyberware mods that modify player inventory state.

## What an effective diagnostic report should tell you

A thorough log inspection scans **`red4ext.log`, `redscript_r6.log`, and `cyber_engine_tweaks.log` for error lines**, counts `.archive` / `.xl` files in `archive\pc\mod`, and verifies your `Saved Games` directory.

## FAQ

**Where are Cyberpunk 2077 mod logs located?**

They are inside the game folder at `red4ext\logs\red4ext.log`, `r6\logs\redscript_r6.log`, and `bin\x64\plugins\cyber_engine_tweaks\cyber_engine_tweaks.log`.

**Why does Cyberpunk 2077 flatline after every update?**

Core frameworks like RED4ext and Cyber Engine Tweaks bind to specific executable addresses and must be updated after each CDPR patch.

**Where do .archive and .xl mods go?**

Place them in `steamapps\common\Cyberpunk 2077\archive\pc\mod\`.

**Where are Cyberpunk 2077 save files stored on Windows?**

Under `%USERPROFILE%\Saved Games\CD Projekt Red\Cyberpunk 2077\`.

**Does this tool install or update RED4ext automatically?**

No. It inspects your local installation and logs; always download frameworks from their official authors.

**Are third-party DLLs or mods bundled here?**

No copyrighted game files or third-party mods are distributed by this repository.

## Useful reading

- [Cyberpunk 2077 on Steam](https://store.steampowered.com/app/1091500/Cyberpunk_2077/)
- [Official Cyberpunk 2077 Website](https://www.cyberpunk.net/)

## More Windows Game Diagnostic Tools

- [PBZ-UE4SSLoaderCheck](../PBZ-UE4SSLoaderCheck) — Phantom Blade Zero `UE4SS.log` AOB & `~mods` loader check
- [BG3-ModSettingsDoctor](../BG3-ModSettingsDoctor) — Baldur's Gate 3 `modsettings.lsx` & Script Extender audit
- [GTAV-ScriptHookModAudit](../GTAV-ScriptHookModAudit) — GTA V `ScriptHookV.dll`, `asiloader.log` & BattlEye check
- [SkyrimSE-SKSEPluginAudit](../SkyrimSE-SKSEPluginAudit) — Skyrim SE `skse64.log` & DLL plugin audit

*Independent community project. Not affiliated with CD PROJEKT RED, Valve, or Steam. All trademarks belong to their respective owners.*
 
 
