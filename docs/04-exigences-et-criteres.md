# Exigences, ambiguïtés et critères d'acceptation : HostelOps

Version 1.1 | Mise à jour d'après le README et l'exploration de l'interface
Les points marqués **[À CONFIRMER]** dépendent de la lecture du code des routes.

## 1. Liste des exigences

| ID | Exigence | Module | Priorité |
|---|---|---|---|
| EX-01 | Un utilisateur inscrit peut se connecter (POST /api/login) avec un email et un mot de passe valides | Authentification | Haute |
| EX-02 | La connexion est refusée avec des identifiants invalides, sans révéler lequel est faux | Authentification | Haute |
| EX-03 | L'inscription (POST /api/register) refuse un email déjà utilisé ; le rôle admin ne peut pas être choisi librement par un inconnu **[À CONFIRMER : règle métier à valider avec l'encadrant]** | Utilisateurs | Haute |
| EX-04 | Une réclamation exige une catégorie, une priorité et une description de 10 à 1 000 caractères, vérifiées aussi côté serveur | Réclamations | Haute |
| EX-05 | Une réclamation est créée avec le statut Pending | Réclamations | Haute |
| EX-06 | Les transitions autorisées sont : Pending → In Progress → Resolved | Suivi | Haute |
| EX-07 | Toute autre transition, et tout statut inexistant, est refusé | Suivi | Haute |
| EX-08 | Une réclamation Resolved ne peut plus être modifiée par un étudiant **[À CONFIRMER : route de modification absente du README]** | Réclamations | Haute |
| EX-09 | Un étudiant ne voit que ses réclamations (GET /api/complaints/my) | Droits | Haute |
| EX-10 | Seul l'administrateur voit toutes les réclamations (GET /api/complaints) et change les statuts (PUT /api/complaints/:id) | Droits | Haute |
| EX-11 | La recherche retrouve les réclamations par mot de la description ou par catégorie | Recherche | Moyenne |
| EX-12 | Le filtre par statut et le tri donnent des résultats corrects | Recherche | Moyenne |
| EX-13 | Les compteurs du tableau de bord correspondent aux réclamations réelles | Suivi | Moyenne |
| EX-14 | Les données restent enregistrées après redémarrage du serveur | Persistance | Haute |
| EX-15 | L'API répond en JSON avec des codes HTTP cohérents (200, 201, 400, 401, 403, 404) | API REST | Moyenne |
| EX-16 | L'image jointe est facultative, en JPG, PNG ou GIF, de 5 Mo maximum ; le reste est refusé | Réclamations | Moyenne |
| EX-17 | Les pages principales répondent dans un délai acceptable sous charge légère (en local) | Non fonctionnel | Basse |
| EX-18 | La priorité choisie est enregistrée et affichée sur la réclamation | Réclamations | Moyenne |
| EX-19 | Le temps de résolution estimé dépend de la catégorie et de la priorité | Réclamations | Basse |

## 2. Ambiguïtés

| N° | Exigence | Ambiguïté | État | Décision |
|---|---|---|---|---|
| A1 | EX-04 | Champ titre et limites de la description | **Résolue** | Pas de titre ; description de 10 à 1 000 (interface) |
| A2 | EX-06 | Statut final, statut Clôturée | **Résolue** | Trois statuts ; Resolved est final |
| A3 | EX-08 | Que veut dire « modifier » ? | Ouverte | Le README ne cite aucune route de modification de la description. Si elle n'existe pas, EX-08 se limite au changement de statut |
| A4 | EX-10 | L'admin peut-il revenir en arrière sur un statut ? | Ouverte | Non : mêmes transitions pour tous |
| A5 | EX-09 | Suppression d'une réclamation | **Résolue (README)** | Aucune route de suppression : hors périmètre |
| A6 | EX-12 | Options de tri disponibles | Ouverte | À relever dans l'interface |
| A7 | EX-17 | « Délai acceptable » | Ouverte | Proposition : 95 % des réponses en moins de 500 ms avec 20 utilisateurs virtuels |
| A8 | EX-02 | Blocage après échecs de connexion | Ouverte | Hors exigence ; observation de sécurité |
| A9 | EX-03 | Le rôle admin à l'inscription est-il voulu ? | Ouverte | Le README le décrit comme un usage normal. À soumettre à l'encadrant : si c'est volontaire, noter le risque comme résiduel |

## 3. Critères d'acceptation

Format : **Étant donné** (situation), **Quand** (action), **Alors** (résultat vérifiable).

**EX-01**
- Étant donné un étudiant inscrit, quand il envoie son email et son mot de passe corrects à POST /api/login, alors il reçoit un jeton (200) et accède à son tableau de bord.

**EX-02**
- Étant donné un email inconnu ou un mauvais mot de passe, quand l'utilisateur se connecte, alors l'accès est refusé (401) avec un message identique dans les deux cas.

**EX-03**
- Étant donné un email déjà enregistré, quand on s'inscrit avec cet email, alors l'inscription est refusée (400 ou 409).
- Étant donné une inscription avec le rôle admin, quand la requête est envoyée sans autorisation, alors le compte n'est pas créé avec ce rôle **[À CONFIRMER]**.

**EX-04**
- Description de 9 caractères : refusée (400). 10 : acceptée. 1 000 : acceptée. 1 001 : refusée. Vide ou espaces seuls : refusée.
- Catégorie ou priorité absente ou inexistante (requête API) : refusée.
- Les mêmes règles s'appliquent quand la requête est envoyée directement à l'API, sans passer par l'interface.

**EX-05**
- Quand une réclamation valide est créée (POST /api/complaints, 201), alors son statut est Pending.

**EX-06 et EX-07**
- PUT /api/complaints/:id par l'admin : Pending vers In Progress acceptée ; In Progress vers Resolved acceptée.
- Pending vers Resolved, In Progress vers Pending, Resolved vers Pending, Resolved vers In Progress, ou un statut inexistant : refusés (400 ou 409), statut inchangé.

**EX-08**
- Quand un étudiant tente de modifier une réclamation Resolved, alors la modification est refusée et les données restent inchangées.

**EX-09**
- Étant donné les étudiants U1 et U2, quand U2 appelle GET /api/complaints/my, alors la réponse ne contient aucune réclamation de U1.

**EX-10**
- Quand un étudiant appelle GET /api/complaints ou PUT /api/complaints/:id, alors la réponse est 403 et rien n'est modifié.
- Quand l'admin appelle GET /api/complaints, alors il voit les réclamations de tous les étudiants.

**EX-11 et EX-12**
- Étant donné 3 réclamations connues, quand on cherche un mot présent dans une seule description, alors seule celle-ci est retournée.
- Quand on filtre sur Pending, toutes les lignes affichées sont Pending.

**EX-13**
- Total = Pending + In Progress + Resolved, et chaque compteur égale le nombre de lignes du filtre associé.

**EX-14**
- Une réclamation créée est retrouvée avec les mêmes valeurs après redémarrage du serveur.

**EX-15**
- Sans jeton : 401. Jeton invalide : 401. Ressource inexistante : 404. Données invalides : 400. Réponses en JSON, sans trace technique.

**EX-16**
- Image de 5 Mo exactement : acceptée. 5 Mo + 1 octet : refusée. Format .exe, .pdf ou fichier renommé en .jpg : refusé.

**EX-17**
- Avec 20 utilisateurs virtuels pendant 1 minute, 95 % des requêtes de la liste répondent en moins de 500 ms, erreurs inférieures à 1 %.

**EX-18**
- Pour chaque priorité de la liste, la valeur affichée sur la carte est celle envoyée.

**EX-19**
- Quand la catégorie ou la priorité change dans le formulaire, l'estimation affichée change en conséquence.

## 4. Validation
Exigences relues avec l'encadrant : [date] | Modifications demandées : [à compléter]
