# Sécurité et configuration

## Paramètres de connexion à renseigner
Les requêtes interrogent l'API Safeplace avec une URL de base (`url`) et, pour chaque source, trois paramètres :

| Source | Chemin API | Identifiant | Secret |
|---|---|---|---|
| Pointages | `key_ptg` | `id_ptg` | `id_ptg_s` |
| Tickets (et tables liées) | `key_tic` | `id_tic` | `id_tic_s` |
| Ordres d'interventions | `key_OI` | `id_OI` | `id_OI_s` |
| Contrats | `key_ctr` | `id_ctr` | `id_ctr_s` |
| Contrats TechPack | `key_ctp` | `id_ctp` | `id_ctp_s` |

Le dépôt contient des valeurs fictives `<A_RENSEIGNER_...>` ; à remplacer dans Power BI Desktop (*Transformer les données › Gérer les paramètres*). L'API reçoit `client_id` et `client_secret` dans les **en-têtes HTTP**.

## Ce qui a été retiré avant publication
- Les identifiants `client_id` / `client_secret` et les chemins d'accès de l'API, qui étaient écrits en clair dans `expressions.tmdl`.
- Le cache de données `.pbi/cache.abf` (données importées) et les réglages locaux `.pbi/localSettings.json`.

## Recommandations
- **Révoquer / régénérer** les identifiants API d'origine : ils ont circulé en clair (fichier de travail, échanges) et ne doivent plus être considérés comme secrets.
- Ne jamais committer de valeurs réelles : conserver le `.gitignore` fourni et, à terme, stocker les secrets hors du modèle (Power BI Service, Azure Key Vault, passerelle de données).
- Les données sont issues de systèmes internes (tickets clients, pointages nominatifs, taux horaires) : ne pas publier de capture ou d'export du rapport sans anonymisation.
