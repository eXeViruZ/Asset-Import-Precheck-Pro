# Asset Import Precheck Pro

**Version:** 1.0.0
**Engine:** Unreal Engine 5.7
**Type:** Editor-only Plugin
**Author:** Tom Leon Vincent Hanke
**Support:** https://discord.gg/vgpmnN6nCR
**Source:** https://github.com/eXeViruZ

---

## What It Does

Asset Import Precheck Pro (AIPP) is a migration-safety tool for Unreal Engine.
Before you copy assets from one project to another, AIPP scans the source folder,
analyzes every asset, detects potential problems, and gives you a clear severity
rating — so you know exactly what will break before anything is touched.

**No guesswork. No broken references after the fact.**

---

## Key Features

- 5-step guided wizard: Source → Assets → Precheck → Target → Report
- Severity system with 4 levels: Safe / Warning / High Risk / Blocked
- Dependency visualization per asset (Flat list + Tree view)
- Conflict detection: checks if an asset already exists in the target project
- Missing dependency detection: flags assets whose hard dependencies cannot be resolved
- Engine name collision detection: warns when asset names match reserved UE class names
- Path safety checks: detects spaces and non-ASCII characters in package paths
- 3 import modes: Import All / Skip Conflicting / Import Safe Only
- AssetRegistry.bin support: full dependency data when a registry is available
- Filesystem fallback: works even without a registry (path + file size only)
- Async scan + import: no editor freeze, progress throbber during operation
- Last source path persisted across sessions
- Export report to clipboard or .csv file
- Accessible via Tools menu and Level Editor toolbar button

---

## Installation

1. Copy the `AssetImportPrecheckPro` folder into your project's `Plugins/` directory.
2. Reopen your project — UE will prompt to compile the plugin.
3. After compilation, the plugin is active automatically.

No additional configuration required.

---

## How to Open

Two entry points are available after installation:

| Entry Point | Location | Notes |
|---|---|---|
| **Tools menu** | Tools → *Asset Import Precheck Pro* | Always available |
| **Toolbar button** | Level Editor toolbar → *AIPP* button | Persistent one-click access, sits in the main editor toolbar alongside other editor tools |

---

## Workflow

### Step 1 — Source
Pick the **Content folder** of the source project (the project you want to import assets *from*).
You can also point to the project root — AIPP will auto-detect the Content subfolder.

### Step 2 — Assets
AIPP displays all `.uasset` and `.umap` files found in the source folder.
The list shows: asset name, class, package path, and file size.

### Step 3 — Precheck
Every asset is analyzed and assigned a severity:

| Severity  | Color  | Meaning                                                              |
|-----------|--------|----------------------------------------------------------------------|
| Safe      | Green  | No issues detected                                                   |
| Warning   | Yellow | Minor risks (spaces in path, no registry, name collision risk)       |
| High Risk | Orange | Asset already exists in target — would overwrite                     |
| Blocked   | Red    | Missing hard dependency — import will break references               |

Click any asset to see its full dependency list in the right panel.
Use the filter buttons to narrow the view: All / Warning+ / High Risk+ / Blocked Only.

### Step 4 — Target
Pick the **Content folder** of the target project and choose an import mode:

| Mode                | Behavior                                                          |
|---------------------|-------------------------------------------------------------------|
| Import All          | Copies all assets except Blocked ones                             |
| Skip Conflicting    | Also skips Warning and High Risk assets                           |
| Import Safe Only    | Only copies assets rated Safe                                     |

### Step 5 — Report
A full summary of what was copied, skipped, or failed.
Shows counts, per-asset results, and skip reasons.
Export to clipboard or save as `.csv`.

---

## AssetRegistry.bin

AIPP works in two scan modes, chosen automatically:

**Registry mode** (recommended)
Requires an `AssetRegistry.bin` file. AIPP searches these locations automatically:
- `<ProjectRoot>/AssetRegistry.bin` *(most common)*
- `<ProjectRoot>/Saved/AssetRegistry.bin`
- `<ContentFolder>/AssetRegistry.bin`

In registry mode, AIPP has full access to asset classes, tags, and hard package dependencies.

**Filesystem fallback mode**
Used when no registry is found. AIPP scans the filesystem directly.
Dependency checks are skipped. All assets receive a minimum Warning severity with a note explaining why.

**How to generate AssetRegistry.bin for a source project:**
1. Open the source project in Unreal Engine.
2. Run a cook or use *File → Cook Content for [Platform]*.
3. The registry will be written to `Saved/AssetRegistry.bin` or the project root.

---

## Severity Rules (Technical)

Rules run in this order on every asset. Severity only ever increases — it never decreases.

1. **Path Safety** — Warns if the package path contains spaces or non-ASCII characters.
2. **Engine Name Collision** — Warns if the asset name matches a reserved UE class name
   (`GameMode`, `GameState`, `PlayerController`, `PlayerState`, `HUD`, `DefaultPawn`, `WorldSettings`, `Level`).
3. **Fallback Mode Note** — If no registry is available, all assets get at minimum Warning.
4. **Name Conflict** *(requires target)* — Marks High Risk if an asset with the same path already exists
   in the target Content folder.
5. **Missing Dependencies** *(requires registry + target)* — Marks Blocked if a hard `/Game/` dependency
   is not present in the source registry and not found on disk in the target.

---

## Import Behavior

- Files are copied at the raw file level (`.uasset` / `.umap`).
- Blocked assets are always skipped regardless of mode.
- If a file already exists at the destination, it is skipped (no overwrite).
- Destination directories are created automatically if missing.
- The copy runs on a background thread — the editor stays responsive.

---

## Troubleshooting

**"Registry not found" warning on all assets**
No `AssetRegistry.bin` was found near the source folder. Generate one by cooking the source project,
or accept the filesystem-only scan.

**Assets show High Risk but I want to overwrite them**
Use *Import All* mode. High Risk assets are copied unless you explicitly choose
*Skip Conflicting* or *Import Safe Only*.

**Asset shows Blocked**
A hard dependency is missing from both the source package and the target project.
Importing this asset will result in broken references. Resolve the missing dependency first,
or accept the broken reference and manually fix it after import.

**The window doesn't open**
Check if the plugin is enabled: *Edit → Plugins → search "Asset Import Precheck Pro"*.

**"Import All" copies nothing**
Verify the target Content folder path is correct.
AIPP will not overwrite files that already exist at the destination.

---

## Limitations

- AIPP performs file-level copying only. It does not trigger UE's asset reimport pipeline.
- Redirectors from the source project are not carried over.
- Blueprint dependency graphs (soft references, function calls) are not analyzed —
  only hard package dependencies from the registry are checked.
- AIPP is an Editor-only plugin and has no runtime component.

---

## License

Copyright (c) 2026 Tom Leon Vincent Hanke. All rights reserved.
