# 02 — Workflow

AIPP guides you through a 5-step wizard. Each step must be completed in order.

---

## Step 1 — Source

Pick the **Content folder** of the source project (the project you want to import assets *from*).

- You can point to the project root — AIPP will auto-detect the `Content` subfolder.
- The last used path is remembered across editor sessions.
- Click **Browse** to open the OS folder picker, or type the path manually.
- Click **Run Scan** to proceed.

---

## Step 2 — Assets

AIPP displays all `.uasset` and `.umap` files found in the source folder.

Columns shown per asset:

| Column | Description |
|---|---|
| Name | Bare asset name |
| Class | UE asset class (e.g. `SkeletalMesh`, `Blueprint`) |
| Package Path | Full `/Game/...` package path |
| File Size | Size on disk |

Click **Run Precheck** to analyze all assets.

---

## Step 3 — Precheck

Every asset is analyzed and assigned a severity rating:

| Severity | Color | Meaning |
|---|---|---|
| Safe | Green | No issues detected |
| Warning | Yellow | Minor risk (spaces in path, no registry, name collision) |
| High Risk | Orange | Asset already exists in target — would overwrite |
| Blocked | Red | Missing hard dependency — import will break references |

- Click any asset row to inspect its **dependency list** in the right panel.
- Use the **filter buttons** to narrow the view: All / Warning+ / High Risk+ / Blocked Only.
- The summary bar shows the total count and a severity breakdown.

Click **Continue** to proceed to target selection.

---

## Step 4 — Target

Pick the **Content folder** of the target project (the project you want to import assets *into*).

Choose an **import mode**:

| Mode | Behavior |
|---|---|
| Import All | Copies all assets except Blocked ones |
| Skip Conflicting | Also skips Warning and High Risk assets |
| Import Safe Only | Only copies assets rated Safe |

Click **Start Import** to begin. A throbber is shown while the import runs in the background.

---

## Step 5 — Report

Shows the full import summary:

- **Copied / Skipped / Failed** counts
- Per-asset result with status and reason
- Source and target paths
- **Copy to Clipboard** button
- **Export CSV** button
- **New Scan** button to start over
