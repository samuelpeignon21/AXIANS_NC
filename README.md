# AXIANS_NC — Tableau de bord Power BI « Teepee / Safeplace »

Rapport Power BI (format **PBIP**, versionnable avec Git) de pilotage de l'activité de support et d'intervention
d'Axians Nouvelle-Calédonie, alimenté par l'API **Safeplace** (plateforme Teepee).

> **Statut** : modèle `Teepee_Model_2.1` — 10 pages de rapport, 12 tables, 19 mesures DAX.

## À quoi sert ce rapport ?

| Besoin métier | Où le voir |
|---|---|
| Suivre les **tickets** d'assistance (statut, priorité, client, volume par jour) | Page *Tickets* + *Détails Tickets* |
| Suivre la consommation du **quota d'heures annuel** des contrats, avec alerte (OK / Vigilance / Critique) | Pages *Suivi du quota d'heures* + *Détails quota d'heures / Client* |
| Suivre la consommation des **TechPack** (forfaits d'heures avec période de validité) | Pages *Suivi du TechPack* + *Détails Ticket TechPack / Client* |
| Contrôler la **validation des feuilles de temps** (pointages) par semaine ou par mois | Pages *Feuilles de temps* et *Feuilles de temps (visuel mois)* |
| Visualiser les **interventions planifiées** par technicien (diagramme de Gantt) | Page *Suivi Interventions Planifiées* |

## Architecture en un coup d'œil

```
API Safeplace (HTTPS, authentification par en-têtes)
   │  5 fichiers Excel exposés : Pointages · Tickets · Ordres d'Interventions · Contrats · Contrats TechPack
   ▼
Power Query  (≈ 70 requêtes, groupées « API Safeplace »)
   00_Paramètres → 01_Sources_API (SRC_*) → 02_Transformées (*_TSF) → 03_Model (FACT / DIM / RELATIONS)
   ▼
Modèle sémantique (schéma en étoile, import) : 3 tables de faits, 7 dimensions, 1 calendrier, 1 table de mesures
   ▼
Rapport : 10 pages, thème Power BI CY26SU04, visuel personnalisé Deneb (Gantt)
```

Détails dans :
- [`docs/modele-de-donnees.md`](docs/modele-de-donnees.md) — tables, relations, schéma, chaîne Power Query
- [`docs/mesures-dax.md`](docs/mesures-dax.md) — dictionnaire des mesures et règles de calcul
- [`docs/rapport.md`](docs/rapport.md) — description page par page
- [`docs/securite-et-configuration.md`](docs/securite-et-configuration.md) — paramètres à renseigner, gestion des secrets

## Structure du dépôt

```
.
├── README.md
├── .gitignore                         # exclut le cache de données et les réglages locaux
├── docs/                              # documentation (ce que vous lisez)
└── powerbi/
    ├── Teepee_Model_2.1.pbip          # point d'entrée : à ouvrir dans Power BI Desktop
    ├── Teepee_Model_2.1.SemanticModel/  # modèle au format TMDL (tables, mesures, relations, requêtes)
    └── Teepee_Model_2.1.Report/         # rapport au format PBIR (pages, visuels, signets, thème)
```

## Ouvrir le projet

1. Installer **Power BI Desktop** (version récente) et activer *Fichier › Options › Fonctionnalités en préversion › « Format de projet Power BI (.pbip) »* ainsi que le *format de métadonnées de rapport Power BI (PBIR)* si nécessaire.
2. Cloner ce dépôt, puis ouvrir `powerbi/Teepee_Model_2.1.pbip`.
3. **Renseigner les paramètres de connexion** (`Accueil › Transformer les données › Gérer les paramètres`). Les identifiants API ne sont volontairement **pas** dans le dépôt : voir [`docs/securite-et-configuration.md`](docs/securite-et-configuration.md).
4. Actualiser les données.

Le dépôt ne contient **aucune donnée** (le cache `.abf` est exclu) : sans accès à l'API Safeplace, le modèle s'ouvre mais reste vide.

## Points d'attention connus

- La table calendrier démarre au **06/07/2026** (valeur codée en dur dans `TAB_Date`) et va jusqu'à la fin du mois courant.
- Les seuils d'alerte (10 % / 30 %) sont codés en dur dans les mesures `Statut Quota` et `Statut TP`.
- `Statut Quota` renvoie `CRITIQUE` / `VIGILANCE` en majuscules, alors que `Nb Contrats Critique ou Vigilance` teste `"Critique"` / `"Vigilance"` : voir l'analyse dans [`docs/mesures-dax.md`](docs/mesures-dax.md#points-de-vigilance).
