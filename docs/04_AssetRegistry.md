# 04 — AssetRegistry.bin

## Why It Matters

`AssetRegistry.bin` is an Unreal Engine cache file that stores asset metadata:
class names, tags, and — most importantly — **hard package dependencies**.

AIPP uses it to know *which assets depend on which other assets*.
Without it, dependency checks are impossible.

---

## Scan Modes

AIPP automatically detects which mode to use:

### Registry Mode (Recommended)

Full analysis. Provides:
- Asset class names
- Asset tags
- Complete hard dependency graph
- Missing dependency detection (Rule 5)

### Filesystem Fallback Mode

Used when no registry is found. Provides:
- Asset file paths
- File sizes
- Package path derivation

**Dependency checks are skipped.** All assets receive a minimum Warning severity.

---

## Where AIPP Looks for the Registry

AIPP searches these locations automatically, in order:

| Priority | Path |
|---|---|
| 1 | `<ProjectRoot>/AssetRegistry.bin` |
| 2 | `<ProjectRoot>/Saved/AssetRegistry.bin` |
| 3 | `<ContentFolder>/AssetRegistry.bin` |

---

## How to Generate AssetRegistry.bin

The registry is generated automatically during a cook.

1. Open the **source project** in Unreal Engine.
2. Go to **Platforms → [Your Platform] → Cook Content**.
3. After cooking, find `AssetRegistry.bin` at:
   - `Saved/AssetRegistry.bin` — or —
   - The project root (depending on UE version and cook settings)
4. Place or leave it in one of the locations listed above.

> **Tip:** You do not need a full shipping cook. A development cook is sufficient to generate the registry.

---

## Registry Status in the UI

The Precheck step (Step 3) summary bar always shows the current registry status:

- ✅ `Registry loaded — full dependency analysis available`
- ⚠️ `Registry not found — dependency checks skipped (filesystem fallback)`
