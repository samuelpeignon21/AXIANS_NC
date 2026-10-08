# Description du rapport

Format 16:9 (1280 × 720), thème de base Power BI **CY26SU04**, logo Axians intégré. Page d'accueil : *Tickets*. Deux signets (*Signet Mois*, *Signet Semaine*) permettent de basculer entre les vues mensuelle et hebdomadaire des feuilles de temps.

## Pages visibles
| Page | Contenu |
|---|---|
| **Tickets** | Segments (année, mois, catégorie, statut, entreprise, site, technicien, n° de ticket) ; carte du nombre de tickets ; anneaux *par statut* et *par priorité* ; courbe *tickets / jour* ; colonnes *répartition par client* |
| **Suivi du quota d'heures – Générale** | Segment année ; carte *contrats en vigilance ou critiques* ; histogramme *heures consommées par entreprise* ; tableau des contrats (quota, solde, % restant, statut) |
| **Suivi du TechPack – Générale** | Segment année ; carte solde / consommé ; histogramme *heures consommées par entreprise* ; tableau des TechPack (période, heures, solde, statut) |
| **Feuilles de temps** | KPI *suivi des validations* par semaine, % validé, tableau des pointages (avec lien « Ouvrir » vers la fiche), matrice affaire × technicien |
| **Suivi Interventions Planifiées** | Diagramme de Gantt (visuel personnalisé **Deneb**) des pointages/interventions par technicien, filtré par date |

## Pages de détail (masquées, accessibles par bouton)
| Page | Rôle |
|---|---|
| **Détails Tickets** | Liste des tickets d'un client avec compte rendu et heures réalisées |
| **Détails quota d'heures / Client** | Jauge de consommation du quota, % restant, courbe cumulée *année en cours vs N-1*, détail des OI |
| **Détails Ticket TechPack / Client** | Détail des OI pour un client TechPack |
| **Feuilles de temps (visuel mois)** | Variante mensuelle de la page *Feuilles de temps* |
| **Info-Bulle Entreprise** | Page infobulle (400 × 500) : tickets d'une entreprise au survol |

## Visuels personnalisés
- **Deneb** (Vega-Lite) — utilisé pour le Gantt des interventions planifiées.
