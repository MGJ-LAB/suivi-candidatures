# Suivi de candidatures

Maquette d'un outil simple pour suivre ses candidatures à des offres d'emploi. La page `index.html` (HTML, CSS et JavaScript intégrés) fonctionne sans dépendance externe ni serveur ; les données sont lues depuis le fichier chiffré `data.enc.json`.

## Fonctionnalités

- **Statuts** colorés : « Refusée » (rouge), « En cours d'examen » (orange), « Validée » (vert), avec compteurs par statut et total.
- **Tri** par date (plus récente / plus ancienne), par secteur, par entreprise et par temps de trajet depuis le domicile (le plus court d'abord).
- **Filtres** par statut, secteur, entreprise, type de contrat et source, plus une recherche texte. Le temps de trajet sert uniquement au tri, pas au filtrage.
- **Lecture seule** : seules les candidatures du fichier chiffré sont affichées (pas d'ajout ni de suppression depuis le site).
- **Bouton « 🔴 À traiter (n) »** rouge, replié par défaut (masqué si n = 0) : un clic affiche la liste des échéances (`date_limite`) dans les 7 prochains jours ou dépassées pour une candidature encore en cours d'examen, et des relances dues ; un nouveau clic, « Fermer » ou Échap la replie. Badge « ⏰ Échéance J-n » sur les cartes.
- **Récap de la semaine** (repliable) : candidatures envoyées et changements de statut des 7 derniers jours (aujourd'hui inclus, d'après `historique`), relances en retard ou prévues dans les 7 prochains jours.
- **Chances de réponse (estimation)** : score indicatif de 5 à 100 pour les candidatures en cours d'examen, affiché sur chaque carte, dans un classement et comme option de tri. Ce n'est **pas une probabilité réelle**. Règles : « Validée » = réponse positive (en tête, sans score) ; « Refusée » = 0, exclue ; « En cours d'examen » : base 50, −2 points par jour au-delà de 7 jours depuis la candidature, +10 si recruteur connu, +10 si la source est une agence d'emploi ou d'intérim, −10 si candidature spontanée, +5 si une relance est prévue, résultat borné entre 5 et 100. Le secteur n'est pas utilisé. La formule est dans la fonction `chancesReponse` de `index.html`.
- **Détails** dépliables par carte, badge « À relancer » et compteur associé, marqueur « à vérifier » pour les entrées signalées `ambigu`.
- Affichage adapté au mobile.

## Données

Les données affichées proviennent de `data.enc.json` (chiffré). Le fichier `data-exemple.json` contient des données d'exemple fictives (en clair) au format attendu. Le site est en lecture seule et n'utilise pas le stockage local du navigateur : les données déchiffrées restent uniquement en mémoire le temps de la session et rien n'est envoyé à un serveur.

### Structure des données

Le JSON en clair est un tableau d'objets. Les 13 champs affichés :

| Champ | Libellé | Format |
|---|---|---|
| `poste` | Poste | texte, obligatoire |
| `entreprise` | Entreprise | texte, obligatoire |
| `secteur` | Secteur | texte, obligatoire |
| `ville` | Ville | texte, vide ou `null` |
| `date` | Date de candidature | `AAAA-MM-JJ`, obligatoire |
| `trajet` | Temps de trajet | nombre de minutes ou `null` |
| `statut` | Statut | « Refusée », « En cours d'examen » ou « Validée », obligatoire |
| `type_contrat` | Type de contrat | texte ou `null` (CDI, CDD, Intérim, Alternance…) |
| `mode_travail` | Mode de travail | texte ou `null` (Présentiel, Télétravail, Hybride) |
| `salaire` | Salaire proposé | texte libre ou `null` |
| `source` | Source de l'offre | texte ou `null` (Indeed, LinkedIn, Adecco, Candidature spontanée…) |
| `recruteur` | Recruteur / agence | texte ou `null` |
| `date_relance` | Date de relance prévue | `AAAA-MM-JJ` ou `null` |

Champs facultatifs supplémentaires : `date_limite` (`AAAA-MM-JJ` ou `null`, échéance de candidature ou de réponse), `historique` (`null` ou tableau de `{ "date": "AAAA-MM-JJ", "statut": … }`), `note` (texte, affiché dans les détails), `ambigu` (`true` = marqueur « à vérifier »), `preuve` et `source_detail` (texte, jamais affichés). Les valeurs absentes s'affichent « Non renseigné ». `setup.html` vérifie cette structure avant de chiffrer et refuse un statut inconnu.

Le lieu de domicile utilisé pour le temps de trajet est défini dans la constante `DOMICILE` en haut du script de `index.html`.

## Accès protégé

Le site n'affiche qu'un formulaire de connexion. Les candidatures ne figurent jamais en clair dans le dépôt ni sur le site : elles sont stockées dans `data.enc.json`, chiffrées avec **AES-256-GCM**. La clé est dérivée de l'identifiant et du mot de passe avec **Argon2id** (64 Mio de mémoire, 3 passes, sel aléatoire) ; aucun mot de passe ni empreinte de mot de passe n'est stocké. Une connexion réussie correspond simplement à un déchiffrement réussi, effectué dans le navigateur. Les paramètres de dérivation sont enregistrés dans le fichier et pourront être relevés plus tard.

Pour mettre à jour les données : ouvrir `setup.html` en local, saisir l'identifiant et le mot de passe, coller ou charger le JSON en clair, cliquer sur « Chiffrer et télécharger », puis remplacer `data.enc.json` dans le dépôt. Le chiffrement se fait entièrement dans le navigateur, sans aucun envoi réseau. Ne jamais déposer le fichier JSON en clair dans le dépôt.

Site : https://mgj-lab.github.io/suivi-candidatures/
