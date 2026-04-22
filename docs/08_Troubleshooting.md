# 08 — Troubleshooting

---

### "Registry not found" warning on all assets

**Cause:** No `AssetRegistry.bin` was found near the source folder.

**Fix:** Generate the registry by cooking the source project
(*Platforms → Cook Content*), then place `AssetRegistry.bin` in the project root
or `Saved/` folder of the source project.

Alternatively, accept the filesystem-only scan — all rules except dependency
checks still run.

---

### All assets show Blocked

**Cause:** Usually means the registry was loaded but the assets reference dependencies
that are not bundled in the source package and not present in the target.

**Fix:** Make sure the source folder contains the complete asset set including all
dependencies, not just a partial selection.

---

### High Risk assets — I want to overwrite them

**Cause:** Assets with the same path already exist in the target.

**Fix:** Use **Import All** mode. High Risk assets are copied in Import All mode.
AIPP only skips High Risk in *Skip Conflicting* and *Import Safe Only* modes.

Note: Existing files at the exact destination path are still skipped (no overwrite).
If you need to replace an existing file, delete it from the target first.

---

### The window doesn't open

**Fix:**
1. Go to *Edit → Plugins*
2. Search for *"Asset Import Precheck Pro"*
3. Make sure it is enabled
4. Restart the editor if you just enabled it

---

### Import All copies nothing

**Cause:** All destination files already exist on disk (no overwrite behavior).

**Fix:** Verify the target Content folder path is correct. If files already exist
and you want to replace them, remove them from the target folder first.

---

### Step 3 shows 0 assets

**Cause:** The source folder scan returned no `.uasset` or `.umap` files.

**Fix:** Confirm the selected path is the actual `Content/` folder of the source project,
not the project root or a subfolder. AIPP attempts auto-detection but will fall back
to the path as given if `Content/` is not found.

---

### Export CSV produces an empty file

**Cause:** The import ran but all assets were skipped.

**Fix:** The CSV correctly reflects the import result. If no assets were copied,
review the per-asset status column to see the skip reasons.
