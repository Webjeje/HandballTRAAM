# Handball · Vitesse vs Précipitation — README

Une webapp pédagogique centrée sur le **recueil de données par les élèves** en handball pour questionner l’idée « plus vite = plus efficace? ».
Elle mesure **temps**, **passes** et **issue/zone**, calcule un **niveau N1→N4**, affiche des **statistiques** et permet l’**export**.

* 🌐 Webapp en ligne : [https://www.webjeje.com/online/webapp/traam/](https://www.webjeje.com/online/webapp/traam/)
* ▶️ Démo vidéo : [https://www.webjeje.com/online/webapp/traam/n3.mp4](https://www.webjeje.com/online/webapp/traam/n3.mp4)

---

## Sommaire

1. [ Démarrage rapide](#tldr--démarrage-rapide)
2. [Fonctionnalités](#fonctionnalités)
3. [Structure de l’UI](#structure-de-lui)
4. [Mode d’emploi — Élèves](#mode-demploi--élèves)
5. [Mode d’emploi — Enseignant·e](#mode-demploi--enseignante)
6. [Seuils & algorithme de niveau](#seuils--algorithme-de-niveau)
7. [Statistiques & exports](#statistiques--exports)
8. [Simulateur visuel dans Réglages](#simulateur-visuel-dans-réglages)
9. [Données & confidentialité](#données--confidentialité)
10. [Compatibilité & accessibilité](#compatibilité--accessibilité)
11. [Dépannage](#dépannage)
12. [Déploiement (local, Pages, Netlify, Vercel)](#déploiement-local-pages-netlify-vercel)
13. [FAQ](#faq)
14. [Contribuer](#contribuer)
15. [Licence](#licence)

---

## Démarrage rapide

* Ouvrir la webapp : **[https://www.webjeje.com/online/webapp/traam/](https://www.webjeje.com/online/webapp/traam/)**
* **Passage (élèves)** :

  1. Appuyer **Start** → le **chrono** démarre.
  2. Comptabiliser les échanges avec **+ Passes**.
  3. Finir l’action en **cliquant une issue** (6 boutons, du moins au plus : Perte Z avant → … → But).
  4. Lire le **bilan** (temps, passes, zone, **Niveau N1–N4**).
* **Stats** : voir les courbes Temps/Passes, le **niveau moyen** (valeur au dixième + descriptif + image), histogramme des issues, **export CSV** et **QR**.
* **Réglages** (PIN **2025**) : éditer les 5 seuils et utiliser le **simulateur visuel**.
* **Élèves** : enregistrer **3 noms**, exporter.

> Aucun serveur requis : tout est **localStorage** + **CSV**.

---

## Fonctionnalités

* 🕒 **Chrono** + **compteur de passes** gros boutons (mobile-first, haptique).
* 🎯 **Issues** / zones (6) colorées :

  * Perte Z avant (rouge)
  * Passage Z avant (orange)
  * Passage Z arrière (orange)
  * Tir manqué (vert clair)
  * Tir cadré (vert clair)
  * But (vert foncé)
* 🧠 **Niveaux N1→N4** : Panique / Lent pas efficace / Lent efficace / Vite efficace.
* 📈 **Stats** :

  * **Niveau moyen** (ex. 3,2) + **descriptif** + **image** N1..N4 (N4 à partir de 3,8).
  * **Temps** (ligne **rouge**), **Passes** (ligne **verte**).
  * **Issues** en barres **colorées** (couleurs identiques aux boutons passage).
  * KPIs : temps moyen, passes moyennes, % buts.
  * **Export CSV** (Excel/Numbers) + **QR** récapitulatif.
* 🧪 **Simulateur visuel** (onglet Réglages) : visualise l’incidence des seuils et d’un bonus/malus d’issue (purement pédagogique).
* 🔐 **PIN** enseignant·e (2025) pour les seuils.
* 💾 **LocalStorage** pour seuils, données et noms.
* 🔁 **Reset** : données seules, ou tout (seuils+noms+données).

---

## Structure de l’UI

* **Header** : bouton **Reset** (modal : tout / données / annuler).
* **Footer** (navigation) : **Passage** · **Stats** · **Réglages** · **Élèves**.

### Passage (élèves)

* Chrono central (couleur selon tempo).
* 3 mini-jauges **Temps / Passes / Issue**.
* Bouton **Start** → remplace par **+ Passes**.
* 6 **issues** : fin de l’action, bilan + enregistrement.

### Stats

* **Niveau moyen** : valeur au dixième, **descriptif** et **image** (`assets/n1.png` … `n4.png`).
* KPIs : temps moyen, passes moyennes, % buts.
* Graphiques Chart.js :

  * **Temps** en ligne **rouge**.
  * **Passes** en ligne **verte**.
  * **Issues** en barres **colorées** (mêmes codes que les boutons).
* **Export CSV** et **QR**.

### Réglages (PIN 2025)

* 5 seuils éditables : **T\_vite**, **T\_lent**, **P\_min**, **P\_max**, **P\_sterile**.
* **Simulateur visuel** : préréglages, décalage (issue), carte des zones N1..N4.

### Élèves

* Saisie **3 noms**, boutons **Export CSV** et **QR** (identiques aux Stats).

---

## Mode d’emploi — Élèves

1. **Start** pour commencer l’action (le chrono s’affiche).
2. Compter les échanges avec **+ Passes** (chiffre très visible).
3. À la **fin de l’action**, cliquer **une issue** (les 6 boutons).
4. Lire le **bilan** (temps, passes, issue, **Niveau**).
5. L’action est **enregistrée** et le cycle se **réarme** automatiquement.

> Objectif pédagogique : confronter la croyance « plus vite = mieux » aux **données** (niveau dépend de l’issue d’abord, puis du temps, puis des passes).

---

## Mode d’emploi — Enseignant·e

* **Seuils** (onglet **Réglages**, PIN **2025**) : adapter aux classes et objectifs.
* **Simulateur** : montrer en **direct** l’effet d’un but/tir/retour arrière sur les zones d’efficacité.
* **Stats** :

  * Sensibiliser aux **compromis** vitesse/efficacité.
  * Comparer **sessions / groupes / élèves** via l’export CSV.
* **Élèves** : renseigner 3 noms (optionnel) pour l’export.

---

## Seuils & algorithme de niveau

### Les 5 réglages (défaut)

* `T_vite = 8 s`
* `T_lent = 18 s`
* `P_min = 2`
* `P_max = 4`
* `P_sterile = 6`

### Règle de priorité

1. **Fin de l’action (issue/zone)**
2. **Tempo (temps)**
3. **Passes**

### Décision (pseudo)

```js
if (isSuccess && isAvant && sec<=T_vite && passes∈[P_min,P_max])  N4; // vite efficace
else if (isSuccess && isAvant && T_vite<sec<=T_lent)              N3; // lent efficace
else if (issue==='passage_arriere' || sec>T_lent || passes>=P_sterile) N2; // lent pas efficace
else if (passes<=1 || issue==='perte_avant')                      N1; // panique
else N2;
```

* `isSuccess = (tir_cadre || but)`
* `isAvant = (≠ passage_arriere)`

> Tout est **paramétrable** côté prof pour coller au niveau de classe (6e/5e…).

---

## Statistiques & exports

* **Niveau moyen** : moyenne N1–N4 au **dixième** (ex. **3,2**).

  * Image affichée : **N4** si ≥ **3,8**, sinon N3/N2/N1 selon seuils.
* **Graphiques** :

  * **Temps (s)** : **ligne rouge**.
  * **Passes (#)** : **ligne verte**.
  * **Issues** : barres **colorées** (rouge/orange/vert clair/vert foncé).
* **KPIs** : temps moyen, passes moyennes, **% buts**.
* **Export CSV** (Excel/Numbers) :

  * Fichier `handball_vitesse_stats.csv` (UTF-8 avec BOM).
  * Colonnes : `date, attaquant1, attaquant2, attaquant3, temps_s, passes, issue, niveau`.
* **QR Code** (résumé lisible) : total actions, temps/passes moyens, buts, **niveau moyen**.

---

## Simulateur visuel dans Réglages

* **Préréglages** proposés (facile/6e/5e) qui **renseignent** les 5 champs (vous validez ensuite).
* **Décalage (s)** appliqué **visuellement** selon l’issue choisie :

  * **But** = bonus (zones “efficaces” s’agrandissent sur les temps courts),
  * **Passage arrière / Perte** = malus, etc.
* La carte affiche les **zones N1..N4** (temps en abscisse inversée, passes en ordonnée), sans impacter la logique d’enregistrement.

---

## Données & confidentialité

* **Aucune donnée serveur**. Tout est stocké **localement** (navigateur) via `localStorage`.
* Clés :

  * `hb_cfg_v1` : seuils (`T_vite`, `T_lent`, `P_min`, `P_max`, `P_sterile`)
  * `hb_passages_v1` : liste des passages (temps, passes, issue, niveau, horodatage)
  * `hb_names_v1` : noms (jusqu’à 3)
* **Export CSV** pour archivage externe.
* **Reset** possible (données seulement, ou tout).

---

## Compatibilité & accessibilité

* **Mobile-first**, gros boutons, couleurs fortement contrastées.
* Vibrations (si supportées).
* Navigateurs modernes (Chromium/Firefox/Safari récents).
* Astuce : ajouter la page à l’écran d’accueil (quasi PWA).

---

## Dépannage

* **La jauge de niveau n’apparaît plus** : c’est normal, remplacée par la **valeur moyenne**.
* **Rien ne s’enregistre** : vérifier que le navigateur **autorise** le stockage local (cookies/données locales non bloqués).
* **CSV illisible en accentuation** : ouvrir via **Importer** (UTF-8) ou Numbers/Excel récent.

---

## Déploiement (local, Pages, Netlify, Vercel)

### Local (fichier unique)

1. Télécharger le fichier HTML (et le dossier `assets/` pour les images N1..N4).
2. Ouvrir le `.html` dans un navigateur → c’est tout.

### GitHub Pages

1. Dépôt → **Settings** → **Pages** → Source `main` / `/ (root)` ou `/docs`.
2. Placer `index.html` à la racine (et `assets/`).
3. URL publique fournie par Pages.

### Netlify / Vercel

* Nouveau site → **Deploy** from Git → dossier statique (aucun build).
* Fichiers : `index.html` + `assets/`.

---

## FAQ

* **PIN ?** → `2025` (modifiable dans le code si besoin).
* **Combien d’élèves ?** → L’app mémorise 3 noms utiles pour l’export.
* **Changer les couleurs ?** → Variables CSS `--n1..--n4` et `--issue-*`.
* **Modifier la règle N1..N4 ?** → Voir `computeLevel()` (priorité issue → temps → passes).
* **Peut-on comparer plusieurs séances ?** → Exporter CSV à chaque séance et agréger côté tableur (ou dupliquer la page avec un autre `LS_KEY_*` si besoin avancé).

---

## Contribuer

* Issues / idées bienvenues (UX, accessibilité, stats avancées…).
* Suggestions possibles :

  * Export **JSON** détaillé,
  * **Filtrage** par élève,
  * **Commentaires** qualitatifs (auto-évaluation),
  * **Moyennes glissantes** / zones de dispersion,
  * **PWA** hors-ligne complet.

---

## Licence

* Usage **pédagogique** libre. Merci de citer la source et de partager vos retours pour améliorer l’outil.
