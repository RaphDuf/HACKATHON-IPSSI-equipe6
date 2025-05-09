# HACKATHON-IPSSI-equipe6

## Fichiers de configuration
L'ensemble des fichiers de configurations sont présent dans le dossier 'greenshop'. Ces fichiers proviennent de l'infra de base (VM fournie). 
Ils comportent les différentes page du site et la définitions des actions misent en place dans des fichiers php, le style dans 'style.css' ainsi que les ressources graphiques dans le dossier 'upload'. 
A noter que le fichier db.php a été modifié pour correspondre a l'utilisation de containers, le host à été spécifié manuellement. 

## Dump de la BD
Le dossier 'dump' contient un fichier sql. Ce dernier provient de l'infra de base (VM fournie). 
Pour l'obtenir, nous utilisons la commande suivante : 
```
mysqldump -u root -p greenshop > greenshop_dump.sql
```
Cette commande réalise un dump de la base mariaDB nommé 'greenshop' en utilisant le user 'root' et l'enregistre dans un fichier. 

## Installation requise
Il est nescessaire d'installer Docker Desktop (https://docs.docker.com/get-started/introduction/get-docker-desktop/). 

## Dockerfile
Le Dockerfile contient la définition de l'image de l'application. L'image se base sur l'image officielle php version 8.1.2 avec apache. Elle installe les extensions PHP pour PDO et MySQL permettant de communiquer avec la base de données, copie le dossier 'greenshop' (qui contient les fichiers de configuration), change le propriétaire et les permissions sur /var/www/html pour le user Apache 'www-data', et expose le port 80 (port par défaut d'Apache). 

## Docker-compose
Le docker-compose permet de build l'image défini précédement et l'image mariadb depuis docker-hub. 
Dans le service 'web' on spécifie la redirection de port 80:80 pour Apache. 
Dans le service 'db' on spécifie les variables d'environement avec le mot de passe, le user et le nom de la base, ainsi que la redirection de port 3306:3306 pour mariaDB et on monte un volume local contenant les scripts SQL d'initialisation. 
Pour lancer le build des images, utiliser la commande suivante : 
```
docker compose -d
```
Pour vérifier le status des containers, utiliser la commande : 
```
docker ps
```
