# Fiche projet : test et validation de HostelOps

Version 1.1 | Mise à jour d'après le README du projet et l'exploration de l'interface

## 1. Présentation
| Élément | Contenu |
|---|---|
| Système testé | HostelOps, application web de gestion des réclamations d'un foyer d'étudiants |
| Dépôt d'origine | https://github.com/Deepthi055/hostelops |
| Dépôt de test (fork) | https://github.com/ghada12-ygb/hostelops |
| Équipe | Ghada Yaakoubi, [binôme éventuel] |
| Encadrant | [nom] |
| Période | 10 semaines |

## 2. Technologies du système testé
- Backend : Node.js, Express, Sequelize, SQLite, JWT, bcryptjs, Multer (adresse locale : http://localhost:5000)
- Frontend : React 18, React Router, Axios, Context API (adresse locale : http://localhost:3000)
- Déploiement : Docker, Docker Compose, Nginx

## 3. Utilisateurs et rôles
| Rôle HostelOps | Équivalent RéclaGest | Remarque |
|---|---|---|
| Étudiant (student) | Client | Dépose et consulte ses réclamations |
| Administrateur (admin) | Administrateur | Voit toutes les réclamations, change les statuts |
| (aucun) | Agent | Pas de rôle agent dans HostelOps |

Le rôle est choisi à l'inscription (README : « Register with admin role »).

## 4. Fonctions
| Fonction | Route API (d'après le README) |
|---|---|
| Inscription | POST /api/register |
| Connexion | POST /api/login |
| Créer une réclamation (étudiant) | POST /api/complaints |
| Lister ses réclamations (étudiant) | GET /api/complaints/my |
| Lister toutes les réclamations (admin) | GET /api/complaints |
| Changer le statut (admin) | PUT /api/complaints/:id |

Interface : tableau de bord avec 4 compteurs (Total, Pending, In Progress, Resolved), formulaire de dépôt, recherche par description ou catégorie, filtre par statut, tri, image jointe.

Les routes sont à confirmer dans `backend/routes/auth.js` et `backend/routes/complaints.js`.

## 5. Données d'une réclamation (constatées)
Catégorie (liste), priorité (liste), description (10 à 1 000 caractères), image facultative (JPG, PNG, GIF, 5 Mo maximum), statut, identifiant du propriétaire. Un temps de résolution estimé s'affiche selon la catégorie et la priorité.

## 6. Fonctions critiques
1. Authentification et rôles (JWT)
2. Dépôt d'une réclamation et validation des champs
3. Changement de statut par l'administrateur
4. Isolation des données entre étudiants
5. Persistance en base SQLite

## 7. Écarts avec le référentiel du sujet
| Règle du sujet | Constat sur HostelOps | Décision |
|---|---|---|
| Titre de 5 à 100 caractères | Aucun champ titre | Règle non applicable, notée comme écart |
| Description de 10 à 1 000 caractères | Confirmé dans l'interface | Appliquée telle quelle |
| Statuts NOUVELLE, EN_COURS, RESOLUE, CLOTUREE | Pending, In Progress, Resolved | Resolved traité comme état final |
| Réclamation clôturée non modifiable | Pas de statut clôturé ; pas de route de modification de la description d'après le README | Appliquée à Resolved ; à confirmer dans le code |
| Rôles client, agent, administrateur | Étudiant et administrateur seulement | Pas de tests sur l'agent |
| Date de création | Non listée dans le schéma du README | À vérifier dans `models/` |
| Historique des changements | Non mentionné | Hors périmètre sauf preuve du contraire |

## 8. Distinctions de vocabulaire
- **Erreur** : faute humaine du développeur.
- **Défaut** : trace de cette erreur dans le code.
- **Défaillance** : comportement incorrect observé à l'exécution.
- **Vérification** : le produit respecte-t-il sa spécification ?
- **Validation** : le produit répond-il au besoin de l'utilisateur ?

## 9. Objectif
Évaluer la qualité de HostelOps, détecter les anomalies et décider de sa livraison (validation, validation conditionnelle ou refus).
