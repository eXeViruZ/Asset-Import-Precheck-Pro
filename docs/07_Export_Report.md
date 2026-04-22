# 07 — Export & Report

## Report Step (Step 5)

After the import completes, AIPP shows a full summary:

| Section | Content |
|---|---|
| Header | Source path, target path, import mode used |
| Summary counts | Total assets · Copied · Skipped · Failed |
| Per-asset table | Asset name · Status · Skip reason or error |

---

## Export Options

### Copy to Clipboard

Click **Copy to Clipboard** to copy the full report as plain text.
Useful for pasting into tickets, chat, or notes.

### Export CSV

Click **Export CSV** to save the report as a `.csv` file.

The CSV contains one row per asset with the following columns:

| Column | Description |
|---|---|
| PackagePath | Full `/Game/...` source package path |
| Status | `Copied`, `Skipped`, or `Failed` |
| Reason | Skip reason or error message (empty if Copied) |
| DestinationPath | Full absolute destination path on disk |

The file save dialog opens to let you choose the output location.

---

## Starting Over

Click **New Scan** on the report step to:
1. Reset the engine (clear all results)
2. Return to Step 1 (source picker)
3. Pre-populate the source path from the last session

The previous import is not undone — files that were copied remain on disk.
