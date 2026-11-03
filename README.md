# Suivi de candidatures

Maquette d'un outil simple pour suivre ses candidatures à des offres d'emploi. Tout tient dans un seul fichier `index.html` (HTML, CSS et JavaScript intégrés), sans dépendance externe ni serveur.

## Fonctionnalités

- **Statuts** colorés : « Refusée » (rouge), « En cours d'examen » (orange), « Validée » (vert), avec compteurs par statut et total.
- **Tri** par date (plus récente / plus ancienne), par secteur, par entreprise et par temps de trajet depuis le domicile (le plus court d'abord).
- **Filtres** par statut, secteur et entreprise, plus une recherche texte. Le temps de trajet sert uniquement au tri, pas au filtrage.
- **Ajout local** d'une candidature via un petit formulaire, suppression, et bouton « Réinitialiser les exemples ».
- Affichage adapté au mobile.

## Données

Les offres affichées sont des **données d'exemple** avec des entreprises fictives. Les candidatures ajoutées sont enregistrées **uniquement dans le navigateur** (`localStorage`) : rien n'est envoyé à un serveur.

Le lieu de domicile utilisé pour le temps de trajet est défini dans la constante `DOMICILE` en haut du script de `index.html`.

Site : https://mgj-lab.github.io/suivi-candidatures/
