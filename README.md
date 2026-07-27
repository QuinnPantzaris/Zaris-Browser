<img width="1000" height="1000" alt="image" src="https://github.com/user-attachments/assets/4a175a3b-8deb-48f0-9f29-da6571147394" />

<div align="center">

# Zaris

**A browser built to get out of your way.**

No sponsored results. No Copilot, no Gemini, no bolted-on AI assistant nobody asked for. No bloat.
Just a fast, dark, keyboard-friendly browser with ad/tracker blocking baked in from the start.

[Download](#download) · [Features](#features) · [Build from source](#build-from-source) · [Known limitations](#known-limitations)

</div>

---

## Why

Chrome used to be the fast, clean option. It isn't anymore. Zaris is a small desktop browser built from
scratch on top of Chromium (via Electron) — designed once, kept simple, and free of anything that wasn't
put there on purpose.

## Features

- **Clean, dark, custom UI** — no native title bar, no visual seams, no vendor branding anywhere in the chrome
- **Tabs that behave** — drag to reorder, pin your regulars, shrink to fit as you open more instead of running
  off-screen, right-click for duplicate / close-others / close-to-the-right
- **Ad & tracker blocking on by default** — EasyList, EasyPrivacy, and uBlock Origin's own filter lists
  (including the ones that specifically target YouTube's ad delivery, not just generic network requests)
- **Bookmarks, history, and downloads** in one Library panel — all of it persists across restarts
- **Session restore** — close Zaris, reopen it, your tabs are exactly where you left them
- **Crash recovery** — a tab whose page process dies gets a real recovery screen, not a frozen blank page
- **Password autofill** — detects login forms, offers to save, encrypts what it saves via your OS's own
  credential store (Windows DPAPI / macOS Keychain / libsecret), autofills on return visits
- **Search prediction** as you type in the address bar, weighted toward your own history and bookmarks first
- **Spell check, PDF viewing inline, download notifications** — the unglamorous stuff a browser just needs
- **Brave Search by default** — real dark theme, real image results, no API key required, and its own header
  branding is stripped out since you're using it inside your own browser, not theirs

## Download

Prebuilt Windows binaries are attached to the [latest release](../../releases/latest):

| File | What it is |
|---|---|
| `Zaris-Setup.exe` | Installer — Start Menu entry, uninstaller, pick your install folder |
| `Zaris-portable.exe` | No install — just run it |

**Windows SmartScreen will likely warn you on first launch** ("Windows protected your PC"). That's because
this isn't signed with a paid Authenticode certificate, not because anything's wrong — click **More info →
Run anyway**. If you'd rather not take that on faith, the full source is right here: read it, or build it
yourself with the instructions below.

Checksums for the current release are in the [release notes](../../releases/latest).

## Build from source

Requires [Node.js](https://nodejs.org).

```bash
git clone https://github.com/QuinnPantzaris/zaris.git
cd zaris
npm install
npm start          # run it live
npm run dist:win   # -> dist/Zaris-Setup-*.exe and dist/Zaris-*-portable.exe
```

Building the Windows target from Linux/macOS additionally needs Wine (Electron-builder uses it to stamp the
`.exe`'s icon and metadata):

```bash
sudo dpkg --add-architecture i386 && sudo apt update
sudo apt install wine32:i386 wine64
```

Or skip all of that: this repo includes a GitHub Actions workflow
(`.github/workflows/build.yml`) that builds a fresh `.exe` on `windows-latest` for you — push to `main` and
grab the result from the Actions tab.

## Known limitations

Being upfront about what this is and isn't:

- Ad/tracker filter lists are fetched once per launch, not continuously like a real extension — a same-day
  countermeasure change on a site's part may need a restart of Zaris to catch up
- Password autofill is a first pass, not a full password manager — no generation, no breach checking, no
  import/export yet
- Reopened tabs (via session restore or "reopen closed tab") get their last URL back, not their own
  back/forward history
- Windows only, for now — the codebase is plain Electron/Chromium so macOS/Linux builds are plausible, just
  untested

## Tech stack

Electron · Chromium (via Electron's `<webview>`) · [`@ghostery/adblocker-electron`](https://github.com/ghostery/adblocker)
· EasyList / EasyPrivacy / uBlock Origin filter lists · Electron's `safeStorage` for credential encryption
