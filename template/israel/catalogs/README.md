# Catalogs

Drop catalog screenshots / saved listings into per-vendor subdirectories. Recommended layout for Israel:

```
catalogs/
├── ksp/        # KSP.co.il
├── ivory/      # Ivory.co.il
├── bug/        # Bug.co.il
├── tms/        # TMS.co.il
├── ali/        # AliExpress
└── other/      # anything else (rename per vendor)
```

Claude Code is multimodal — it reads PNG/JPG screenshots directly and extracts product + price data via the `/shopping:catalog-to-json` command.
