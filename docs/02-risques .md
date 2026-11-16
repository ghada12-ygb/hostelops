# Analyse des risques et périmètre de test

Version 1.1
Score = Probabilité (1 à 3) × Impact (1 à 3). Seuils : 7 à 9 critique, 4 à 6 important, 1 à 3 faible.

## 1. Cartographie des risques
| ID | Risque | Module | P | I | Score | Test prévu |
|---|---|---|---|---|---|---|
| R1 | Un étudiant accède aux réclamations d'un autre (liste personnelle incorrecte, route admin ouverte) | Droits / API | 3 | 3 | 9 | API, E2E |
| R2 | Un étudiant fait une action réservée à l'admin (changer un statut avec PUT /api/complaints/:id) | Droits / API | 3 | 3 | 9 | API |
| R3 | Transition de statut incohérente (ex. Resolved vers Pending, Pending vers Resolved, statut inexistant) | Suivi | 3 | 3 | 9 | Unitaire, API |
| R13 | Le rôle admin se choisit librement à l'inscription (README : « Register with admin role ») : élévation de privilèges | Utilisateurs | 3 | 3 | 9 | API |
| R4 | Authentification faible (jeton non vérifié, sans expiration, mot de passe en clair) | Authentification | 2 | 3 | 6 | API, sécurité |
| R5 | Validation des champs absente côté serveur (description, catégorie, priorité), même si l'interface la fait | Réclamations | 3 | 2 | 6 | Partitions, valeurs limites |
| R6 | Réclamation Resolved encore modifiable | Réclamations | 2 | 3 | 6 | Unitaire, API |
| R7 | Données perdues ou incohérentes en base | Persistance | 2 | 3 | 6 | Intégration |
| R8 | Téléversement dangereux ou trop volumineux (Multer) : taille, type de fichier | Réclamations | 2 | 3 | 6 | API, sécurité |
| R9 | Mauvais codes HTTP ou messages d'erreur trop détaillés | API REST | 2 | 2 | 4 | Postman |
| R10 | Recherche, filtre, tri ou temps estimé erronés | Recherche | 2 | 2 | 4 | E2E, API |
| R11 | Compteurs du tableau de bord faux | Suivi | 2 | 2 | 4 | E2E, intégration |
| R12 | Lenteur sous charge | Global | 2 | 2 | 4 | k6 |

Changement de cette version : R13 passe de 6 à 9, car le README indique que le rôle admin s'obtient à l'inscription. Les risques de score 9 sont maintenant R1, R2, R3 et R13. Ce sont des hypothèses tant que le code et les tests ne les ont pas confirmées ou infirmées.

## 2. Périmètre
**Inclus :** authentification, inscription, réclamations, statuts, droits par rôle (étudiant, admin), recherche et filtres, API REST locale, persistance SQLite, un parcours d'interface, un test de performance local.

**Exclus (avec justification) :**
- Application publique indiquée dans le README (http://3.12.71.152) : serveur appartenant à un tiers, aucun test (surtout pas de charge) sans autorisation. Les tests se font uniquement sur `localhost`.
- Compatibilité multi-navigateurs complète : un seul navigateur, par manque de temps.
- Audit de sécurité approfondi : seulement des vérifications élémentaires.
- Docker, Nginx et déploiement : environnement local uniquement.
- Rôle agent, suppression de réclamation, historique des changements : absents d'après le README et l'interface.

## 3. Hypothèses
- Le référentiel de test est celui du sujet, adapté à HostelOps (écarts dans `01-fiche-projet.md`, section 7).
- Les tests s'exécutent sur une base SQLite de test, jamais sur des données réelles.
- Les routes de l'API sont celles du README, à confirmer dans le code.
