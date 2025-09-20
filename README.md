# Handball · Vitesse vs Précipitation — TRAAM (README)

Webapp pédagogique **mobile-first** pour objectiver le couple **vitesse / précipitation** en handball à partir de 3 indicateurs : **temps**, **passes**, **issue/zone**.
Le dépôt regroupe : une **introduction** au projet, un **mode d’emploi**, un **guide technique** et le **code source** de la webapp.

---

## ✨ Démo rapide

* 🌐 Webapp (en ligne) : [https://www.webjeje.com/online/webapp/traam/](https://www.webjeje.com/online/webapp/traam/)
* ▶️ Vidéo de démonstration : [https://www.webjeje.com/online/webapp/traam/n3.mp4](https://www.webjeje.com/online/webapp/traam/n3.mp4)

---

## Sommaire

1. [Objectifs du projet](#objectifs-du-projet)
2. [Contenu du dépôt (les 4 fichiers)](#contenu-du-dépôt-les-4-fichiers)
3. [Démarrage rapide](#démarrage-rapide)
4. [Fonctionnement de la webapp](#fonctionnement-de-la-webapp)
5. [Déploiement (local & statique)](#déploiement-local--statique)
6. [Contribuer](#contribuer)
7. [Licence](#licence)

---

## Objectifs du projet

* Intention centrale : rendre explicite le rendement entre conservation du ballon, progression vers la zone de marque, et tir en bonne disposition, sous contrainte temporelle.
* Mettre les **élèves** au centre du recueil de données en situation d’opposition (3v1 + 1 def arrière + GB).
* **Questionner** la croyance « plus vite = plus efficace » en croisant **temps** (temps), **passes** (occurence), **issue** (espace).
* Donner des **retours immédiats** (couleurs, niveaux N1→N4) et des **statistiques** simples (moyennes, graphes, export CSV).
* Proposer un **module enseignant** pour **paramétrer** les seuils et **visualiser** l’incidence des réglages.

---

## Contenu du dépôt (les 4 fichiers)

1. **IntroTraam.md** — *Introduction & contexte pédagogique*
   ➜ Présente la démarche TRAAM, la problématique « vitesse vs précipitation », les principes d’évaluation et de retour élève.
   Lien : [https://github.com/Webjeje/HandballTRAAM/blob/main/IntroTraam.md](https://github.com/Webjeje/HandballTRAAM/blob/main/IntroTraam.md)

2. **Mode\_Emploi.md** — *Mode d’emploi détaillé*
   ➜ Guide d’utilisation pour **élèves** et **enseignant·e** : déroulement d’un passage, interprétation des niveaux, navigation, exports.
   Lien : [https://github.com/Webjeje/HandballTRAAM/blob/main/Mode\_Emploi.md](https://github.com/Webjeje/HandballTRAAM/blob/main/Mode_Emploi.md)

3. **guide\_technique.md** — *Guide technique*
   ➜ Architecture front (HTML/CSS/JS), modèle de données, algorithme des niveaux (N1→N4), stockage local, déploiement, customisation.
   Lien : [https://github.com/Webjeje/HandballTRAAM/blob/main/guide\_technique.md](https://github.com/Webjeje/HandballTRAAM/blob/main/guide_technique.md)

4. **index.html** — *Code de la webapp (fichier unique)*
   ➜ Application complète (UI + logique) : chrono, compte-passes, issues, calcul du niveau, stats (Chart.js), exports CSV/QR, réglages (PIN 2025) et visualisation des zones.
   Lien : [https://github.com/Webjeje/HandballTRAAM/blob/main/index.html](https://github.com/Webjeje/HandballTRAAM/blob/main/index.html)

---

## Démarrage rapide

1. **Ouvrir** la page en ligne ou télécharger `index.html` et **double-cliquer** pour l’ouvrir dans un navigateur moderne.
2. Onglet **Passage** :

   * **Start** lance le chrono (vibration si supportée).
   * **+ Passes** incrémente le compteur (gros bouton, couleur prédictive).
   * **Issue** (1 des 6 boutons) termine le passage → bilan (temps, passes, issue, **Niveau**).
3. Onglet **Stats** :

   * **Niveau moyen** (valeur au dixième + descriptif + image N1..N4).
   * **Temps** (ligne rouge), **Passes** (ligne verte), **Issues** (barres colorées).
   * **Export CSV** et **QR**.
4. Onglet **Réglages** (PIN **2025**) :

   * Ajuster **T\_vite**, **T\_lent**, **P\_min**, **P\_max**, **P\_sterile**.
   * **Visualisation** des zones N1..N4 (simulateur pédagogique, n’altère pas les calculs).
5. Onglet **Élèves** :

   * Saisir **3 noms** (facultatif) utilisés dans l’export CSV.

---

## Fonctionnement de la webapp

* **Indicateurs** :

  * Temps (s) · Passes (#) · Issue/Zone (Perte Z avant / Z avant / Z arrière / Tir manqué / Tir cadré / But).
* **Niveaux** :

  * N1 Panique · N2 Lent pas efficace · N3 Lent efficace · N4 Vite efficace.
  * Règle de **priorité** : **issue** → **temps** → **passes** (détaillée dans *guide\_technique.md*).
* **Stats** :

  * KPIs (temps moyen, passes moyennes, % buts), **niveau moyen** (ex. 3,2 → image N3 ; ≥ 3,8 → image N4).
  * Graphes Chart.js : temps (ligne rouge), passes (ligne verte), issues (barres colorées).
* **Données** : tout est stocké en **localStorage** (pas de serveur).
* **Exports** : CSV (Excel/Numbers) en UTF-8 BOM ; QR récapitulatif lisible.

> Pour les détails d’implémentation (modèle, algorithme, clés de stockage), voir **guide\_technique.md**.

---

## Déploiement (local & statique)

* **Local** :

  * Télécharger `index.html` (+ `assets/` si vous utilisez des images de niveaux `n1.png..n4.png`).
  * Ouvrir dans Chrome/Firefox/Edge/Safari récents.
* **Statique (GitHub Pages / Netlify / Vercel)** :

  * Déposer `index.html` à la racine du site (aucun build).
  * Activer Pages (branche principale) ou importer le dépôt sur Netlify/Vercel.

---

## Contribuer

* **Issues / PR** bienvenues (UX, accessibilité, stats, filtrage par élève/séance, PWA…).
* Merci de lire d’abord :

  * **IntroTraam.md** (vision pédagogique)
  * **guide\_technique.md** (contrats de données et règles de calcul)

---

## Licence

Usage pédagogique libre. Mentionner la source et partager vos retours pour enrichir le projet.
