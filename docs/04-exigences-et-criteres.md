# Exigences, ambiguïtés et critères d'acceptation : HostelOps

Version 1.0 | Semaine 3
Les points marqués **[À VÉRIFIER]** sont des hypothèses à confirmer en explorant l'application. Terminologie : Pending, In Progress, Resolved sont les statuts observés.

## 1. Liste des exigences

| ID | Exigence | Module | Priorité |
|---|---|---|---|
| EX-01 | Un utilisateur inscrit peut se connecter avec un email et un mot de passe valides | Authentification | Haute |
| EX-02 | La connexion est refusée avec des identifiants invalides, sans révéler lequel est faux | Authentification | Haute |
| EX-03 | L'inscription refuse un email déjà utilisé, et l'utilisateur ne peut pas choisir le rôle admin | Utilisateurs | Haute |
| EX-04 | Une réclamation exige une catégorie et une description (10 à 1 000 caractères) **[À VÉRIFIER]** | Réclamations | Haute |
| EX-05 | Une réclamation est créée avec le statut Pending et une date de création | Réclamations | Haute |
| EX-06 | Les transitions autorisées sont : Pending → In Progress → Resolved | Suivi | Haute |
| EX-07 | Toute autre transition est refusée (ex. Pending → Resolved, Resolved → Pending) | Suivi | Haute |
| EX-08 | Une réclamation Resolved ne peut plus être modifiée par un étudiant | Réclamations | Haute |
| EX-09 | Un étudiant ne peut consulter que ses propres réclamations | Droits | Haute |
| EX-10 | Seul l'administrateur change les statuts et voit toutes les réclamations | Droits | Haute |
| EX-11 | La recherche retrouve les réclamations par mot de la description ou par catégorie | Recherche | Moyenne |
| EX-12 | Le filtre par statut et le tri (plus récent en premier) donnent des résultats corrects | Recherche | Moyenne |
| EX-13 | Les compteurs du tableau de bord correspondent aux réclamations réelles | Suivi | Moyenne |
| EX-14 | Les données restent enregistrées après redémarrage du serveur | Persistance | Haute |
| EX-15 | L'API renvoie des codes HTTP cohérents (200, 201, 400, 401, 403, 404) et du JSON | API REST | Moyenne |
| EX-16 | Le téléversement de fichier refuse les types interdits et les fichiers trop gros **[À VÉRIFIER]** | Réclamations | Moyenne |
| EX-17 | Les pages principales répondent dans un délai acceptable sous charge légère | Non fonctionnel | Basse |

## 2. Ambiguïtés relevées

Une exigence ambiguë ne peut pas être testée. Chaque point ci-dessous est une question à trancher.

| N° | Exigence | Ambiguïté | Décision proposée |
|---|---|---|---|
| A1 | EX-04 | Y a-t-il un champ titre ? Quelles sont les limites réelles de la description ? | Tester l'application. Sans titre, appliquer 10 à 1 000 caractères à la description. |
| A2 | EX-06 | Le statut Resolved est-il final ? Existe-t-il un statut Clôturée ? | Considérer Resolved comme l'état final. Documenter l'écart avec le sujet. |
| A3 | EX-08 | « Modifier » veut-il dire changer la description, la catégorie, ou ajouter un fichier ? | Toute modification des champs de la réclamation par l'étudiant. |
| A4 | EX-10 | L'admin peut-il revenir en arrière sur un statut ? | Non, mêmes transitions que pour tous (EX-06 et EX-07). |
| A5 | EX-09 | Un étudiant peut-il supprimer sa réclamation ? Que se passe-t-il alors ? | À tester ; à noter comme comportement constaté. |
| A6 | EX-12 | Quels sont les critères de tri disponibles et leur ordre exact ? | Relever les options de la liste « Sort By ». |
| A7 | EX-17 | Qu'est-ce qu'un « délai acceptable » ? | Proposition : 95 % des réponses en moins de 500 ms avec 20 utilisateurs virtuels. |
| A8 | EX-02 | Y a-t-il un blocage après plusieurs échecs de connexion ? | Hors exigence ; à signaler comme observation de sécurité. |

## 3. Critères d'acceptation

Format : **Étant donné** (situation de départ), **Quand** (action), **Alors** (résultat vérifiable).

**EX-01 Connexion valide**
- Étant donné un étudiant inscrit, quand il envoie son email et son mot de passe corrects, alors il reçoit un jeton et accède à son tableau de bord (réponse 200).

**EX-02 Connexion invalide**
- Étant donné un email inconnu ou un mauvais mot de passe, quand l'utilisateur se connecte, alors l'accès est refusé (401) avec un message identique dans les deux cas.

**EX-03 Inscription**
- Étant donné un email déjà enregistré, quand on s'inscrit avec cet email, alors l'inscription est refusée (400 ou 409).
- Étant donné une inscription avec le rôle admin dans la requête, quand elle est envoyée, alors le compte créé a le rôle étudiant ou la requête est refusée.

**EX-04 Validation de la description** (si 10 à 1 000 est confirmé)
- Description de 9 caractères : refusée (400). 10 caractères : acceptée. 1 000 : acceptée. 1 001 : refusée.
- Description vide ou composée d'espaces : refusée.
- Catégorie absente : refusée.

**EX-05 Statut initial**
- Étant donné une réclamation valide, quand elle est créée, alors son statut est Pending et sa date de création est renseignée.

**EX-06 et EX-07 Transitions**
- Pending vers In Progress par l'admin : acceptée. In Progress vers Resolved : acceptée.
- Pending vers Resolved, Resolved vers Pending, Resolved vers In Progress : refusées (400 ou 409), le statut reste inchangé.

**EX-08 Réclamation résolue**
- Étant donné une réclamation Resolved, quand son propriétaire tente de la modifier, alors la modification est refusée (403 ou 400) et les données restent inchangées.

**EX-09 Isolation des données**
- Étant donné les étudiants A et B, quand A demande la réclamation de B par son identifiant, alors la réponse est 403 ou 404 et aucune donnée de B n'apparaît.
- La liste de A ne contient que les réclamations de A.

**EX-10 Droits de l'administrateur**
- Quand un étudiant tente de changer un statut (appel direct à l'API), alors la réponse est 403.
- Quand l'admin consulte la liste, alors il voit les réclamations de tous les étudiants.

**EX-11 et EX-12 Recherche, filtre, tri**
- Étant donné 3 réclamations connues, quand on cherche un mot présent dans une seule description, alors seule celle-ci est retournée.
- Quand on filtre sur Pending, alors toutes les lignes affichées sont Pending et leur nombre égale le compteur Pending.

**EX-13 Compteurs**
- Total = Pending + In Progress + Resolved, et chaque compteur correspond au nombre de lignes du filtre associé.

**EX-14 Persistance**
- Étant donné une réclamation créée, quand le serveur est redémarré, alors elle est retrouvée avec les mêmes valeurs.

**EX-15 Codes HTTP**
- Sans jeton : 401. Avec un jeton invalide ou expiré : 401. Ressource inexistante : 404. Données invalides : 400. Les réponses d'erreur sont en JSON et ne contiennent pas de trace technique.

**EX-16 Téléversement**
- Un fichier d'un type interdit ou dépassant la taille maximale est refusé. La taille et les types autorisés sont à relever dans le code ou le README.

**EX-17 Performance**
- Avec 20 utilisateurs virtuels pendant 1 minute, 95 % des requêtes de la liste répondent en moins de 500 ms et le taux d'erreur est inférieur à 1 %.

## 4. Validation
Exigences relues avec l'encadrant : [date] | Modifications demandées : [à compléter]
