# CapellanIA — Chapelles Castrales pour Crusader Kings III

Mod CK3 implémentant le **Système de Chapelles Castrales** (GDD v2.0, intégration Royal Court) : une institution de cour dynamique articulant le spirituel, le politique et le matériel, fondée sur le principe *« tout avantage crée une obligation »*.

## Fonctionnalités (MVP du GDD)

### 1. Fondation interactive
- Décision **Fonder une Chapelle de Cour** (100 Or) → chaîne d'événements en 3 étapes :
  - **Dédicace** : Notre-Dame (piété), Saint-Michel (prestige) ou Saint-Martin (opinion du clergé) — chaque choix confère un modificateur permanent.
  - **Dotation** : modeste (gratuit), moyenne (50 Or, +capacité) ou prestigieuse (120 Or, +capacité, 1 livre liturgique de départ).
  - **Statut canonique** : licence épiscopale (+conformité) ou fondation clandestine (−conformité, risque juridictionnel accru).

### 2. Boucle centrale : Capacité Liturgique et Messes Fictives
- **Capacité** (calculée en continu) = base 4 + personnel employé (Premier chapelain +4, Maître de chapelle +2, Chantre +1, Gardien +1) + livres liturgiques (×2) + mobilier + dotation − absence du personnel en voyage.
- **Charge** = offices quotidiens (2) + 2 par **messe fondée**.
- Fonder une messe anniversaire (40 Or) : +piété, +prestige dynastique, et des paliers de bénédiction mensuels (1/3/5 messes). **Réformer** une fondation libère de la charge mais scandalise la famille (malus d'opinion 5 ans, −prestige dynastique).
- En **surcharge**, la dette liturgique (« Messes Fictives ») s'accumule chaque trimestre :
  - **Phase 1** (dette ≥1) : perte de piété mensuelle.
  - **Phase 2** (dette ≥3) : l'Évêque gagne un hameçon pour « Négligence Sacrée ».
  - **Phase 3** (dette ≥5) : scandale public (−prestige, −prestige dynastique, option de pénitence).

### 3. Personnel de la chapelle (postes de cour)
- Maître de chapelle, Premier chapelain, Chantre principal, Gardien du trésor.
- Dérive trimestrielle : le Premier chapelain restaure la conformité, le Maître la discipline ; sans encadrement, la discipline s'érode.
- Événement annuel de **Conflit de Préséance** entre le Maître et le Premier chapelain : trancher entre Prestige et Conformité (ou réconcilier avec Diplomatie 12).

### 4. Livres liturgiques
- Décision/bouton **Commander des Livres** → choix d'atelier : local (60 Or), scriptorium monastique (80 Or, +conformité/+piété), atelier flamand (120 Or, +2 livres et prestige, 25 % de casse au transport). Maximum 3 niveaux, cooldown 2 ans.

### 5. Itinérance (Travel Planner)
- Trois options de voyage : suite **Légère** (5 Or), **Ordinaire** (25 Or, −5 % vitesse) et **Solennelle** (75 Or, −20 % vitesse, +diplomatie).
- **Stabilité de Résidence** : partir avec la suite ordinaire/solennelle affaiblit la capacité du siège (−2/−4) — risque de « messes bâclées » (dette) si la discipline est basse.
- **Vol du trésor** en route : déjoué si le Gardien du trésor est employé, sinon perte d'or et de prestige de chapelle. Retour automatique au statut « en résidence » à la fin du voyage.

### 6. Conflit paroissial (Situation à phases, via événements)
- Émergence pilotée par : usurpation de sacrements, fondation clandestine, faible conformité, sacrements secrets.
- Chaîne : **Protestation du curé** → **Exigence d'enquête épiscopale** → **Visite canonique** → issue :
  - **Privilège Permanent** (modificateur positif, immunise contre de futurs conflits),
  - **Amende** et mise sous observation,
  - **Interdit** : fermeture forcée (tous les bonus suspendus). Levée par décision/bouton (150 Or + 100 Piété).
- Décision **Célébrer les Sacrements à la Chapelle** : prestige et renom dynastique contre conformité et tension juridictionnelle.
- Décision **Sacrements Secrets** (activable/désactivable) : or occulte trimestriel, risque d'affaire du **Prêtre Scandaleux**.

### 7. Succession : Transition Institutionnelle
- À la mort du souverain, l'institution (stats, messes fondées, dettes +1, livres, décor, Interdit éventuel) est transférée à l'héritier principal, qui choisit :
  - **Confirmer le personnel** : stabilité (+discipline, +conformité, +prestige dynastique).
  - **Purger** : +prestige mais désorganisation liturgique (2 ans), −discipline, risque de perte d'objets.
  - **Dissoudre** : abandon des fondations (−100 prestige dynastique, malus d'opinion familiale).

### 8. Tableau de bord (fenêtre « Chapelle de Cour »)
- Ouverture via la décision **Gérer la Chapelle de Cour**.
- Jauges Prestige / Discipline / Conformité, barre **Charge / Capacité**, état de la dette (4 niveaux), compteurs et boutons Messes Fondées / Livres, état de l'institution (dédicace, statut canonique, déploiement de voyage, étape du conflit, privilège, bannière d'Interdit avec bouton de levée), et les 4 emplacements de décor cliquables (Autel, Vitraux, Tombeaux, Mobilier).

## Architecture technique

| Dossier | Contenu |
| --- | --- |
| `common/script_values/` | Calculs : capacité, charge, surcharge, phase de dette, fractions UI |
| `common/scripted_effects/` | Moteur : initialisation, pouls trimestriel, modificateurs, transfert successoral, Interdit |
| `common/on_action/` | `on_death` (succession) et `yearly_playable_pulse` (chien de garde du pouls + rivalité) |
| `common/modifiers/` | Modificateurs statiques (dédicaces, paliers de messes, phases de dette, Privilège, Interdit) |
| `common/opinion_modifiers/` | Opinions familiales (réforme des fondations, dissolution) |
| `common/decisions/` | Fondation, gestion, livres, sacrements, Interdit |
| `common/scripted_guis/` | Logique des boutons du tableau de bord |
| `common/court_positions/` | Les 4 charges de la chapelle |
| `common/travel/travel_options/` | Les 3 niveaux de suite itinérante |
| `events/` | Fondation, pouls + dette, conflit paroissial, succession, cour (rivalité, livres) |
| `gui/` | Fenêtre `window_court_chapel` (enregistrée via `scripted_widgets`) |
| `localization/` | Français et anglais |

Le cœur temporel est un **événement caché auto-reprogrammé tous les 3 mois** (`capellania_pulse.0001`), protégé par un chien de garde annuel (`yearly_playable_pulse`) qui le relance s'il meurt (le drapeau `chapel_pulse_alive` expire après 200 jours).

## Test rapide en jeu
1. Lancer une partie avec un personnage possédant des terres.
2. Décision « Fonder une Chapelle de Cour » → suivre les 3 événements.
3. Décision « Gérer la Chapelle de Cour » pour ouvrir le tableau de bord.
4. Fonder 2-3 messes sans personnel pour déclencher la surcharge et observer la dette monter (3 mois par tick).
5. Employer le Premier chapelain / commander des livres pour résorber la dette.
6. Décision « Célébrer les Sacrements » puis attendre : le conflit paroissial émergera.
7. `debug_mode` : `effect trigger_event = capellania_conflict.0001` pour forcer la chaîne de conflit.

## Limitations connues / pistes v3
- La scène 3D Royal Court est simulée par la scène 2D interactive (une vraie scène 3D nécessite des assets propriétaires).
- Les artéfacts utilisent des variables plutôt que le système d'artéfacts natif (migration possible en v3).
- L'IA ne fonde pas de chapelle (`ai_check_interval = 0`) — système orienté joueur pour l'instant.
- Le conflit paroissial est un système à phases par événements, pas une « Situation » native (plus robuste entre versions du jeu).
