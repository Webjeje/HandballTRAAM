# Handball · Vitesse vs Précipitation — README technique

Webapp pédagogique **mobile-first** pour collecter des données en handball (temps, passes, issue) et objectiver la relation **vitesse / précipitation**.
Aucune dépendance serveur : tout fonctionne **côté client** (HTML/CSS/JS) avec **localStorage** + **exports** (CSV/QR).

* 🌐 Démo en ligne : [https://www.webjeje.com/online/webapp/traam/](https://www.webjeje.com/online/webapp/traam/)
* ▶️ Vidéo : [https://www.webjeje.com/online/webapp/traam/n3.mp4](https://www.webjeje.com/online/webapp/traam/n3.mp4)

---

## Sommaire

1. [Architecture & pile technique](#architecture--pile-technique)
2. [Structure des fichiers / modules](#structure-des-fichiers--modules)
3. [Modèle de données](#modèle-de-données)
4. [Algorithme de niveau (N1→N4)](#algorithme-de-niveau-n1n4)
5. [Seuils & configuration (PIN)](#seuils--configuration-pin)
6. [Stockage local (clés & migration)](#stockage-local-clés--migration)
7. [Exports (CSV, QR)](#exports-csv-qr)
8. [UI & accessibilité](#ui--accessibilité)
9. [Graphiques & visualisation](#graphiques--visualisation)
10. [Simulateur visuel (Réglages)](#simulateur-visuel-réglages)
11. [Machine d’états & événements](#machine-détats--événements)
12. [Performance & compatibilité](#performance--compatibilité)
13. [Sécurité & confidentialité](#sécurité--confidentialité)
14. [Build / déploiement](#build--déploiement)
15. [Tests & validation](#tests--validation)
16. [Roadmap & extensions](#roadmap--extensions)
17. [Licence](#licence)

---

## Architecture & pile technique

* **Frontend pur** : `index.html` unique (aucun framework requis).
* **CSS** : variables CSS (thème sombre), layout responsive (grid/flex).
* **JavaScript** : vanilla ES6, `localStorage`, `sessionStorage`, `performance.now()`, `requestAnimationFrame`.
* **Libs externes** (CDN) :

  * **Chart.js 4.x** : lignes & barres pour Stats.
  * **QRious 4.x** : génération de QR code “résumé”.
  * **Gauge.js** : historiquement présent, **non utilisé** dans le module Stats (la jauge a été remplacée par la valeur moyenne + image). La lib peut être retirée si souhaité.
* **Aucune API externe**.
* **Mobile-first** : gros boutons, couleurs signifiantes, haptique (`navigator.vibrate`).

---

## Structure des fichiers / modules

Exemple conseillé de repo :

```
/ (racine du dépôt)
├── index.html              # Webapp complète (UI + logique)
├── /assets
│   ├── n1.png              # Visuel niveau N1
│   ├── n2.png              # Visuel niveau N2
│   ├── n3.png              # Visuel niveau N3
│   └── n4.png              # Visuel niveau N4
└── README.md               # Ce document
```

### Sections de l’UI

* **Passage** : chrono, +passes, 6 issues (terminaison de l’action).
* **Stats** : KPIs, niveau moyen (valeur + descriptif + image), courbes temps/passes, histogramme issues, exports.
* **Réglages** (PIN `2025`) : 5 seuils, **simulateur visuel** (carte des zones N1..N4).
* **Élèves** : 3 noms (facultatif) + exports.

---

## Modèle de données

### Énumérations / codes

```txt
issue ∈ {
  'perte_avant',        // rouge
  'passage_avant',      // orange
  'passage_arriere',    // orange (retour)
  'tir_manque',         // vert clair
  'tir_cadre',          // vert clair
  'but'                 // vert foncé
}

niveau_calcule ∈ {1,2,3,4} // N1..N4
```

### Enregistrement d’un passage (objet)

```json
{
  "id": "P-<timestamp_ms>",
  "eleve": "Nom1|Nom2|Nom3",
  "temps_s": 7.4,
  "passes": 3,
  "issue": "but",
  "ordre_issue": 6,          // index 1..6 dans l’ordre pédagogique croissant
  "niveau_calcule": 4,       // N1..N4
  "seance": 1,
  "horodatage": "2025-09-20T11:22:33.123Z"
}
```

### Schéma configuration (seuils)

```json
{
  "seuils": {
    "T_vite": 8.0,   // s
    "T_lent": 18.0,  // s
    "P_min": 2,      // passes utiles min
    "P_max": 4,      // passes utiles max
    "P_sterile": 6   // à partir de ce nombre : "stérile"
  }
}
```

---

## Algorithme de niveau (N1→N4)

**Priorité** : `issue (fin)` → `temps` → `passes`.

```js
function computeLevel(sec, passes, issue){
  const {T_vite,T_lent,P_min,P_max,P_sterile} = cfg.seuils;
  const isAvant   = (issue !== 'passage_arriere');           // zone avant/fin constructive
  const isSuccess = (issue === 'tir_cadre' || issue === 'but');

  if (isSuccess && isAvant && sec <= T_vite && passes >= P_min && passes <= P_max) return 4; // N4 "vite efficace"
  if (isSuccess && isAvant && sec >  T_vite && sec <= T_lent)                     return 3; // N3 "lent efficace"
  if (issue === 'passage_arriere' || sec > T_lent || passes >= P_sterile)         return 2; // N2 "lent pas efficace"
  if (passes <= 1 || issue === 'perte_avant')                                     return 1; // N1 "panique"
  return 2; // repli conservateur
}
```

> `predictLevel(sec, passes)` (utilisé pour colorer le bouton **+Passes**) exploite les mêmes seuils (sans l’issue).

---

## Seuils & configuration (PIN)

* Édition via l’onglet **Réglages** (protégé par **PIN = 2025**).
* 5 **seuils** :

  * `T_vite (s)`, `T_lent (s)`
  * `P_min`, `P_max` (passes “utiles”)
  * `P_sterile` (≥)
* Persistance dans `localStorage` (voir [Stockage local](#stockage-local-clés--migration)).
* **Simulateur** : effets visuels d’un **bonus/malus** selon l’issue (ne change pas la logique d’enregistrement).

---

## Stockage local (clés & migration)

Clés utilisées dans `localStorage` / `sessionStorage` :

| Clé              | Type           | Contenu                                                         |
| ---------------- | -------------- | --------------------------------------------------------------- |
| `hb_cfg_v1`      | localStorage   | JSON config `{ seuils: {T_vite,T_lent,P_min,P_max,P_sterile} }` |
| `hb_passages_v1` | localStorage   | `Array<Passage>` (voir modèle)                                  |
| `hb_names_v1`    | localStorage   | `["Nom1","Nom2","Nom3"]`                                        |
| `hb_unlocked`    | sessionStorage | `"1"` si le panneau Réglages est déverrouillé                   |

### Réinitialisation

* **Tout** (seuils + données + noms)
* **Données** uniquement
* Géré via un **modal** (bouton **Reset** en header).

### Migration

* Versionnage simple via suffixe `_v1`.
* En cas d’évolution de schéma, créer `*_v2`, lire `v1` si présent, convertir, puis supprimer `v1`.

---

## Exports (CSV, QR)

### CSV (Excel/Numbers)

* Encodage **UTF-8** avec **BOM**.
* Nom de fichier : `handball_vitesse_stats.csv`.
* En-têtes :

  ```
  date,attaquant1,attaquant2,attaquant3,temps_s,passes,issue,niveau
  ```
* Exemple :

  ```
  2025-09-20T11:22:33.123Z,Ali,Ben,Chloé,7.4,3,but,4
  ```

### QR code

* Généré avec **QRious**.
* Contenu : **résumé textuel** (pas de données nominatives détaillées), incluant :

  * total actions, temps moyen, passes moyennes, nb buts, **niveau moyen**.

---

## UI & accessibilité

* **Mobile-first**, **gros boutons**, contraste fort.
* Couleurs pédagogiques (variables CSS) :

  * Niveaux : `--n1` rouge · `--n2` orange · `--n3` vert clair · `--n4` vert foncé
  * Issues : `--issue-rouge` / `--issue-orange` / `--issue-vert-clair` / `--issue-vert-fonce`
* **Vibrations** (`navigator.vibrate`) : retour haptique sur Start/Passes.
* **Clavier / Focus** : boutons natifs HTML, focus outline (CSS minimal).
* **Langue** : `lang="fr"`.

---

## Graphiques & visualisation

* **Chart.js 4.x** :

  * **Temps (s)** : **line** rouge.
  * **Passes (#)** : **line** verte.
  * **Issues** : **bar** (couleurs = boutons du passage).
* **KPIs** :

  * Temps moyen (s), Passes moyennes (#), `% buts` (rapport buts / total passages).
* **Niveau moyen** :

  * Affiché **au dixième** (ex. `3,2`), avec **descriptif** :

    * `≥ 3.8` → **N4 · Vite efficace**
    * `≥ 3.0` → **N3 · Lent efficace**
    * `≥ 2.0` → **N2 · Lent pas efficace**
    * sinon → **N1 · Panique**
  * Image correspondante : `assets/n1.png` … `assets/n4.png`.

---

## Simulateur visuel (Réglages)

* **But** : comprendre l’incidence conjointe **temps / passes / issue** sur les zones **N1..N4**.
* **Préréglages** : `facile`, `6e`, `5e` (renseignent les 5 champs).
* **Décalage (s)** : offset visuel appliqué à `T_vite/T_lent` selon l’issue :

  ```js
  issueWeights = { but:-1.5, tir_cadre:-1.0, tir_manque:-0.5, passage_avant:0, passage_arriere:1.0, perte_avant:1.5 }
  effectiveTVite = T_vite - (decalage * issueWeights[selectedIssue])
  effectiveTLent = T_lent - (decalage * issueWeights[selectedIssue])
  ```
* **Important** : simulateur **n’affecte pas** `computeLevel()` (purement démonstratif).

---

## Machine d’états & événements

### États principaux

* `idle` → `running` → `idle` (après issue).
* Transitions :

  * `Start` : `idle → running`
  * `+Passes` : incrément si `running`
  * `Issue` (1 des 6 boutons) : `running → idle` + enregistrement + bilan (modal)

### Événements clés

* `requestAnimationFrame(tick)` : MAJ chrono + jauges + couleur du bouton **+Passes** (via `predictLevel`).
* `endPassage(issueKey)` :

  * calcule `temps_s`, mappe `issue`, calcule `niveau`, pousse l’objet dans `passages`.
  * rafraîchit les Stats si l’onglet est ouvert.

---

## Performance & compatibilité

* **Chrono** : `performance.now()` + `requestAnimationFrame` (précision ms).
* **Destruction / recréation** des charts à chaque rendu (datasets simples, volumes faibles).
* **Compatibilité** : Chrome/Edge/Firefox/Safari récents (mobile & desktop).
* **Poids** : dépend des 2 libs (Chart.js, QRious) chargées via CDN.

---

## Sécurité & confidentialité

* **Aucune donnée serveur** : tout en local.
* **PIN** : simple barrière UX (`2025`) — ne protège pas contre un utilisateur malveillant ayant accès au poste.
* **Noms élèves** : champs libres, stockés localement (`hb_names_v1`).
* **Exports** : le CSV contient les noms si renseignés.

---

## Build / déploiement

* **Local** : ouvrir `index.html` dans le navigateur.
* **GitHub Pages** : activer Pages sur la branche contenant `index.html`.
* **Netlify / Vercel** : déploiement statique (pas de build).
* **Cache** : possibilité d’ajouter un `manifest` PWA et un Service Worker (non inclus).

---

## Tests & validation

* **Unitaires** (manuel) :

  * `computeLevel()` : cas limites (exact `T_vite`, `T_lent`, `P_min`, `P_max`, `P_sterile`).
  * `predictLevel()` : cohérence couleur bouton **+Passes**.
  * **Exports** : encodage CSV (UTF-8 + BOM), ouverture Excel/Numbers.
* **E2E** (manuel) :

  * Cycle complet **Start → Passes → Issue** × N (statistiques et KPIs).
  * **Reset** (données seules vs tout).
  * **PIN** (mauvais code / bon code).
* **Accessibilité** :

  * Contrastes couleurs (N1..N4, issues).
  * Navigation tactile/clavier.

---

## Roadmap & extensions

* Filtrage **par élève** et par **séance**.
* **JSON** d’export/import.
* **PWA** (offline complet + icône).
* **Commentaires** qualitatifs (auto-évaluation).
* Courbes **médianes/quartiles** et densités de frappes.
* **Tests automatiques** (Playwright) pour le cycle de passage.

---

## Licence

Usage pédagogique libre. Merci de **citer la source** et de partager vos retours/PR pour améliorer l’outil.
