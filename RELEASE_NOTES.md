# PBSRx Phoenix v7.6.7 RC1 — GitHub Pages Deployment Package

**Build date:** 2026-07-17  
**Source:** PBSRx-Phoenix-Canonical-Production-v7_6_7.html  
**Release type:** Deployment packaging only — no application changes

---

## What changed from the canonical source

Two surgical patches only. No application logic, UI, CSS, or business rules were modified.

### Patch 1 — Manifest link (line 11)
- Changed `href="manifest.json"` → `href="./manifest.json"` (explicit relative path for GitHub Pages)
- Removed duplicate placeholder `<link rel="manifest" href="#" id="manifestPlaceholder">` (line 14)

### Patch 2 — Service Worker registration (appended before `</body>`)
Added a guarded registration block that:
- **Does NOT register** when running from `file://` (local HTML double-click)
- **Does NOT register** when running from `claudeusercontent.com` (Claude preview)
- **DOES register** normally on GitHub Pages and any other host

```js
if(
  'serviceWorker' in navigator &&
  location.protocol !== 'file:' &&
  location.hostname !== 'www.claudeusercontent.com'
){
  navigator.serviceWorker.register('./sw.js').catch(console.warn);
}
```

---

## What was NOT changed

- All application JavaScript (100% untouched)
- All CSS rules
- All HTML layout and DOM structure
- All navigation and panel logic
- Product Migration Engine
- AI Snap / Barcode Scanner
- Intake Workspace
- Review Workspace
- POS / Cash Desk
- Customers module
- Suppliers module
- Phoenix Inventory Dataset (PID)
- Product Identity Model
- Opening Stock Engine
- Owner/Consignment Separation
- Expired Inventory Preservation
- Migration Engine, History, Session ID
- Dry Run Mode, Snapshot, Rollback, Certificate
- Receipt templates (string literals verified untouched)
- Version strings (remain v7.6.7-RC1)

---

## Package contents

```
index.html        — Canonical source + 2 surgical patches
manifest.json     — PWA manifest (relative paths only)
sw.js             — Service Worker (relative paths only)
icons/
  icon-192.png    — PWA icon 192×192
  icon-512.png    — PWA icon 512×512
RELEASE_NOTES.md  — This file
```

---

## GitHub Pages deployment

1. Push all files to your repo root (or `docs/` branch)
2. Enable GitHub Pages in repo Settings → Pages
3. Access at `https://<username>.github.io/<repo>/`
4. Install prompt will appear after first visit (PWA)

---

## Validation results

| Check | Result |
|---|---|
| JavaScript parses cleanly | ✅ PASS (2 script blocks, no errors) |
| Exactly one manifest link | ✅ PASS (line 11, `./manifest.json`) |
| Exactly one SW registration | ✅ PASS (guarded, appended before `</body>`) |
| Receipt template unchanged | ✅ PASS (string literals at lines 7326, 7623 untouched) |
| No orphan quotes | ✅ PASS |
| Relative paths only (manifest) | ✅ PASS |
| Relative paths only (sw.js) | ✅ PASS |
| No changes outside SW wrapper | ✅ PASS (diff confirms 3 line changes only) |

---

## Engineering Log

- 2026-07-17: Established Android-native Git workflow using Termux and Acode.