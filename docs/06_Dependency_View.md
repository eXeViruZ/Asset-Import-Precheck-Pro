# 06 — Dependency View

## Where to Find It

The dependency view is visible on **Step 3 (Precheck)**.

Select any asset row in the left list — the right panel updates immediately to show
that asset's dependencies.

---

## Flat List

Shows all hard package dependencies of the selected asset as a simple list.

| Indicator | Meaning |
|---|---|
| Normal text | Dependency is present (in source registry or target disk) |
| Red text | Dependency is **missing** — not found in source or target |

---

## Tree View

Shows the same data in a hierarchical structure:

```
SelectedAsset
├── /Game/Characters/HeroMesh      ← present
├── /Game/Materials/HeroMat        ← present
└── /Game/Animations/HeroAnim      ← MISSING (red)
```

The tree is one level deep — it shows the direct dependencies of the selected asset.
Transitive dependencies (dependencies of dependencies) are not expanded automatically.

---

## Notes

- Dependencies are derived from the `AssetRegistry.bin` hard dependency graph.
- In **filesystem fallback mode**, no dependencies are shown (the list will be empty).
- Only `/Game/` package paths are shown. Engine and plugin packages are filtered out.
- The dependency view is read-only — it is for inspection only, not for editing.
