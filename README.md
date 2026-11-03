# Suivi de candidatures

Maquette d'un outil simple pour suivre ses candidatures à des offres d'emploi. La page `index.html` (HTML, CSS et JavaScript intégrés) fonctionne sans dépendance externe ni serveur ; les données sont lues depuis le fichier chiffré `data.enc.json`.

## Fonctionnalités

- **Statuts** colorés : « Refusée » (rouge), « En cours d'examen » (orange), « Validée » (vert), avec compteurs par statut et total.
- **Tri** par date (plus récente / plus ancienne), par secteur, par entreprise et par temps de trajet depuis le domicile (le plus court d'abord).
- **Filtres** par statut, secteur et entreprise, plus une recherche texte. Le temps de trajet sert uniquement au tri, pas au filtrage.
- **Ajout local** d'une candidature via un petit formulaire, suppression, et bouton « Réinitialiser les modifications locales ».
- Affichage adapté au mobile.

## Données

Les données affichées proviennent de `data.enc.json` (chiffré). Le fichier `data-exemple.json` contient des données d'exemple fictives (en clair) au format attendu. Les candidatures ajoutées sont enregistrées **uniquement dans le navigateur** (`localStorage`) : rien n'est envoyé à un serveur. Les données déchiffrées ne sont jamais enregistrées dans le navigateur.

Le lieu de domicile utilisé pour le temps de trajet est défini dans la constante `DOMICILE` en haut du script de `index.html`.

## Accès protégé

Le site n'affiche qu'un formulaire de connexion. Les candidatures ne figurent jamais en clair dans le dépôt ni sur le site : elles sont stockées dans `data.enc.json`, chiffrées avec **AES-256-GCM**. La clé est dérivée de l'identifiant et du mot de passe avec **Argon2id** (64 Mio de mémoire, 3 passes, sel aléatoire) ; aucun mot de passe ni empreinte de mot de passe n'est stocké. Une connexion réussie correspond simplement à un déchiffrement réussi, effectué dans le navigateur. Les paramètres de dérivation sont enregistrés dans le fichier et pourront être relevés plus tard.

Pour mettre à jour les données : ouvrir `setup.html` en local, saisir l'identifiant et le mot de passe, coller ou charger le JSON en clair, cliquer sur « Chiffrer et télécharger », puis remplacer `data.enc.json` dans le dépôt. Le chiffrement se fait entièrement dans le navigateur, sans aucun envoi réseau. Ne jamais déposer le fichier JSON en clair dans le dépôt.

Site : https://mgj-lab.github.io/suivi-candidatures/
