# Report description

16:9 format (1280 × 720), Power BI base theme **CY26SU04**, embedded Axians logo. Home page: *Tickets*. Two bookmarks (*Signet Mois*, *Signet Semaine*) switch between the monthly and weekly timesheet views.

## Visible pages
| Page | Content |
|---|---|
| **Tickets** | Slicers (year, month, category, status, company, site, technician, ticket no.); ticket count card; donut charts *by status* and *by priority*; *tickets / day* line chart; *breakdown by client* columns |
| **Suivi du quota d'heures – Générale** (hours quota tracking – overview) | Year slicer; *contracts on watch or critical* card; *hours consumed per company* column chart; contract table (quota, balance, % remaining, status) |
| **Suivi du TechPack – Générale** (TechPack tracking – overview) | Year slicer; balance / consumed card; *hours consumed per company* column chart; TechPack table (period, hours, balance, status) |
| **Feuilles de temps** (timesheets) | *Validation tracking* KPI per week, % validated, time entry table (with an "Open" link to the record), project × technician matrix |
| **Suivi Interventions Planifiées** (planned interventions tracking) | Gantt chart (custom **Deneb** visual) of time entries/interventions per technician, filtered by date |

## Detail pages (hidden, reachable via a button)
| Page | Role |
|---|---|
| **Détails Tickets** | List of a client's tickets with report and hours performed |
| **Détails quota d'heures / Client** | Quota consumption gauge, % remaining, cumulative curve *current year vs previous year*, OI details |
| **Détails Ticket TechPack / Client** | OI details for a TechPack client |
| **Feuilles de temps (visuel mois)** | Monthly variant of the *Feuilles de temps* page |
| **Info-Bulle Entreprise** | Tooltip page (400 × 500): a company's tickets on hover |

## Custom visuals
- **Deneb** (Vega-Lite) — used for the planned interventions Gantt chart.
