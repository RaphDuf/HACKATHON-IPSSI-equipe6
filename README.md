# HACKATHON-IPSSI-equipe6

## Fichiers de configuration
L'ensemble des fichiers de configuration est présent dans le dossier 'greenshop'. Ces fichiers proviennent de l'infrastructure de base (VM fournie).  
Ils comportent les différentes pages du site et la définition des actions mises en place dans des fichiers PHP, le style dans 'style.css' ainsi que les ressources graphiques dans le dossier 'upload'.  
À noter que le fichier db.php a été modifié pour correspondre à l'utilisation de conteneurs : le host a été spécifié manuellement.

## Dump de la BD
Le dossier 'dump' contient un fichier SQL. Ce dernier provient de l'infrastructure de base (VM fournie).  
Pour l'obtenir, nous utilisons la commande suivante :  
```
mysqldump -u root -p greenshop > greenshop_dump.sql
```
Cette commande réalise un dump de la base MariaDB nommée 'greenshop' en utilisant l'utilisateur 'root' et l'enregistre dans un fichier.

## Installation requise
Il est nécessaire d'installer Docker Desktop (https://docs.docker.com/get-started/introduction/get-docker-desktop/).

## Dockerfile
Le Dockerfile contient la définition de l'image de l'application. L'image se base sur l'image officielle PHP version 8.1.2 avec Apache.  
Elle installe les extensions PHP pour PDO et MySQL permettant de communiquer avec la base de données, copie le dossier 'greenshop' (qui contient les fichiers de configuration), change le propriétaire et les permissions sur /var/www/html pour l'utilisateur Apache 'www-data', et expose le port 80 (port par défaut d'Apache).

## Docker-compose
Le docker-compose permet de builder l'image définie précédemment et l'image MariaDB depuis Docker Hub.  
Dans le service 'web', on spécifie la redirection de port 80:80 pour Apache.  
Dans le service 'db', on spécifie les variables d’environnement avec le mot de passe, l'utilisateur et le nom de la base, ainsi que la redirection de port 3306:3306 pour MariaDB, et on monte un volume local contenant les scripts SQL d'initialisation.  

Pour lancer le build des images, utiliser la commande suivante :  
```
docker compose up -d --build
```
Pour vérifier le statut des conteneurs, utiliser la commande :  
```
docker ps
```
Pour supprimer les deux conteneurs créés précédemment, utiliser la commande : 
```
docker compose down
```