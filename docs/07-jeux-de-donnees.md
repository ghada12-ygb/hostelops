# Jeux de données de test : HostelOps

Version 1.1
But : refaire les tests avec les mêmes données et obtenir les mêmes résultats. Ne jamais écrire de vrais mots de passe ici : utiliser des mots de passe de test que vous n'utilisez nulle part ailleurs.

## 1. Comptes (à créer en local)

L'inscription se fait par l'interface ou par POST /api/register. Le rôle admin se choisit à l'inscription d'après le README.

| Réf. | Rôle | Email | Mot de passe de test |
|---|---|---|---|
| U1 | Étudiant | u1@test.com | [à choisir] |
| U2 | Étudiant | u2@test.com | [à choisir] |
| ADM | Administrateur | admin@test.com | [à choisir] |

Comptes déjà vus dans l'application : « ghada yaakoubi » et « arij rouja ». Préférer les comptes dédiés ci-dessus.

## 2. Réclamations de départ

| Réf. | Propriétaire | Catégorie | Priorité | Description | Statut cible |
|---|---|---|---|---|---|
| R-A | U1 | Electrical | Medium | Probleme de electricite dans la chambre | Pending |
| R-B | U1 | Plumbing | High | Fuite d'eau dans la salle de bain | In Progress (via ADM) |
| R-C | U1 | [autre catégorie] | Low | Porte de l'armoire cassée à réparer | Resolved (via ADM, après In Progress) |
| R-U2 | U2 | Plumbing | Medium | Robinet qui coule dans le couloir | Pending |

Créer R-A à R-C et R-U2 avec les comptes ci-dessus, puis faire passer R-B et R-C aux statuts voulus avec ADM.

## 3. Chaînes pour les valeurs limites (PowerShell)

```powershell
$d9    = 'a' * 9
$d10   = 'a' * 10
$d999  = 'a' * 999
$d1000 = 'a' * 1000
$d1001 = 'a' * 1001
$d9.Length, $d10.Length, $d1000.Length, $d1001.Length
```
La dernière ligne doit afficher 9, 10, 1000, 1001. Copier une chaîne avec `$d1001 | Set-Clipboard`. Cas supplémentaires : 10 espaces (`' ' * 10`) et description vide.

## 4. Fichiers pour TC-27

```powershell
mkdir donnees-test
fsutil file createnew donnees-test\image-5mo-exact.jpg 5242880
fsutil file createnew donnees-test\image-5mo-plus1.jpg 5242881
fsutil file createnew donnees-test\faux-image.jpg 1000
```
Ces fichiers ont la bonne taille mais ne sont pas de vraies images. Si le serveur les refuse pour cette raison, refaire le test avec une vraie image PNG redimensionnée, et noter la différence. Prévoir aussi un fichier .pdf et un .txt renommé en .jpg. Ne pas ajouter `donnees-test/` à Git.

## 5. Remise à zéro de la base
Fichier : `backend/database.sqlite`.
1. Arrêter le serveur.
2. Copier le fichier sous un autre nom (`database.sqlite.sauvegarde`) **avant** la campagne, avec les comptes et réclamations de départ déjà créés.
3. Pour revenir à l'état initial : remplacer `database.sqlite` par la copie.
4. Relancer le serveur.

La base contient des comptes : ne pas la publier sur GitHub sans l'avoir vérifiée.

## 6. Environnement
| Élément | Valeur |
|---|---|
| Node.js | [résultat de `node -v`] |
| Navigateur | [nom et version] |
| Backend | http://localhost:5000 |
| Frontend | http://localhost:3000 |
| Date des tests | [date] |
