# AXIANS_NC — "Teepee / Safeplace" Power BI dashboard

Power BI report (**PBIP** format, Git-friendly) for steering the support and intervention activity of Axians New Caledonia, fed by the **Safeplace** API (Teepee platform).

> **Status**: `Teepee_Model_2.1` model — 10 report pages, 12 tables, 19 DAX measures.

## What is this report for?

| Business need | Where to see it |
|---|---|
| Track support **tickets** (status, priority, client, volume per day) | *Tickets* + *Détails Tickets* pages |
| Track consumption of the contracts' **annual hours quota**, with alerts (OK / Watch / Critical) | *Suivi du quota d'heures* + *Détails quota d'heures / Client* pages |
| Track consumption of **TechPacks** (hour bundles with a validity period) | *Suivi du TechPack* + *Détails Ticket TechPack / Client* pages |
| Check **timesheet validation** (time entries) by week or by month | *Feuilles de temps* and *Feuilles de temps (visuel mois)* pages |
| Visualise **planned interventions** per technician (Gantt chart) | *Suivi Interventions Planifiées* page |

## Architecture at a glance

```
Safeplace API (HTTPS, header-based authentication)
   │  5 Excel files exposed: Time entries · Tickets · Intervention orders · Contracts · TechPack contracts
   ▼
Power Query  (≈ 70 queries, grouped under "API Safeplace")
   00_Paramètres → 01_Sources_API (SRC_*) → 02_Transformées (*_TSF) → 03_Model (FACT / DIM / RELATIONS)
   ▼
Semantic model (star schema, import): 3 fact tables, 7 dimensions, 1 calendar, 1 measures table
   ▼
Report: 10 pages, Power BI CY26SU04 theme, custom Deneb visual (Gantt)
```

Details in:
- [`docs/modele-de-donnees.md`](docs/modele-de-donnees.md) — tables, relationships, schema, Power Query chain
- [`docs/mesures-dax.md`](docs/mesures-dax.md) — measure dictionary and calculation rules
- [`docs/rapport.md`](docs/rapport.md) — page-by-page description
- [`docs/securite-et-configuration.md`](docs/securite-et-configuration.md) — parameters to fill in, secrets handling

(The documentation files keep their original French file names; their content is in English.)

## Repository structure

```
.
├── README.md
├── .gitignore                         # excludes the data cache and local settings
├── docs/                              # documentation (what you are reading)
└── powerbi/
    ├── Teepee_Model_2.1.pbip          # entry point: open in Power BI Desktop
    ├── Teepee_Model_2.1.SemanticModel/  # model in TMDL format (tables, measures, relationships, queries)
    └── Teepee_Model_2.1.Report/         # report in PBIR format (pages, visuals, bookmarks, theme)
```

## Opening the project

1. Install a recent **Power BI Desktop** and enable *File › Options › Preview features › "Power BI Project (.pbip) format"* as well as the *Power BI Enhanced Report Format (PBIR)* if needed.
2. Clone this repository, then open `powerbi/Teepee_Model_2.1.pbip`.
3. **Fill in the connection parameters** (`Home › Transform data › Manage parameters`). The API credentials are deliberately **not** in the repository: see [`docs/securite-et-configuration.md`](docs/securite-et-configuration.md).
4. Refresh the data.

The repository contains **no data** (the `.abf` cache is excluded): without access to the Safeplace API the model opens but stays empty.

## Known points of attention

- The calendar table starts on **2026-07-06** (hard-coded in `TAB_Date`) and runs to the end of the current month.
- Alert thresholds (10 % / 30 %) are hard-coded in the `Statut Quota` and `Statut TP` measures.
- `Statut Quota` returns `CRITIQUE` / `VIGILANCE` in upper case, while `Nb Contrats Critique ou Vigilance` tests `"Critique"` / `"Vigilance"`: see the analysis in [`docs/mesures-dax.md`](docs/mesures-dax.md#points-of-attention).
