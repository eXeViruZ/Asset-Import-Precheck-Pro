# 03 — Severity Rules

## How Severity Works

Every asset starts at **Safe**. Rules run in sequence.
Severity can only increase — it is never lowered once raised.

```
Safe → Warning → HighRisk → Blocked
```

---

## Rule 1 — Path Safety

**Fires:** Always
**Result on trigger:** Warning

Checks the asset's `/Game/...` package path for:
- **Spaces** — can cause import failures in UE
- **Non-ASCII characters** (codepoint > 127) — can cause import failures on some platforms

---

## Rule 2 — Engine Name Collision

**Fires:** Always
**Result on trigger:** Warning

Checks the asset name (case-insensitive) against a list of reserved Unreal Engine class names:

`GameMode` · `GameState` · `PlayerController` · `PlayerState` · `HUD` · `DefaultPawn` · `WorldSettings` · `Level`

Assets with these names risk shadowing engine classes and causing unpredictable behavior.

---

## Rule 3 — Fallback Mode Note

**Fires:** When no `AssetRegistry.bin` was found
**Result on trigger:** Warning (minimum)

When AIPP runs in filesystem fallback mode (no registry), dependency checks are skipped.
All assets receive at minimum a Warning with a note explaining the limitation.

---

## Rule 4 — Name Conflict

**Fires:** When a target Content folder is provided
**Result on trigger:** High Risk

Checks if an asset with the same package path already exists in the target Content folder.
Both `.uasset` and `.umap` extensions are checked.

Importing a conflicting asset would silently overwrite the existing file.

---

## Rule 5 — Missing Dependencies

**Fires:** When a target Content folder is provided AND `AssetRegistry.bin` was loaded
**Result on trigger:** Blocked

For each hard `/Game/` package dependency of the asset:
1. Is it present in the source registry? → OK
2. Is it present on disk in the target Content folder? → OK
3. Neither found → **Blocked**, note added

Engine and plugin packages (`/Engine/`, `/Script/`, etc.) are intentionally ignored.
Only user-content dependencies under `/Game/` are checked.
