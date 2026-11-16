# Conception des cas de test : HostelOps

Version 1.1 | Semaine 4
Les identifiants TC reprennent ceux de `05-tracabilite.md`. Les colonnes « Résultat obtenu » et « Statut » restent vides jusqu'à l'exécution. Les routes viennent du README ; les noms exacts des champs JSON sont à relever dans `backend/routes/*.js`. Points **[À CONFIRMER]** : à vérifier dans le code.

## 1. Faits constatés
- Pas de champ titre : catégorie, priorité, description, image facultative.
- Description : 10 à 1 000 caractères. Image : 5 Mo maximum, JPG, PNG, GIF.
- Statuts : Pending, In Progress, Resolved.
- Rôles : étudiant et admin, choisis à l'inscription.
- Routes : POST /api/register, POST /api/login, POST /api/complaints, GET /api/complaints/my, GET /api/complaints (admin), PUT /api/complaints/:id (admin).

## 2. Partitions d'équivalence

| Champ | Partition valide | Partitions invalides |
|---|---|---|
| Description | 10 à 1 000 caractères | Vide ; 1 à 9 ; plus de 1 000 ; espaces seuls |
| Catégorie | Chaque valeur de la liste | Absente ; valeur inexistante (API) |
| Priorité | Chaque valeur de la liste | Absente ; valeur inexistante (API) |
| Image | Absente ; JPG, PNG ou GIF de 5 Mo ou moins | Plus de 5 Mo ; autre format ; fichier renommé en .jpg |
| Email d'inscription | Valide et inconnu | Déjà utilisé ; format invalide ; vide |
| Statut envoyé à PUT | Pending, In Progress, Resolved | Valeur inexistante (ex. « Closed ») |

## 3. Valeurs limites

| Champ | Valeurs | Résultat attendu |
|---|---|---|
| Description | 9 | Refusée |
| | 10, 11, 999, 1 000 | Acceptées |
| | 1 001 | Refusée |
| Image | 5 242 880 octets | Acceptée |
| | 5 242 881 octets | Refusée |

## 4. Table de décision : droits par rôle

| Action (route) | Non connecté | Étudiant | Admin |
|---|---|---|---|
| Créer une réclamation (POST /api/complaints) | 401 | Autorisé | [À CONFIRMER] |
| Ses réclamations (GET /api/complaints/my) | 401 | Autorisé, les siennes seulement | [À CONFIRMER] |
| Toutes les réclamations (GET /api/complaints) | 401 | 403 | Autorisé |
| Changer un statut (PUT /api/complaints/:id) | 401 | 403 | Autorisé |

## 5. Table des transitions

| De \ Vers | Pending | In Progress | Resolved |
|---|---|---|---|
| Pending | non applicable | Autorisée (TC-11) | Interdite (TC-13) |
| In Progress | Interdite (TC-31) | non applicable | Autorisée (TC-12) |
| Resolved | Interdite (TC-14) | Interdite (TC-32) | non applicable |

Statut inexistant : TC-33.

## 6. Catalogue

Type : **P** positif, **N** négatif, **L** valeur limite.

| ID | Exig. | Technique | Type | Préconditions | Étapes | Données | Résultat attendu | Obtenu | Statut |
|---|---|---|---|---|---|---|---|---|---|
| TC-01 | EX-01 | Partition | P | U1 existe | POST /api/login (ou page de connexion) avec email et mot de passe corrects | U1 | 200, jeton reçu, tableau de bord affiché | | |
| TC-02 | EX-02 | Partition | N | U1 existe | POST /api/login avec bon email, mauvais mot de passe | U1 / « faux123 » | 401, message générique | | |
| TC-03 | EX-02 | Partition | N | Aucune | POST /api/login avec email inconnu | inconnu@test.com | 401, même message que TC-02 | | |
| TC-04 | EX-03 | Partition | N | U1 existe | POST /api/register avec l'email de U1 | Email de U1 | 400 ou 409, aucun doublon créé | | |
| TC-05 | EX-03 | Table de décision | N | Aucune | POST /api/register avec le rôle admin, sans être connecté | Corps avec rôle « admin » | Refusé ou rôle ignoré. D'après le README, le compte admin sera probablement créé : anomalie possible (R13) | | |
| TC-06 | EX-04, EX-05 | Partition | P | U1 connecté | POST /api/complaints avec catégorie, priorité, description de 30 caractères, sans image | Electrical, Medium | 201, réclamation visible dans GET /api/complaints/my | | |
| TC-07 | EX-04 | Partition | N | U1 connecté | POST /api/complaints avec description vide | Description vide | 400, rien n'est créé | | |
| TC-08 | EX-04 | Limite | L | U1 connecté | POST avec 9 caractères, puis 10, **directement par l'API** | `a` × 9, `a` × 10 | 9 : 400 ; 10 : 201 | | |
| TC-09 | EX-04 | Limite | L | U1 connecté | POST avec 1 000, puis 1 001 caractères, directement par l'API | `a` × 1000, `a` × 1001 | 1 000 : 201 ; 1 001 : 400 | | |
| TC-10 | EX-05 | Partition | P | U1 connecté | Créer une réclamation valide, lire son statut | Données de TC-06 | Statut Pending | | |
| TC-11 | EX-06 | Transition | P | R-A (Pending), ADM connecté | PUT /api/complaints/:id avec statut In Progress | R-A | 200, statut In Progress | | |
| TC-12 | EX-06 | Transition | P | R-B (In Progress), ADM connecté | PUT avec statut Resolved | R-B | 200, statut Resolved | | |
| TC-13 | EX-07 | Transition | N | R-A (Pending), ADM connecté | PUT avec statut Resolved | R-A | 400 ou 409, statut inchangé | | |
| TC-14 | EX-07 | Transition | N | R-C (Resolved), ADM connecté | PUT avec statut Pending | R-C | 400 ou 409, statut inchangé | | |
| TC-15 | EX-08 | Table de décision | N | R-C (Resolved), U1 connecté | U1 tente toute modification de R-C (interface, ou PUT avec son jeton) | R-C | Refusée (403), données inchangées. Si aucune route de modification n'existe, noter « non applicable » avec la preuve | | |
| TC-16 | EX-09 | Table de décision | N | U1 connecté | U1 appelle GET /api/complaints (route admin) | Jeton de U1 | 403, aucune donnée | | |
| TC-17 | EX-09 | Table de décision | P | U1 et U2 ont chacun une réclamation | U2 appelle GET /api/complaints/my | Jeton de U2 | Seulement les réclamations de U2 | | |
| TC-18 | EX-10 | Table de décision | N | R-A (Pending), U1 connecté | U1 appelle PUT /api/complaints/:id avec statut In Progress | R-A | 403, statut inchangé | | |
| TC-19 | EX-10 | Table de décision | P | ADM connecté, réclamations de U1 et U2 | ADM appelle GET /api/complaints | Jeton de ADM | Réclamations de tous les étudiants | | |
| TC-20 | EX-11 | Partition | P | Jeu de base | Chercher un mot présent dans une seule description, puis une catégorie | « electricite », Plumbing | Seules les réclamations correspondantes | | |
| TC-21 | EX-12 | Partition | P | Réclamations dans les 3 statuts | Filtrer sur Pending, puis In Progress ; changer le tri | Jeu de base | Lignes conformes au filtre ; tri correct | | |
| TC-22 | EX-13 | Partition | P | Jeu de base | Comparer chaque compteur au nombre de lignes | Jeu de base | Total = somme ; chaque compteur exact | | |
| TC-23 | EX-14 | Partition | P | Réclamation créée | Arrêter puis relancer le backend, se reconnecter | Réclamation de TC-06 | Réclamation retrouvée à l'identique | | |
| TC-24 | EX-15 | Partition | N | Aucune | GET /api/complaints/my sans jeton | Aucune | 401, JSON | | |
| TC-25 | EX-15 | Partition | N | Aucune | GET /api/complaints/my avec un jeton modifié | Jeton falsifié | 401 | | |
| TC-26 | EX-15 | Partition | N | ADM connecté | PUT /api/complaints/999999 | Identifiant inexistant | 404, sans trace technique | | |
| TC-27 | EX-16 | Limite | L | U1 connecté | POST /api/complaints avec image de 5 Mo exactement, puis 5 Mo + 1 octet, puis .exe renommé .jpg | Fichiers de `07-jeux-de-donnees.md` | Acceptée ; refusée ; refusée | | |
| TC-28 | EX-17 | Non fonctionnel | P | Backend local démarré | 20 utilisateurs virtuels pendant 1 minute (k6) sur localhost | Script k6 (semaine 9) | 95 % en moins de 500 ms, erreurs inférieures à 1 % | | |
| TC-29 | EX-18 | Partition | P | U1 connecté | Créer une réclamation par priorité de la liste | Chaque priorité | Priorité affichée = priorité choisie | | |
| TC-30 | EX-19 | Partition | P | U1 connecté | Changer catégorie et priorité dans le formulaire | Plusieurs combinaisons | L'estimation change selon la combinaison | | |
| TC-31 | EX-07 | Transition | N | R-B (In Progress), ADM connecté | PUT avec statut Pending | R-B | Refusé, statut inchangé | | |
| TC-32 | EX-07 | Transition | N | R-C (Resolved), ADM connecté | PUT avec statut In Progress | R-C | Refusé, statut inchangé | | |
| TC-33 | EX-07 | Partition | N | R-A, ADM connecté | PUT avec un statut inexistant | « Closed » | 400, statut inchangé | | |
| TC-34 | EX-04 | Partition | N | U1 connecté | POST /api/complaints avec catégorie ou priorité inexistante, directement par l'API | Catégorie « Inconnue » | 400, rien n'est créé | | |

## 7. Bilan de la conception
- 34 cas (minimum du sujet : 15) ; positifs, négatifs et valeurs limites représentés.
- Techniques : partitions, valeurs limites, table de décision, transitions d'état.
- Scénarios alternatifs : avec et sans image (TC-06, TC-27) ; interface contre API directe (TC-07 à TC-09, TC-34).
- Cas liés aux risques de score 9 : TC-04, TC-05, TC-10 à TC-14, TC-16 à TC-19, TC-31 à TC-33.

## 8. À relever avant la semaine 8
Dans `backend/routes/complaints.js` et `auth.js` : noms des champs JSON (category, priority, description, status ?), vérifications de rôle, règles de transition, format du jeton (en-tête Authorization Bearer ?). Reporter les valeurs dans les étapes ci-dessus.
