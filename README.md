# Séquence détaillée – Handball 6e/5e

**Thème :** « Vitesse ou précipitation ? »
**Intention centrale :** rendre explicite le **rendement** entre **conservation du ballon**, **progression vers la zone de marque**, et **tir en bonne disposition**, **sous contrainte temporelle**.
**Format d’évaluation/entraînement pivot :** **3 attaquants** vs **1 défenseur en zone avant** + **1 défenseur en zone arrière** + **1 gardien** (GB).
**Recueil des données :** assuré par une **application dédiée** (Voir plus bas), permettant un **feedback visuel immédiat** et des **comparaisons multi-passages**.

Tester l'application en ligne:
https://www.webjeje.com/online/webapp/traam/
---

## 1) Ancrage programmes & cap de formation

* **Champ d’apprentissage CA4** (conduire et maîtriser un affrontement collectif) – cycles 3/4, public **6e-5e**.
* **Compétences visées**

  * **Motrices :** se démarquer, enchaîner passes/courses/tirs en respectant les règles (3 pas/3 s), occuper les couloirs, lire les surnombres.
  * **Méthodologiques et sociales :** tenir des rôles (joueur, défenseur, GB, **observateur-chronométreur**), **co-analyser** des données, débattre de l’efficacité d’une stratégie.
  * **Culturelles :** comprendre que « aller vite » n’est **efficace** que si la **prise d’information** et la **qualité de la disposition de tir** sont réunies (angle, élan, distance).
* **Finalité :** doter les élèves d’**outils d’analyse** pour décentrer la croyance « plus vite = plus efficace », et **objectiver** leurs progrès par des **mesures produites en classe**.

---

## 2) Dispositif de jeu et d’évaluation 

**Terrain** : ½ terrain de handball (en largeur si besoin), matérialiser **zone arrière** (> 9 m) et **zone avant** (de 9 m à 6 m).
**Rôles** : 3 attaquants // 1 déf en **zone arrière** // 1 déf en **zone avant** // 1 GB // 1 **observateur**. Rotation toutes les 2-3 actions.
**Règles spécifiques**

1. Les défenseurs **restent** dans leur zone (ils défendent la profondeur/interceptions sans sortir de leur bande).
2. Départ d’action sur **passe de l’enseignant** (ou remise neutre).
3. **Fin d’action** = **but**, **tir cadré** (arrêt GB), **tir manqué**, **perte de balle** (interception, faute, sortie).
4. **Contrainte temporelle** : le chrono **démarre** à la **première prise de balle attaquante**, s’**arrête** à la fin d’action.

**Indicateurs à relever par l’observateur (via l’appli)**

* **Temps d’action** (sec).
* **Nombre de passes** (compte exact).
* **Zone de fin** : **Avant** / **Arrière**.
* **Issue** : **Perte** / **Tir manqué** / **Tir cadré** / **But**.

**Retour visuel attendu (depuis l’appli)**

* Un **chrono** lisible.
* Une **pastille issue** (rouge = perte ; orange = tir manqué ; vert = tir cadré ; vert foncé = but).
* Un **repère de zone atteinte** (icône Avant/Arrière).
* Un **historique par élève** (3 à 5 passages minimum) pour comparer **temps**, **passes**, **issue/zone**.

**Sécurité et cadre**

* Rappels 3 pas/3 s ; interdits de charge ; tirs maîtrisés ; rotations fluides ; gestion des rebonds avec GB.

---
Voici l’idée en clair : **classer une action** avec **3 facteurs simultanés** (temps, passes, zone/issue) est **naturellement complexe** : en 2D déjà (p. ex. temps × passes) la frontière entre « bon » et « pas bon » n’est pas une ligne droite unique ; en **3D** (temps × passes × zone) c’est un **volume** aux bords irréguliers.
Pour éviter que ça devienne illisible pour les élèves, on **impose une hiérarchie de décision** (règle de priorité) qui tranche proprement.

# 3) Pourquoi c’est compliqué sans le numérique....

* **Multicritère** : une action peut être rapide **mais** mal finie, ou bien préparée **mais** trop longue. Sans règle claire, on ne sait pas quoi privilégier.
* **Frontières non linéaires** : “2–4 passes utiles” n’est pas « mieux » si le tir est mauvais ; “< 8 s” n’est pas « mieux » si la balle est perdue.
* **3D = volumique** : temps (axe X), passes (axe Y), zone/issue (axe Z). Visualiser/pondérer ces trois dimensions sans priorité crée des cas ambigus.

# La règle de priorité (simple et pédagogique)

On **priorise le sens du jeu** : d’abord la **fin** (qualité du tir et **zone avant**), puis le **tempo**, puis la **quantité de passes**.

Pseudo-code (celui de l’app) :

```js
// Priorité : FIN (zone/issue) → TEMPO → PASSES
if (isSuccess && isAvant && sec <= T_vite && passes ∈ [P_min, P_max]) return 4; // vite efficace
if (isSuccess && isAvant && sec > T_vite && sec <= T_lent)           return 3; // lent efficace
if (issue==='passage_arriere' || sec > T_lent || passes >= P_sterile) return 2; // lent pas efficace
if (passes <= 1 || issue==='perte_avant')                              return 1; // panique
return 2; // filet de sécurité
```

**Traduction** :

1. **La fin domine** : un **tir cadré/but en zone avant** qualifie l’action (N3 ou N4 selon le temps).
2. **Le tempo tranche** : si c’est réussi **et** en zone avant, **≤ T\_vite → N4**, **(T\_vite, T\_lent] → N3**.
3. **Les passes arbitrent** la qualité de la construction : trop peu (≤1) → **panique (N1)** ; trop (≥ P\_stérile) → **stérile (N2)**.
4. **Filets** : passage bloqué en **zone arrière** ou **au-delà de T\_lent** → **N2**.

# Ce que gagne l’élève (et toi)

* **Lisibilité** : on sait **tout de suite** quoi améliorer :

  * fin mauvaise → travailler **angle/élan/zone** ;
  * fin bonne mais lente → **lecture + enchaînement** ;
  * trop de passes → **intention d’attaque** ;
  * trop peu de passes → **conserver/regarder**.
* **Équité** : une action “rapide mais bâclée” n’est **pas** valorisée ; une action “préparée et cadrée” est **valorisée** même si elle n’est pas ultra-rapide.

# Exemples flash (avec réglages par défaut)

* **N4** : 7,2 s · 3 passes · **tir cadré** **en zone avant** → vite **et** bien.
* **N3** : 12,5 s · 4 passes · **but** **en zone avant** → bien, un peu long.
* **N2** : 19 s · 5 passes · **zone arrière** ou **tir manqué** → lent/stérile.
* **N1** : 5 s · **1 passe** · perte **en zone avant** → précipitation.

**Synthèse** : on remplace un **normogramme 3D** (complexe) par une **suite de tests ordonnés** qui reflète la **logique du jeu** : *je finis bien → je regarde le temps → je contrôle les passes*.


## 4) Référentiel à 4 niveaux (seuils indicatifs à **calibrer séance 1**)

> Ajuster légèrement les seuils après la première séance selon votre salle, la taille du terrain et le niveau.

| Niveau                   | Profil de jeu                      | **Temps**  | **Passes** | **Fin d’action / Zone**                             | Lecture pédagogique                        |
| ------------------------ | ---------------------------------- | ---------- | ---------- | --------------------------------------------------- | ------------------------------------------ |
| **1. Panique**           | précipitation, faible prise d’info | **≤ 8 s**  | **0–1**    | Perte, tir manqué, ou tir pris **depuis l’arrière** | Aller vite **sans sens**                   |
| **2. Lent pas efficace** | conservation stérile               | **> 18 s** | **≥ 6**    | Pas de tir dangereux, fin **arrière** ou perte      | Rythme lent **mais** stérile               |
| **3. Lent efficace**     | préparation pertinente             | **9–18 s** | **2–5**    | **Tir cadré** **en zone avant** ou **but**          | Tempo construit                            |
| **4. Vite efficace**     | vitesse maîtrisée                  | **≤ 8 s**  | **2–4**    | **Tir cadré** **en zone avant** ou **but**          | Exploite le surnombre **vite** et **bien** |

**Repères d’adaptation**

* **6e** : élargir « lent efficace » jusqu’à **20 s** ; accepter un tir cadré à **9–10 m** comme « avant » si l’angle est ouvert.
* **5e** : resserrer « vite efficace » à **≤ 7 s** ; exiger **déplacement/élan** avant tir.
A tester...
---
 valeurs par défaut :
**T\_vite = 8 s · T\_lent = 18 s · P\_min = 2 · P\_max = 4 · P\_stérile = 6**

---

# Où régler ?

Footer → **Réglages** (PIN **2025**)
Les 5 champs modifiables :

* **Temps vite (T\_vite)**
* **Temps lent (T\_lent)**
* **Passes min (P\_min)**
* **Passes max (P\_max)**
* **Seuil stérile (P\_stérile)**

Les changements sont **pérennes** (localStorage) et s’appliquent dès le prochain passage.

---

# Ce que décident les règles (rappel synthèse)

* **N4 — vite efficace** : `t ≤ T_vite` **et** `passes ∈ [P_min, P_max]` **et** `issue ∈ {tir_cadré, but}` **et** `zone = avant`.
* **N3 — lent efficace** : `T_vite < t ≤ T_lent` **et** `passes ∈ [P_min, P_max]` **et** `issue ∈ {tir_cadré, but}` **et** `zone = avant`.
* **N2 — lent pas efficace** : `t > T_lent` **ou** `passes ≥ P_stérile` **ou** `issue ∈ {tir_manqué, zone_arriere}`.
* **N1 — panique** : `passes ≤ 1` **ou** `issue = perte_avant` **ou** `zone_avant` (sans tir).

> Priorité de calcul : **fin (issue/zone)** → **tempo** → **passes**.

---

# Incidence des 5 réglages (effets attendus + quand bouger le curseur)

## 1) **Temps vite (T\_vite)**

* **Baisser** (ex. 7 s) : N4 devient **plus exigeant** → pousse à **lire vite** l’avantage (2x1) et à enchaîner.
* **Monter** (ex. 9 s) : N4 devient **plus accessible** (utile en 6e) → réduit la “pression” sans dénaturer l’objectif.

**À utiliser quand** :

* Terrain long/défense serrée → **monte** T\_vite.
* Terrain court/défense permissive → **baisse** T\_vite.

## 2) **Temps lent (T\_lent)**

* **Baisser** (ex. 16 s) : sanctionne plus tôt la **stérilité** (N2), évite les rondes de passes.
* **Monter** (ex. 20 s) : **laisse le temps** de construire (N3) quand la technique est fragile.

**À utiliser quand** :

* Classe qui “tricote” → **baisse** T\_lent.
* Classe en difficulté technique → **monte** T\_lent.

## 3) **Passes min (P\_min)**

* **Baisser** (ex. 1) : autorise des sorties **très directes**.
* **Monter** (ex. 3) : **force la conservation** et la **prise d’info** avant d’accélérer.

**À utiliser quand** :

* Beaucoup de pertes en 1re passe → **monte** P\_min (2→3).
* Très bonne lecture du jeu → **baisse** P\_min (2→1) pour valoriser la spontanéité.

## 4) **Passes max (P\_max)**

* **Baisser** (ex. 3) : cible un jeu **plus tranchant** (évite les passes “pour passer”).
* **Monter** (ex. 5) : valorise une **préparation plus longue** (renversement, décalage).

**À utiliser quand** :

* Tirs pris sans angle → **monte** P\_max (4→5) pour encourager la préparation.
* Trop de latéralités inutiles → **baisse** P\_max (4→3).

## 5) **Seuil stérile (P\_stérile)**

* **Baisser** (ex. 5) : classe plus vite le jeu en **N2 (stérile)** si ça tourne sans avancer.
* **Monter** (ex. 7–8) : **tolère** plus de passes avant sanction (utile en 6e).

**À utiliser quand** :

* On n’attaque pas la profondeur → **baisse** P\_stérile.
* Manip techniques en construction → **monte** P\_stérile.

---

# Exemples concrets (avec les **valeurs par défaut**)

## N4 — Vite efficace

* **7,2 s – 3 passes – but – zone avant** → **N4** (vite + passes utiles + fin excellente)
* **8,0 s – 2 passes – tir cadré – zone avant** → **N4** (pile sur T\_vite)
* **6,5 s – 4 passes – tir cadré – zone avant** → **N4** (max passes utiles atteint)

**Si tu montes T\_vite à 9 s** : l’action **8,6 s – 3 passes – tir cadré** passerait de **N3 → N4** (plus de N4 attendus).
**Si tu baisses P\_max à 3** : l’action **6,5 s – 4 passes – tir cadré** deviendrait **N2** (trop de passes au regard de la cible).

---

## N3 — Lent efficace

* **12,4 s – 3 passes – but – zone avant** → **N3** (préparation > T\_vite mais tir de qualité)
* **17,9 s – 2 passes – tir cadré – zone avant** → **N3** (à la limite de T\_lent)
* **10,2 s – 4 passes – tir cadré – zone avant** → **N3** (tempo modéré, passes utiles)

**Si tu baisses T\_lent à 16 s** : la seconde action (**17,9 s…**) deviendrait **N2** (trop long).
**Si tu montes P\_max à 5** : une action **13 s – 5 passes – tir cadré** resterait **N3** (préparation acceptée).

---

## N2 — Lent pas efficace

* **19,5 s – 4 passes – tir cadré – zone avant** → **N2** (trop long > T\_lent)
* **13,0 s – 7 passes – tir cadré – zone avant** → **N2** (≥ **P\_stérile**)
* **10,0 s – 3 passes – tir manqué – zone avant** → **N2** (fin de mauvaise qualité)
* **14,0 s – 3 passes – zone arrière** → **N2** (on n’a pas vraiment progressé)

**Si tu montes T\_lent à 20 s** : la 1re action (**19,5 s…**) passerait **N3** (préparation longue mais valide).
**Si tu baisses P\_stérile à 5** : la 2e action (**7 passes…**) resterait **N2**, et des **6 passes** basculeraient aussi **N2** (durcissement).

---

## N1 — Panique

* **5,0 s – 1 passe – tir cadré – zone avant** → **N1** (passes ≤ 1)
* **4,0 s – 0 passe – perte – zone avant** → **N1** (perte)
* **9,0 s – 1 passe – zone avant (sans tir)** → **N1** (on y est, mais on **gâche**)

**Si tu **baisses P\_min** à 1** : la 1re action (**1 passe – tir cadré**) ne serait **plus** N1 par “manque de passes”, mais resterait **N2** si tu veux garder l’exigence de **tir cadré** *et* **tempo**.

---

# Petits presets utiles (à charger en début de cycle)

* **6e (sécuriser la construction)** : `T_vite = 9 s · T_lent = 20 s · P_min = 2 · P_max = 5 · P_stérile = 7`
  → Moins de N1, N3 plus accessible, N4 possible mais méritant.
* **5e (tranchant & lecture rapide)** : `T_vite = 7 s · T_lent = 18 s · P_min = 2 · P_max = 4 · P_stérile = 6`
  → N4 exigeant, N2 sanctionne les séquences stériles.

---

# Message à faire passer aux élèves

* **Rouge (N1)** : je me précipite (peu/pas de passes, perte, pas de tir).
* **Orange (N2)** : je garde sans attaquer / tir peu dangereux.
* **Vert clair (N3)** : je **prépare** et je **cadre** en zone avant.
* **Vert foncé (N4)** : je **lis vite**, passes **utiles**, tir **cadré** ou **but** en zone avant.



## En quoi la séquence rend l’apprentissage **explicite**

* Les **trois dimensions** (conservation, progression, marque en bonne disposition) sont **mesurées** par **temps**, **passes**, **zone/issue**.
* Les élèves **produisent**, **lisent** et **discutent** leurs propres **données**, ce qui **déconstruit** la croyance « plus vite = mieux » et installe l’idée de **vitesse maîtrisée**.
* La **contrainte temporelle** n’est pas punitive ; c’est un **repère** pour **qualifier le tempo** (vite **ou** lent) **et** l’**efficacité**.



---
---
### Cahier des charges Application.  
---
---



1. Installer le **3v1+1+GB**, les **rôles** et la **prise de données**.
2. **Calibrer** les **seuils** (séance 1), puis travailler **Conserver → Progresser → Marquer**.
3. Exploiter l’appli pour **comparer** des **passages**, **décider** d’une **fenêtre-tempo**, **débat** sur l’efficacité.
4. **Positionner** N1–N4, **valoriser** le **progrès**, fixer une **règle personnelle d’action**.

Parfait. Voici un **cahier des charges UX + logique métier** mobile-first, centré élèves, qui colle exactement à tes contraintes. Tout est prêt pour coder l’app ensuite.

# Vision d’ensemble (mobile-first)

* **Écran unique “Passage”** pour les élèves : **Start → compter les passes → finir l’action** en tapant **un seul** bouton parmi 6 issues.
* **Gros contrôles**, **couleurs franches**, **retour en temps réel** (chrono, passes, “niveau” visualisé façon **jauge de héros**).
* **Réarmement automatique** à chaque fin d’action (reset chrono + passes + état).
* **Navigation** (footer) : `Passage` · `Stats` · `Réglages` (protégé PIN 2025).
* **Stockage local** pour les seuils et les données ; **export CSV/JSON**.

---

# Parcours élève (écran Passage)

## Zone HUD (toujours visible)

* **Chrono** (giga-typo, centré, mm\:ss.d)
* **Compteur de passes** : un **gros bouton** “Passes +1” avec le **nombre affiché dedans** (grossissement animé à chaque +1).
* **Jauges “héros”** (3 mini jauges horizontales, toujours à jour) :

  1. **Temps** : barre qui se remplit, tick d’alerte à T\_vif (ex. 8 s) puis T\_lent (ex. 18 s).
  2. **Passes** : zones colorées (0–1 = panique ; 2–4 = utile ; ≥6 = stérile).
  3. **Zone/Issue** : prévision neutre tant que l’action n’est pas terminée, puis **verdict instantané** (couleur de l’issue).

## Contrôles

* **Start** (primair e) → démarre chrono, active `Passes +1`, **désactive** Start.
* **Fin d’action** = **tap sur un des 6 boutons** (du plus faible au meilleur, couleurs imposées) :

  1. **Noir** — *Perte zone avant*
  2. **Rouge** — *Passage zone avant*
  3. **Orange** — *Passage zone arrière (vers le but)*
  4. **Vert clair** — *Tir manqué*
  5. **Vert foncé** — *Tir cadré*
  6. **Bleu** — *But marqué*
* **Après le tap** :

  * Sauvegarde du passage (temps, passes, issue, timestamp).
  * Affichage d’un **cartouche résumé** 2 s (temps, passes, icône issue, **niveau 1-4** calculé).
  * **Réarmement auto** : chrono à 00:00.0, passes = 0, retour à Start.
* **Actions rapides** : `Annuler dernier` (undo), `Réinitialiser série`.

> Haptique & feedback : vibration courte au Start, *pulse* sur Passes +1, vibration longue + flash cadre lors du tap “fin d’action”.

---

# Logique métier (niveaux, calcul, états)

## États

* `idle` → `running` (Start) → `ended` (tap issue) → `idle` (réarmé).

## Données d’un passage

```json
{
  "id": "P-2025-09-17-10-21-05",
  "eleve": "optionnel",
  "temps_s": 7.8,
  "passes": 3,
  "issue": "but",           // "perte_avant"|"passage_avant"|"passage_arriere"|"tir_manque"|"tir_cadre"|"but"
  "ordre_issue": 6,         // 1..6 selon la liste ci-dessus (noir→bleu)
  "niveau_calcule": 4,      // 1..4
  "seance": 3,
  "horodatage": "2025-09-17T10:21:05"
}
```

## Seuils par défaut (éditables côté prof)

* **Temps** : `vite ≤ 8 s` ; `lent efficace 9–18 s` ; `lent stérile > 18 s`.
* **Passes utiles** : `2–4` (ok), `0–1` (panique), `≥6` (stérile).
* **Zone/issue** (du - au +) : noir < rouge < orange < vert clair < vert foncé < bleu.

## Règles de niveau (par défaut, éditables)

1. **N4 “Vite efficace”** si `temps ≤ T_vite` **et** `passes in [P_min,P_max]` **et** `issue ∈ {tir_cadre, but}` **et** `zone attendue = avant` (déduite de l’issue ou du bouton).
2. **N3 “Lent efficace”** si `temps in [T_vite+ε, T_lent]` **et** `passes in [P_min-ε,P_max+ε]` **et** `issue ∈ {tir_cadre, but}` **et** `zone = avant`.
3. **N2 “Lent pas efficace”** si `temps > T_lent` **ou** `passes ≥ P_sterile` **ou** `issue ∈ {tir_manque, passage_arriere}`.
4. **N1 “Panique”** si `passes ≤ 1` **ou** `issue ∈ {perte_avant, passage_avant sans tir}` **ou** `tir très précoce` (< 3 s, option).

> L’algorithme choisi privilégie d’abord la **qualité de la fin** (zone/issue), puis **tempo**, puis **passes**. Tout est **paramétrable** par le prof.

---

# Stats & retours (élève et cumul)

## Après chaque passage

* Cartouche : **Temps | Passes | Issue | Niveau** avec couleur dominante du niveau (N1 gris, N2 orange, N3 vert, N4 bleu/émeraude).
* Ajout dans la **timeline** (liste des passages) avec mini-icône issue.

## Vue Stats (footer)

* **Panel gauche “Zones/Issues”** : histogramme empilé des 6 issues (taux + cumul).
* **Panel droit “Passes & Temps”** : courbes superposées par passage (+ **moyennes** et **médianes**).
* **Badges de synthèse** : `Temps moyen`, `Passes moyennes`, `% zone avant`, `% tirs cadrés`, `% buts`, `% pertes`.
* **Filtres** : par séance, par élève, par tranche de temps.
* **Export** : CSV (UTF-8 BOM) et JSON.

---

# Réglages (protégé PIN 2025)

* **Saisie PIN** sur numpad : 2025 → déverrouille 5 min (sessionStorage).
* **Seuils** : sliders/inputs pour `T_vite`, `T_lent`, `P_min`, `P_max`, `P_sterile`.
* **Mapping issues → zones** (si besoin d’ajuster la logique pédagogique).
* **Niveaux** : éditeur de règles (priorités, conditions).
* **Configs** : `Enregistrer`, `Charger`, `Par défaut` (localStorage, multi-prof possible via un label).
* **Données** : `Export`, `Purger`, `Importer` (JSON).

---

# Design système (tokens & accessibilité)

* Couleurs (WCAG AA contrastées sur fond sombre) :

  * **Noir** `#111827`, **Rouge** `#ef4444`, **Orange** `#f59e0b`, **Vert clair** `#86efac`, **Vert foncé** `#22c55e`, **Bleu** `#3b82f6`.
* Typo **très lisible**, tailles XXL pour Start/Chrono/Passes.
* Touch targets ≥ 56 px, **haptique** activée.
* **Offline-first** possible (PWA + cache), mais non indispensable pour V1.
* **Mode daltonisme** optionnel : pictos + textures de boutons.

---

# Structure technique proposée

* **HTML/CSS/JS vanille** (Bootstrap/Tailwind si tu préfères) ; **Chart.js** pour les graphs.
* **Modules** :

  1. `Timer` (rafraîchissement 60 Hz, affichage 10 Hz).
  2. `PassCounter` (+1 avec animation).
  3. `OutcomeBar` (6 boutons, état actif/désactif, vibrations).
  4. `Gauges` (temps/passes/issue en direct).
  5. `LevelEngine` (calcul N1–N4 selon réglages).
  6. `Store` (localStorage + export).
  7. `StatsView` (graphs, moyennes, filtres).
  8. `Settings` (PIN, seuils, profils).

---

# Schémas de données (stockage local)

```json
{
  "config_active": "prof-default",
  "configs": {
    "prof-default": {
      "pin": "2025",
      "seuils": {
        "T_vite": 8.0,
        "T_lent": 18.0,
        "P_min": 2,
        "P_max": 4,
        "P_sterile": 6
      },
      "priorites": ["issue_zone", "tempo", "passes"]
    }
  },
  "passages": [
    { "id":"...", "eleve":"", "seance":1, "temps_s":7.2, "passes":3, "issue":"but", "ordre_issue":6, "niveau_calcule":4, "horodatage":"..." }
  ]
}
```

---

# Plan d’évaluation (ajout des jauges “héros”)

* **Jauge Temps** : verte jusqu’à T\_vite, jaune entre T\_vite et T\_lent, rouge au-delà.
* **Jauge Passes** : rouge 0–1, **vert** 2–4, **orange** ≥6.
* **Jauge Zone/Issue** : gradient noir→bleu selon le bouton tapé.
* **Positionnement auto N1–N4** à chaque passage + **synthèse multi-passages** (médiane de niveau, progression).

---

# Micro-détails UX qui font la différence

* **Empêcher les erreurs** : les 6 issues sont **désactivées** tant que Start n’a pas été tapé.
* **Tap long** sur Passes +1 → **-1** (correction rapide).
* **Undo** visible 10 s après chaque fin d’action.
* **Toast** de sécurité si l’élève tente de finir l’action avec “0 passe” (on accepte, mais on marque “panique” par défaut).
* **Alerte sonore douce** à 8 s et 18 s (désactivable).





