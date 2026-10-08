# DAX measures dictionary

All measures are in the `Mesures` table.

## "POINTAGES" folder (time entries)
| Measure | Definition | Usage |
|---|---|---|
| `NBR_Pointages` | Number of time entries (`COUNT` on `Pointage_ID`) | Tracking KPI |
| `NBR_POINTAGE_vld` | Number of time entries where `Valider (True/False)` is true | Tracking KPI |
| `%_Pointage_Vld` | `NBR_POINTAGE_vld / NBR_Pointages` (999 if the denominator is zero) | "% validated" card |
| `Mois sélectionné` | Name of the selected month (`SELECTEDVALUE`) | Dynamic title |

## "QUOTA D'HEURE & TP" folder (hours quota & TechPack)
### Annual hours quota (contracts)
| Measure | Definition |
|---|---|
| `Heures Contrat` | Sum of the contracts' annual hours quota |
| `Solde heures QH` | Annual quota − hours of tickets flagged `QH = 1` |
| `% Solde Quota Restant` | `Solde heures QH / Heures Contrat` |
| `Statut Quota` | `CRITIQUE` if balance < 10 %, `VIGILANCE` if < 30 %, otherwise `OK` |
| `Nb Contrats Critique ou Vigilance` | Number of contracts with a critical or watch status |
| `Total Heures Réalisées / Interventions` | Sum of hours performed in the intervention orders |
| `Heures consommées cumulées` | Cumulative intervention hours from the start to the current date of the filter context |
| `Heures consommées cumulées N-1` | Same cumulative value shifted by one year (`PARALLELPERIOD`) |
| `80% Quota Atteint` | `Quota × 0.2` — gauge alert threshold (20 % left = 80 % consumed) |
| `Valeur seuil Quota` / `Valeur null` | Constants (30 and 0); `Valeur null` feeds the quota gauge, `Valeur seuil Quota` is not used on any page |

### TechPack
| Measure | Definition |
|---|---|
| `Heures consommées TechPack` | For each TechPack, sum of the hours of tickets created **between its validity start and end dates** (the calendar filter is deliberately ignored) |
| `Solde Heures TechPack` | Purchased hours − hours consumed during the validity period |
| `%_solde_TP_restant` | `Balance / Purchased hours` |
| `Statut TP` | `CRITIQUE` < 10 %, `VIGILANCE` < 30 %, otherwise `OK` (blank if there is no TechPack) |

## Business rules to remember
- A ticket can consume the **hours quota** (`QH = 1`) or a **TechPack** (`TP`): two separate counting circuits.
- TechPacks have a **validity period**; only tickets created within that period are counted.
- The status thresholds (10 % / 30 %) are identical for the quota and the TechPack.

## Points of attention
Noted while analysing the code; to be validated with the model owner:
1. **Status casing**: `Statut Quota` produces `"CRITIQUE"` / `"VIGILANCE"`, while `Nb Contrats Critique ou Vigilance` compares against `"Critique"` / `"Vigilance"`. The DAX `IN` operator ignores case for text, so the result is correct, but harmonising them would remove any ambiguity.
2. **Unprotected division**: `% Solde Quota Restant` and `%_solde_TP_restant` use `/` rather than `DIVIDE`; a contract without a quota would cause a division by zero.
3. **Misleading name**: `80% Quota Atteint` computes 20 % of the quota (the threshold where 20 % remains).
4. **Sentinel value**: `%_Pointage_Vld` returns `999` (rather than blank) when there are no time entries, which can skew an average.
5. **Hard-coded thresholds**: 10 % / 30 % could become parameters (the `Valeur seuil Quota` = 30 measure already exists but is not reused in `Statut Quota`).
