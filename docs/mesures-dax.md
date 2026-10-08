# Dictionnaire des mesures DAX

Toutes les mesures sont dans la table `Mesures`.

## Dossier « POINTAGES »
| Mesure | Définition | Usage |
|---|---|---|
| `NBR_Pointages` | Nombre de pointages (`COUNT` sur `Pointage_ID`) | KPI de suivi |
| `NBR_POINTAGE_vld` | Nombre de pointages dont `Valider (True/False)` = vrai | KPI de suivi |
| `%_Pointage_Vld` | `NBR_POINTAGE_vld / NBR_Pointages` (999 si dénominateur nul) | Carte « % validé » |
| `Mois sélectionné` | Nom du mois sélectionné (`SELECTEDVALUE`) | Titre dynamique |

## Dossier « QUOTA D'HEURE & TP »
### Quota d'heures annuel (contrats)
| Mesure | Définition |
|---|---|
| `Heures Contrat` | Somme du quota d'heures / an des contrats |
| `Solde heures QH` | Quota annuel − heures des tickets marqués `QH = 1` |
| `% Solde Quota Restant` | `Solde heures QH / Heures Contrat` |
| `Statut Quota` | `CRITIQUE` si solde < 10 %, `VIGILANCE` si < 30 %, sinon `OK` |
| `Nb Contrats Critique ou Vigilance` | Nombre de contrats en statut critique ou vigilance |
| `Total Heures Réalisées / Interventions` | Somme des heures réalisées dans les ordres d'intervention |
| `Heures consommées cumulées` | Cumul des heures d'intervention depuis le début jusqu'à la date courante du contexte |
| `Heures consommées cumulées N-1` | Même cumul décalé d'un an (`PARALLELPERIOD`) |
| `80% Quota Atteint` | `Quota × 0,2` — seuil d'alerte de la jauge (il reste 20 % = 80 % consommé) |
| `Valeur seuil Quota` / `Valeur null` | Constantes (30 et 0) ; `Valeur null` alimente la jauge du quota, `Valeur seuil Quota` n'est utilisée dans aucune page |

### TechPack
| Mesure | Définition |
|---|---|
| `Heures consommées TechPack` | Pour chaque TechPack, somme des heures de tickets créés **entre sa date de début et de fin de validité** (le filtre calendrier est volontairement ignoré) |
| `Solde Heures TechPack` | Heures achetées − heures consommées sur la période de validité |
| `%_solde_TP_restant` | `Solde / Heures achetées` |
| `Statut TP` | `CRITIQUE` < 10 %, `VIGILANCE` < 30 %, sinon `OK` (vide si pas de TechPack) |

## Règles métier à retenir
- Un ticket peut consommer le **quota d'heures** (`QH = 1`) ou un **TechPack** (`TP`) : deux circuits distincts de décompte.
- Les TechPack ont une **période de validité** ; seuls les tickets créés dans cette période sont décomptés.
- Les seuils de statut (10 % / 30 %) sont identiques pour le quota et le TechPack.

## Points de vigilance
Relevés lors de l'analyse du code ; à valider avec le propriétaire du modèle :
1. **Casse des statuts** : `Statut Quota` produit `"CRITIQUE"` / `"VIGILANCE"`, alors que `Nb Contrats Critique ou Vigilance` compare à `"Critique"` / `"Vigilance"`. L'opérateur `IN` de DAX ignore la casse pour le texte, donc le résultat est correct, mais l'harmoniser éviterait toute ambiguïté.
2. **Division sans protection** : `% Solde Quota Restant` et `%_solde_TP_restant` utilisent `/` et non `DIVIDE` ; un contrat sans quota provoquerait une division par zéro.
3. **Nom trompeur** : `80% Quota Atteint` calcule 20 % du quota (le seuil où il reste 20 %).
4. **Valeur sentinelle** : `%_Pointage_Vld` renvoie `999` (et non vide) quand il n'y a aucun pointage, ce qui peut fausser une moyenne.
5. **Seuils codés en dur** : 10 % / 30 % pourraient devenir des paramètres (la mesure `Valeur seuil Quota` = 30 existe déjà mais n'est pas réutilisée dans `Statut Quota`).
