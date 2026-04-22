# 01 — Installation

## Requirements

- Unreal Engine 5.7
- Windows (64-bit)

## Steps

1. Copy the `AssetImportPrecheckPro` folder into your project's `Plugins/` directory:

```
YourProject/
└── Plugins/
    └── AssetImportPrecheckPro/
        ├── AssetImportPrecheckPro.uplugin
        ├── Source/
        └── ...
```

2. Reopen your project — Unreal Engine will prompt you to compile the plugin.
3. Click **Yes** to compile.
4. After compilation the plugin is active automatically. No further setup required.

## Verify Installation

- Open **Edit → Plugins**
- Search for *"Asset Import Precheck Pro"*
- The plugin should appear as **Enabled**

## How to Open the Tool

Two entry points:

| Entry Point | Location |
|---|---|
| Tools menu | **Tools → Asset Import Precheck Pro** |
| Toolbar button | **Level Editor toolbar → AIPP** button — persistent one-click access in the main editor toolbar, visible as long as the plugin is enabled |
