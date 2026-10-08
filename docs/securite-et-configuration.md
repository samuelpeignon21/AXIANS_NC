# Security and configuration

## Connection parameters to fill in
The queries call the Safeplace API with a base URL (`url`) and, for each source, three parameters:

| Source | API path | Identifier | Secret |
|---|---|---|---|
| Time entries (*Pointages*) | `key_ptg` | `id_ptg` | `id_ptg_s` |
| Tickets (and related tables) | `key_tic` | `id_tic` | `id_tic_s` |
| Intervention orders | `key_OI` | `id_OI` | `id_OI_s` |
| Contracts | `key_ctr` | `id_ctr` | `id_ctr_s` |
| TechPack contracts | `key_ctp` | `id_ctp` | `id_ctp_s` |

The repository contains dummy `<A_RENSEIGNER_...>` ("to be filled in") values; replace them in Power BI Desktop (*Transform data › Manage parameters*). The API receives `client_id` and `client_secret` in the **HTTP headers**.

## What was removed before publication
- The `client_id` / `client_secret` credentials and the API access paths, which were written in clear text in `expressions.tmdl`.
- The data cache `.pbi/cache.abf` (imported data) and the local settings `.pbi/localSettings.json`.

## Recommendations
- **Revoke / regenerate** the original API credentials: they circulated in clear text (working file, exchanges) and must no longer be considered secret.
- Never commit real values: keep the provided `.gitignore` and, in the long run, store secrets outside the model (Power BI Service, Azure Key Vault, data gateway).
- The data comes from internal systems (client tickets, named time entries, hourly rates): do not publish screenshots or exports of the report without anonymisation.
