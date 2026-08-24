# Roue de création de joueur

Générateur de rookie fictif pour NBA 2K (mode MyNBA) : tirage aléatoire d'un profil complet en 8 étapes, génération des 22 attributs détaillés, sauvegarde d'un effectif, et système de progression de saison. Le tout dans un seul fichier HTML autonome, sans backend.

## Aperçu

1. **Roue de création (page 1)** - 8 tirages successifs, un bouton, un profil qui se construit ligne par ligne :
   - Nom et nationalité (10 nationalités, prénoms/noms assortis)
   - Poste (Meneur, Arrière, Ailier, Ailier fort, Pivot)
   - Taille et poids (fourchettes réalistes par poste)
   - Envergure (taille + 2 à 6 pouces)
   - Style de jeu (4 archétypes par poste, avec joueurs NBA de comparaison et points forts)
   - Joueur de comparaison (cohérent avec le style tiré)
   - Overall rookie (70-80) et points forts hérités du style

2. **Attributs du joueur (page 2)** - les 22 attributs façon NBA 2K, regroupés par catégorie colorée (Finition, Shooting, Playmaking, Défense, Rebond, Physique). Générés à partir du poste, du style de jeu et des points forts, puis **recalés mathématiquement** pour que leur moyenne pondérée par poste (à la manière du système positionnel de NBA 2K) retombe exactement sur l'overall rookie annoncé à la page 1.

3. **Mon effectif (page 3)** - les joueurs sauvegardés, conservés dans le navigateur (`localStorage`). Suppression possible.

4. **Progression de saison (page 4)** - on choisit un joueur sauvegardé, on entre ses statistiques moyennes de la saison (minutes, points, rebonds, passes, interceptions, contres, pourcentages au tir, balles perdues), et ses attributs montent ou descendent selon ces performances **jugées par rapport à son poste** (8 rebonds/match, c'est excellent pour un meneur mais médiocre pour un pivot). L'overall est recalculé automatiquement à partir des nouveaux attributs, et l'évolution peut être enregistrée dans l'effectif.

## Utilisation

Aucune installation nécessaire : c'est un fichier HTML unique.

- **En local** : ouvrir `index.html` directement dans un navigateur.
- **En ligne (GitHub Pages)** :
  1. Déposer le fichier à la racine du dépôt sous le nom `index.html`.
  2. Dans les paramètres du dépôt GitHub, activer *Pages* sur la branche principale (dossier racine).
  3. Le site est servi à l'adresse `https://<utilisateur>.github.io/<repo>/`.

## Fonctionnement technique

- Un seul fichier HTML/CSS/JS, sans dépendance à un serveur ou une API.
- Polices chargées depuis Google Fonts (Barlow Condensed, IBM Plex Sans, IBM Plex Mono) - nécessite une connexion internet pour le rendu final, le site fonctionne sinon avec les polices de secours du système.
- Le tirage en cours (pages 1 et 2) vit uniquement en mémoire de page : il est perdu à l'actualisation tant qu'il n'est pas sauvegardé.
- L'effectif sauvegardé (page 3) et les évolutions de saison (page 4) sont stockés via `localStorage`, sous la clé `rcj_roster_v1`, **propre à chaque navigateur** - pas de synchronisation entre appareils, pas de compte, pas de serveur.

## Logique de génération et de progression

- **Overall → attributs** : chaque poste a une pondération différente par catégorie d'attributs (ex. un Meneur doit beaucoup au Playmaking, un Pivot beaucoup au Rebond et à la Défense intérieure). Les 22 attributs sont générés puis ajustés pour que leur moyenne pondérée corresponde exactement à l'overall annoncé.
- **Progression de saison** : chaque statistique saisie est comparée à des repères (mauvais / moyen / excellent) définis **par poste**, ce qui donne un score de -1,5 à +1,5. Ce score est ensuite réparti sur les attributs concernés (ex. passes et balles perdues → playmaking, contres et interceptions → défense). Les attributs physiques (vitesse, agilité, force, détente) restent fixes d'une saison à l'autre ; seule l'endurance évolue, en fonction des minutes jouées.

## Limites connues

- Les données ne sont pas partagées entre navigateurs ou appareils (pas de compte, pas de synchronisation cloud).
- Vider les données de navigation du site efface l'effectif sauvegardé.
- Les repères statistiques de progression sont des approximations générales, pas des données NBA officielles.

## Pistes d'évolution possibles

- Export / import de l'effectif en JSON pour changer de navigateur sans tout perdre.
- Historique des évolutions saison par saison pour un même joueur.
- Badges ou tendances de jeu en complément des 22 attributs.
