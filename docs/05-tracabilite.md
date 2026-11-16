# Matrice de traçabilité : HostelOps

Version 1.1
Elle relie chaque exigence (`04-exigences-et-criteres.md`) aux risques (`02-risques.md`) et aux cas de test (`06-cas-de-test.md`). La colonne Résultat reste vide jusqu'à l'exécution : renseigner Réussi, Échoué (avec le numéro d'anomalie) ou Non exécuté (avec la raison).

## 1. Matrice exigences, risques, cas de test

| Exigence | Risque(s) | Cas de test | Niveau | Résultat |
|---|---|---|---|---|
| EX-01 | R4 | TC-01 | API, E2E | |
| EX-02 | R4, R9 | TC-02, TC-03 | API | |
| EX-03 | R13 | TC-04, TC-05 | API | |
| EX-04 | R5 | TC-06, TC-07, TC-08, TC-09, TC-34 | Unitaire, API | |
| EX-05 | R3 | TC-10 | Intégration | |
| EX-06 | R3 | TC-11, TC-12 | Unitaire, API | |
| EX-07 | R3 | TC-13, TC-14, TC-31, TC-32, TC-33 | Unitaire, API | |
| EX-08 | R6 | TC-15 | Unitaire, API | |
| EX-09 | R1 | TC-16, TC-17 | API, E2E | |
| EX-10 | R2 | TC-18, TC-19 | API | |
| EX-11 | R10 | TC-20 | E2E, API | |
| EX-12 | R10 | TC-21 | E2E | |
| EX-13 | R11 | TC-22 | E2E, intégration | |
| EX-14 | R7 | TC-23 | Intégration | |
| EX-15 | R9 | TC-24, TC-25, TC-26 | API | |
| EX-16 | R8 | TC-27 | API | |
| EX-17 | R12 | TC-28 | k6 | |
| EX-18 | R5 | TC-29 | E2E | |
| EX-19 | R10 | TC-30 | E2E | |

## 2. Couverture des risques

| Risque | Score | Exigences | Cas de test |
|---|---|---|---|
| R1 | 9 | EX-09 | TC-16, TC-17 |
| R2 | 9 | EX-10 | TC-18, TC-19 |
| R3 | 9 | EX-05, EX-06, EX-07 | TC-10 à TC-14, TC-31 à TC-33 |
| R13 | 9 | EX-03 | TC-04, TC-05 |
| R4 | 6 | EX-01, EX-02 | TC-01 à TC-03 |
| R5 | 6 | EX-04, EX-18 | TC-06 à TC-09, TC-29, TC-34 |
| R6 | 6 | EX-08 | TC-15 |
| R7 | 6 | EX-14 | TC-23 |
| R8 | 6 | EX-16 | TC-27 |
| R9 | 4 | EX-02, EX-15 | TC-02, TC-03, TC-24 à TC-26 |
| R10 | 4 | EX-11, EX-12, EX-19 | TC-20, TC-21, TC-30 |
| R11 | 4 | EX-13 | TC-22 |
| R12 | 4 | EX-17 | TC-28 |

## 3. Contrôles
- Chaque exigence (19 sur 19) a au moins un cas de test.
- Chaque risque (13 sur 13) est couvert par au moins un cas de test.
- Les risques de score 9 ont plusieurs cas, à plusieurs niveaux (unitaire, API, interface).
- Seuls les cas dont le résultat est renseigné comptent comme preuves.

## 4. Utilisation
- À chaque anomalie, noter l'identifiant TC et l'exigence concernée.
- Si une exigence change, rechercher dans la matrice les tests à mettre à jour.
- En fin de campagne, toutes les lignes doivent avoir un résultat : la colonne alimente le bilan.
