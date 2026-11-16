# Plan de test : HostelOps

Version 1.1 | Auteur : Ghada Yaakoubi | À valider avec l'encadrant

## 1. Objectifs
Évaluer la qualité de HostelOps, détecter les anomalies et décider de la livraison, en priorité sur les risques de score 9 : R1, R2, R3 et R13 (voir `02-risques.md`).

## 2. Niveaux de test
| Niveau | Cible | Outil | Responsable |
|---|---|---|---|
| Unitaire | Fonctions de validation, règles de transition de statut | Jest | [Membre A] |
| Intégration | Routes Express + base SQLite (CRUD, contraintes) | Jest + Supertest | [Membre A] |
| API | Routes /api/register, /api/login, /api/complaints : codes HTTP, JWT, rôles | Postman | [Membre B] |
| Interface | Parcours connexion, création, vérification | Playwright | [Membre B] |
| Non fonctionnel | Performance locale (choix possible : sécurité élémentaire) | k6 | Tous |
| Acceptation | Vérification des critères d'acceptation | Manuel | Tous |

## 3. Techniques de conception
- Partitions d'équivalence et valeurs limites : description, image, champs de saisie
- Table de décision : droits par rôle et action
- Transitions d'état : Pending, In Progress, Resolved
- Tests exploratoires : séances de 45 minutes avec une charte écrite

## 4. Environnement
| Élément | Valeur |
|---|---|
| Système | Windows, exécution locale uniquement |
| Node.js | [résultat de `node -v`] |
| Navigateur | [nom et version] |
| Backend | http://localhost:5000 |
| Frontend | http://localhost:3000 |
| Base de données | SQLite locale (`backend/database.sqlite`), sauvegardée avant chaque campagne |
| Données | 2 étudiants, 1 admin, réclamations dans chaque statut (voir `07-jeux-de-donnees.md`) |
| Dépôt | https://github.com/ghada12-ygb/hostelops |

Aucun test sur le site publique mentionné dans le README.

## 5. Critères d'entrée
- Backend et frontend démarrent en local
- Exigences relues et critères d'acceptation rédigés
- Comptes de test créés, jeu de données prêt, dépôt Git en place

## 6. Critères de sortie
À figer avant la campagne finale, en accord avec l'encadrant.
- 100 % des tests liés aux risques de score 9 (R1, R2, R3, R13) exécutés et réussis
- Au moins 90 % de l'ensemble des tests réussis
- Aucune anomalie bloquante ou critique ouverte
- Couverture de lignes d'au moins 70 % sur le code des règles métier
- Anomalies restantes documentées avec leur risque résiduel

## 7. Gestion des anomalies
| Sévérité | Définition |
|---|---|
| Bloquante | Empêche l'usage principal ou expose des données |
| Majeure | Fonction importante incorrecte, contournement possible |
| Mineure | Défaut sans impact sur le fonctionnement principal |
| Cosmétique | Affichage, texte |

Priorité : haute, moyenne, basse. Chaque fiche contient : étapes de reproduction, résultat attendu, résultat obtenu, preuve (capture ou log), identifiant du cas de test. Outil : GitHub Issues avec les étiquettes `bug`, `bloquant`, `majeur`, `mineur`.

## 8. Décision de livraison
| Décision | Condition |
|---|---|
| Validation | Critères de sortie satisfaits, aucun défaut bloquant |
| Validation conditionnelle | Défauts résiduels acceptables, risques et conditions explicités |
| Refus | Défaut critique, faille de sécurité majeure ou exigence essentielle non satisfaite |

## 9. Responsabilités
| Tâche | Responsable |
|---|---|
| Plan de test, risques, traçabilité | [à répartir] |
| Tests unitaires et intégration | [à répartir] |
| API, interface, performance | [à répartir] |
| Registre des anomalies, rapport final | [à répartir] |

## 10. Calendrier
| Semaines | Jalon |
|---|---|
| 1 à 3 | Analyse, plan, traçabilité |
| 4 à 5 | Cas de test, exploration |
| 6 à 7 | Tests unitaires et d'intégration |
| 8 à 9 | API, interface, non fonctionnel, régression |
| 10 | Campagne finale, bilan, décision |

## 11. Organisation du dépôt
```
hostelops/
├── backend/ , frontend/    (application d'origine)
├── docs/                   (documents de test 01 à 07, puis registre et bilan)
└── tests/
    ├── unit/  integration/  api/  e2e/  perf/
```
Branches : `main` stable, `feature/...` par tâche, fusion par pull request.
