# 05 — Import Modes

## Overview

AIPP offers three import modes. The mode is selected on **Step 4 (Target)** before starting the import.

| Mode | Description |
|---|---|
| **Import All** | Copies every asset except those rated Blocked |
| **Skip Conflicting** | Skips Blocked, High Risk, and Warning assets |
| **Import Safe Only** | Only copies assets rated Safe |

---

## Behavior Per Severity

| Severity | Import All | Skip Conflicting | Import Safe Only |
|---|---|---|---|
| Safe | ✅ Copied | ✅ Copied | ✅ Copied |
| Warning | ✅ Copied | ⏭ Skipped | ⏭ Skipped |
| High Risk | ✅ Copied | ⏭ Skipped | ⏭ Skipped |
| Blocked | ❌ Skipped | ❌ Skipped | ❌ Skipped |

**Blocked assets are always skipped, regardless of mode.**

---

## File Copy Behavior

- Files are copied at the raw disk level (`.uasset` / `.umap`).
- If a file **already exists** at the destination, it is **skipped** (no silent overwrites).
- Destination directories are **created automatically** if they do not exist.
- The copy runs on a **background thread** — the editor stays fully responsive.
- UE's asset reimport pipeline is **not triggered** — this is a raw file copy.

---

## Choosing the Right Mode

| Situation | Recommended Mode |
|---|---|
| First-time migration, clean target project | Import All |
| Target project has existing assets you want to preserve | Skip Conflicting |
| Strict migration, only verified clean assets | Import Safe Only |
| Investigating issues before committing | Run precheck first, review Blocked assets |
