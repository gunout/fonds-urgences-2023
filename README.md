<div align="center">

# Monitor Fonds d'urgence — Émeutes été 2023

[![Source](https://img.shields.io/badge/source-data.gouv.fr-003399?style=flat-square)](https://www.data.gouv.fr/datasets/fonds-durgence-pour-les-communes-impactees-par-les-emeutes-de-lete-2023) [![Licence](https://img.shields.io/badge/licence-Open%20Data-00a95f?style=flat-square)](https://www.etalab.gouv.fr/licence-ouverte-open-licence) [![Version](https://img.shields.io/badge/version-2.0-ffcc00?style=flat-square)]() [![Status](https://img.shields.io/badge/status-stable-00a95f?style=flat-square)]()

Tableau de bord interactif pour explorer les données du fonds d'urgence de la Région Île-de-France destiné aux communes impactées par les émeutes de l'été 2023.
</div>
## Sommaire

- [Présentation](#présentation)
- [Fonctionnalités](#fonctionnalités)
- [Données](#données)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Structure du projet](#structure-du-projet)
- [Développement](#développement)
- [Limites connues](#limites-connues)
- [Sources et crédits](#sources-et-crédits)

## Présentation

Ce tableau de bord est un outil autonome, sans dépendance serveur, conçu pour visualiser et analyser les 41 communes ayant bénéficié d'avances remboursables du fonds d'urgence régional, pour un montant total de 11,3 M€.

Il permet d'explorer les données par commune, département, montant et nature des opérations, avec une carte interactive géolocalisée.

### Contexte

À la suite des émeutes de l'été 2023, la Région Île-de-France a mis en place un fonds d'urgence de plus de 20 M€ :

- 18 M€ destinés aux communes (avances remboursables jusqu'à 70 %)
- 2 M€ destinés aux commerçants et artisans

Après instruction des dossiers, 41 communes et 37 entreprises ont été retenues, pour des montants respectifs de 11,3 M€ et 171 500 €.

## Fonctionnalités

### Visualisation

- Liste paginée des communes avec recherche full-text (nom, INSEE, département, opérations)
- Carte interactive (Leaflet + CARTO) avec marqueurs proportionnels au montant et popups détaillés
- Vue par département avec agrégation et part du montant total
- Analyses : barres par département, top 15 communes

### Interaction

- Filtres combinables : recherche texte + département + tri (montant, commune, département)
- Sélection croisée : clic sur un marqueur, une carte ou la sidebar synchronise les vues
- KPI dynamiques : communes, montant cumulé, moyenne, médiane, maximum
- Thème clair/sombre persistant

### Données

- Chargement local du fichier JSON officiel (pas d'appel API externe)
- Export CSV et JSON des données filtrées
- Formatage français : montants en euros, nombres avec séparateurs

### Interface

- Design system inspiré de l'État français (bleu Marianne, or, rouge)
- Responsive (mobile, tablette, desktop)
- Accessibilité : raccourci barre oblique pour la recherche, Échap pour fermer

## Données

### Source officielle

Jeu de données : [Fonds d'urgence pour les communes impactées par les émeutes de l'été 2023](https://www.data.gouv.fr/datasets/fonds-durgence-pour-les-communes-impactees-par-les-emeutes-de-lete-2023)

- Éditeur : Région Île-de-France
- Publication : 18 décembre 2023
- Dernière mise à jour : 19 décembre 2023
- Licence : Open Data (Licence Ouverte 2.0)
- Formats : CSV, JSON, Shapefile

### Structure des données

| Champ | Type | Description | Exemple |
|---|---|---|---|
| num | float | Numéro d'ordre | 15.0 |
| beneficiaire | string | Nom de la commune | Gennevilliers |
| c_insee | int | Code INSEE | 92036 |
| dpt | float | Code département | 92.0 |
| departement | string | Libellé département | 92 - Hauts-de-Seine |
| operations | string | Description des travaux | Remise en état de la Bourse du travail |
| montant_de_l_avance | float | Montant de l'avance (€) | 500000.0 |
| centroid | object | Coordonnées géographiques | lon 2.29, lat 48.93 |

### Statistiques clés

- 41 communes réparties sur 7 départements franciliens
- 11,3 M€ d'avances remboursables
- Plafond individuel : 500 000 € (atteint par 14 communes)
- Montant minimum : 2 065,50 € (Torcy)
- Départements couverts : 77, 78, 91, 92, 93, 94, 95

## Installation

### Prérequis

- Un navigateur moderne (Chrome, Firefox, Edge, Safari)
- Python 3 ou Node.js (pour servir les fichiers en HTTP)

Important : l'ouverture directe via le protocole fichier bloque le chargement du JSON pour des raisons de sécurité (CORS). Un serveur HTTP local est nécessaire.

### Étapes

1. Cloner le dépôt :

       git clone https://github.com/votre-user/urgences.git
       cd urgences

2. Vérifier la présence des fichiers :

       ls
       # index.html
       # fonds-d-urgence-pour-les-communes-impactees-par-les-emeutes-de-l-ete-2023.json

3. Lancer un serveur local :

       python3 -m http.server 8000

### Accès

Ouvrir http://localhost:8000 dans un navigateur.

## Utilisation

### Navigation

| Onglet | Description |
|---|---|
| Liste | Vue tabulaire paginée des communes |
| Carte | Marqueurs géolocalisés, taille proportionnelle au montant |
| Départements | Agrégation par département |
| Analyses | Graphiques comparatifs |

### Recherche et filtres

- Recherche texte : nom de commune, code INSEE, département, description des opérations
- Filtre département : menu déroulant
- Tri : montant (croissant/décroissant), commune (A→Z), département (A→Z)

### Interactions

- Clic sur une commune (liste, sidebar ou marqueur) : sélection croisée, centrage carte
- Clic sur un département : filtre immédiat
- Raccourci barre oblique : focus sur la recherche
- Échap : ferme les modales

### Export

- CSV : données filtrées avec tous les champs
- JSON : données filtrées au format structuré

### Thème

Bouton dans la barre supérieure. Le choix est mémorisé entre les sessions.

## Structure du projet

    urgences/
    ├── index.html                                    # Dashboard autonome (HTML/CSS/JS)
    ├── fonds-d-urgence-...-2023.json                 # Données officielles (JSON)
    ├── fonds-d-urgence-...-2023.shp                  # Géométries (optionnel)
    ├── fonds-d-urgence-...-2023.dbf                  # Attributs (optionnel)
    ├── fonds-d-urgence-...-2023.prj                  # Projection (optionnel)
    ├── fonds-d-urgence-...-2023.shx                  # Index spatial (optionnel)
    └── README.md                                     # Ce fichier

### Fichiers Shapefile

Les fichiers shp/dbf/prj/shx contiennent les polygones des communes. Ils ne sont pas utilisés par le dashboard actuel (qui s'appuie sur les centroïdes du JSON), mais peuvent servir pour :

- Une carte choroplèthe précise (contours des communes)
- Des analyses spatiales (superficie, contiguïté)

Conversion en GeoJSON si besoin :

    ogr2ogr -f GeoJSON communes.geojson fonds-d-urgence-....shp

## Développement

### Technologies

- HTML5 / CSS3 : interface sans framework
- JavaScript vanilla : pas de build, pas de dépendance npm
- Leaflet 1.9.4 (CDN) : cartographie interactive
- CARTO/OSM : fonds de carte

### Architecture

    index.html (fichier unique)
    ├── style  : design system CSS
    ├── body   : structure 3 colonnes
    └── script : logique applicative
        ├── state          : état global
        ├── normalize()    : mapping des champs
        ├── loadData()     : chargement JSON
        ├── applyFilters() : filtres + tri
        ├── render*()      : vues (liste/carte/etc.)
        └── export*()      : CSV/JSON

### Ajouter un champ

1. Ajouter le champ dans la fonction normalizeCommune :

       const monChamp = raw.mon_champ || '—';
       return { ..., monChamp };

2. L'afficher dans la fonction renderResults :

       <span class="tag">${escapeHtml(c.monChamp)}</span>

3. L'ajouter à l'export CSV dans la liste cols.

### Modifier la carte

- Fond de carte : remplacer l'URL tileLayer dans renderMap
- Couleurs des marqueurs : fonction color dans renderMap
- Taille : variable size dans renderMap

### Déboguer

Ouvrir la console développeur (F12). Les erreurs de chargement sont préfixées par loadData.

## Limites connues

1. Dépendance à Leaflet (CDN) : la carte nécessite une connexion internet pour charger la librairie et les tuiles. Le reste du dashboard fonctionne hors-ligne.

2. Protocole fichier non supporté : le chargement du JSON est bloqué par les navigateurs en protocole fichier. Un serveur HTTP local est requis.

3. Entreprises non détaillées : les 37 entreprises aidées (171 500 €) sont mentionnées dans la description du jeu de données mais ne figurent pas dans le fichier JSON. Les valeurs sont codées en dur dans l'interface.

4. Pas de persistance des filtres : les filtres sont réinitialisés à chaque rechargement de page.

5. Données statiques : le JSON est chargé une fois ; aucune actualisation automatique.

## Sources et crédits

### Données

- Éditeur : [Région Île-de-France](https://www.iledefrance.fr/)
- Plateforme : [data.gouv.fr](https://www.data.gouv.fr)
- Licence : [Licence Ouverte 2.0](https://www.etalab.gouv.fr/licence-ouverte-open-licence)

### Bibliothèques

- [Leaflet](https://leafletjs.com/) — BSD 2-Clause
- [CARTO Basemaps](https://carto.com/basemaps/) — Attribution requise
- [OpenStreetMap](https://www.openstreetmap.org/) — ODbL

### Design system

Inspiré du [Système de Design de l'État](https://www.systeme-de-design.gouv.fr/) (DSFR) :

- Couleurs : bleu Marianne, or, rouge Marianne
- Typographie : Marianne (fallback system-ui)
- Composants : cartes, badges, KPI, onglets

## Licence

Ce projet est distribué sous Licence Ouverte 2.0 pour le code, conformément aux principes de l'Open Data. Les données restent la propriété de la Région Île-de-France.

## Contribution

Les suggestions et corrections sont bienvenues. Pour toute modification :

1. Tester sur les 3 navigateurs principaux
2. Vérifier la compatibilité mobile (moins de 1000 px)
3. Valider l'accessibilité (contraste, navigation clavier)

---

Dernière mise à jour : octobre 2026 · Version : 2.0

---

<div align="center">

### 🇫🇷 Gunout · 2026

![Made in France](https://img.shields.io/badge/Made_in-France-002395?style=flat-square&labelColor=FFFFFF&logo=data:image/svg+xml;base64,PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHZpZXdCb3g9IjAgMCA5MDAgNjAwIj48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjYwMCIgZmlsbD0iIzAwMjM5NSIvPjxyZWN0IHdpZHRoPSI5MDAiIGhlaWdodD0iNDAwIiB5PSIxMDAiIGZpbGw9IiNmZmYiLz48cmVjdCB3aWR0aD0iOTAwIiBoZWlnaHQ9IjIwMCIgeT0iNDAwIiBmaWxsPSIjZWQyOTM5Ii8+PC9zdmc+)
![GitHub](https://img.shields.io/badge/GitHub-gunout-181717?style=flat-square&logo=github&logoColor=white)
![Year](https://img.shields.io/badge/2026-ED2939?style=flat-square&labelColor=FFFFFF)

<sub>© 2026 <strong>Gunout</strong> — Tous droits réservés.</sub>

</div>
