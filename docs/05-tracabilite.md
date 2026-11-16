# Matrice de traçabilité : HostelOps

Version 1.0 | Semaine 3
Elle relie chaque exigence (`04-exigences-et-criteres.md`) aux risques (`02-risques.md`) et aux cas de test. Les identifiants TC sont **provisoires** : la semaine 4 les détaille (au moins 15 cas documentés). La colonne Résultat reste vide jusqu'à l'exécution.

## 1. Matrice exigences, risques, cas de test

| Exigence | Risque(s) | Cas de test | Niveau | Résultat |
|---|---|---|---|---|
| EX-01 | R4 | TC-01 connexion valide | API, E2E | |
| EX-02 | R4, R9 | TC-02 mauvais mot de passe ; TC-03 email inexistant | API | |
| EX-03 | R13 | TC-04 email déjà utilisé ; TC-05 inscription avec rôle admin | API | |
| EX-04 | R5 | TC-06 création valide ; TC-07 description vide ; TC-08 limites 9 et 10 caractères ; TC-09 limites 1 000 et 1 001 | Unitaire, API | |
| EX-05 | R3 | TC-10 statut initial Pending | Intégration | |
| EX-06 | R3 | TC-11 Pending vers In Progress ; TC-12 In Progress vers Resolved | Unitaire, API | |
| EX-07 | R3 | TC-13 Pending vers Resolved refusée ; TC-14 Resolved vers Pending refusée | Unitaire, API | |
| EX-08 | R6 | TC-15 modification d'une réclamation Resolved par un étudiant | Unitaire, API | |
| EX-09 | R1 | TC-16 étudiant A lit la réclamation de B ; TC-17 liste limitée à ses réclamations | API, E2E | |
| EX-10 | R2 | TC-18 étudiant tente de changer un statut ; TC-19 admin voit toutes les réclamations | API | |
| EX-11 | R10 | TC-20 recherche par mot et par catégorie | E2E, API | |
| EX-12 | R10 | TC-21 filtre par statut et tri | E2E | |
| EX-13 | R11 | TC-22 cohérence des compteurs | E2E, intégration | |
| EX-14 | R7 | TC-23 données conservées après redémarrage | Intégration | |
| EX-15 | R9 | TC-24 accès sans jeton ; TC-25 jeton invalide ; TC-26 ressource inexistante | API | |
| EX-16 | R8 | TC-27 fichier interdit ou trop gros | API | |
| EX-17 | R12 | TC-28 charge de 20 utilisateurs virtuels | k6 | |

## 2. Couverture des risques

| Risque | Score | Exigences associées | Cas de test |
|---|---|---|---|
| R1 | 9 | EX-09 | TC-16, TC-17 |
| R2 | 9 | EX-10 | TC-18, TC-19 |
| R3 | 9 | EX-05, EX-06, EX-07 | TC-10 à TC-14 |
| R4 | 6 | EX-01, EX-02 | TC-01 à TC-03 |
| R5 | 6 | EX-04 | TC-06 à TC-09 |
| R6 | 6 | EX-08 | TC-15 |
| R7 | 6 | EX-14 | TC-23 |
| R8 | 6 | EX-16 | TC-27 |
| R9 | 4 | EX-02, EX-15 | TC-02, TC-03, TC-24 à TC-26 |
| R10 | 4 | EX-11, EX-12 | TC-20, TC-21 |
| R11 | 4 | EX-13 | TC-22 |
| R12 | 4 | EX-17 | TC-28 |
| R13 | 6 | EX-03 | TC-04, TC-05 |

## 3. Contrôles de la matrice
- Chaque exigence a au moins un cas de test : oui (17 sur 17).
- Chaque risque est couvert par au moins un cas de test : oui (13 sur 13).
- Les risques de score 9 ont plusieurs cas, à plusieurs niveaux : oui.
- Seuls les cas dont le **résultat** est renseigné comptent comme preuves.

## 4. Utilisation
- À chaque anomalie, noter l'identifiant TC qui l'a révélée et l'exigence concernée.
- Si une exigence change, rechercher dans la matrice les tests à mettre à jour.
- En fin de campagne, remplir la colonne Résultat (Réussi, Échoué, Non exécuté) : elle alimente le bilan.
