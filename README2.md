# Documentation du déploiement Ansible

## Introduction

Cette documentation décrit le processus d'automatisation du déploiement d'une application web sur des instances Amazon EC2 en utilisant Ansible. Elle détaille les fichiers de configuration nécessaires, leur fonctionnement, et les étapes de déploiement.

## Prérequis

- **Infrastructure fonctionnelle** comme présenté dans le README.md
- **Ansible** : Installer Ansible sur votre machine bastion. [Documentation Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- **Récupérer le point de terminaison de la base de données**

## Fonctionnement du déploiement

Le déploiement fonctionne comme suit :

- **Inventaire Dynamique** : Utilisation du fichier `aws_ec2.yml` pour interroger l'API AWS et récupérer dynamiquement les instances EC2 correspondant à des critères spécifiques (tag Name).
- **Installation de Docker** : Le fichier `docker.yml` installe Docker sur les instances Ubuntu, permettant de gérer des conteneurs.
- **Configuration du Serveur LAMP** : Le fichier `apache-php.yml` installe et configure Apache, PHP, et MariaDB pour exécuter l'application web.
- **Importation de la Base de Données** : Le fichier `database.yml` permet d'importer un dump SQL dans une base de données sur Amazon RDS.

## Mise en place du déploiement

1. **Exécuter les Playbooks Ansible** :

    - Utilisez les commandes suivantes pour déployer l'application :

    ```bash
    ansible-playbook -i /home/ubuntu/ansible/aws_ec2.yml /home/ubuntu/ansible/docker.yml --private-key=/home/ubuntu/.ssh/connexion.pem --ssh-extra-args="-o StrictHostKeyChecking=no"
    ```

Remplacer les informations en liens avec la base de données dans les fichiers /home/ubuntu/ansible/greenshop/db.php et /home/ubuntu/ansible/database.yml

    ```bash
    ansible-playbook -i /home/ubuntu/ansible/aws_ec2.yml /home/ubuntu/ansible/apache-php.yml --private-key=/home/ubuntu/.ssh/connexion.pem --ssh-extra-args="-o StrictHostKeyChecking=no"
    ansible-playbook -i localhost /home/ubuntu/ansible/database.yml
    ```
    
2. **Vérification** : Copiez le DNS du load balancer et vérifiez que l'application est accessible via le navigateur et que tous les services sont opérationnels.
