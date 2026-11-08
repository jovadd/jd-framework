# Changelog

Tutte le modifiche rilevanti, una voce per versione. Versioni con [semver](https://semver.org/lang/it/):
- **patch** (1.0.x): correzioni che non cambiano nomi né aspetto previsto;
- **minor** (1.x.0): nuovi token o classi, compatibili con i progetti esistenti;
- **major** (x.0.0): token o classi rinominati/rimossi, o cambi visibili ai default.

## [1.0.0] — 2026-11-08

Prima versione.

- `tokens.default.css`: scala tipografica fluida `--jd-text--2` … `--jd-text-7` (360→1440px, 16px×1.2 → 18px×1.333), spaziature fluide `--jd-space-3xs` … `--jd-space-3xl` con coppie, font, interlinea, tracking, pesi, container, gutter, misura, colori di base, focus e default delle classi di layout.
- `framework.css`: layer `jd.reset`, `jd.base`, `jd.layout`; classi `.jd-container`, `.jd-section`, `.jd-stack`, `.jd-cluster`, `.jd-grid`, `.jd-sidebar`, `.jd-switcher`, `.jd-flow`, `.jd-full-bleed`, `.jd-cq`.
- `demo/index.html`: pagina di verifica di scale e layout.
