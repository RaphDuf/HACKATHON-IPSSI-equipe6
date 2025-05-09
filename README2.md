# Documentation du Déploiement Ansible

## Introduction

Cette documentation décrit le processus d'automatisation du déploiement d'une application web sur des instances Amazon EC2 en utilisant Ansible. Elle détaille les fichiers de configuration nécessaires, leur fonctionnement, et les étapes de déploiement.

## Prérequis

- **Compte AWS** : Avoir un compte AWS accessible avec des permissions pour gérer EC2.
- **Ansible** : Installer Ansible sur votre machine locale. [Documentation Ansible](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
- **Informations d'identification AWS** : Configurer les informations d'identification AWS dans `~/.aws/credentials` ou via des variables d'environnement.
- **Docker** : Docker doit être installé sur les instances pour exécuter des conteneurs. [Documentation Docker](https://docs.docker.com/engine/install/ubuntu/)

## Fonctionnement du Déploiement

Le déploiement fonctionne comme suit :

- **Inventaire Dynamique** : Utilisation du fichier `aws_ec2.yml` pour interroger l'API AWS et récupérer dynamiquement les instances EC2 correspondant à des critères spécifiques (tag Name).
- **Installation de Docker** : Le fichier `docker.yml` installe Docker sur les instances Ubuntu, permettant de gérer des conteneurs.
- **Configuration du Serveur LAMP** : Le fichier `apache-php.yml` installe et configure Apache, PHP, et MariaDB pour exécuter l'application web.
- **Importation de la Base de Données** : Le fichier `database.yml` permet d'importer un dump SQL dans une base de données sur Amazon RDS.

## Mise en Place du Déploiement

1. **Créer un Dépôt GitHub** : Créer un dépôt public sur GitHub pour héberger vos fichiers de configuration Ansible.
2. **Configurer les Informations d'Identification AWS** : Vérifier que vos informations d'identification AWS sont correctement configurées.
3. **Exécuter les Playbooks Ansible** :
    - Utilisez les commandes suivantes pour déployer l'application :
    ```bash
    ansible-playbook -i inventory aws_ec2.yml
    ansible-playbook -i inventory docker.yml
    ansible-playbook -i inventory apache-php.yml
    ansible-playbook -i inventory database.yml
    ```
4. **Vérification** : Copiez le DNS du load balancer et vérifiez que l'application est accessible via le navigateur et que tous les services sont opérationnels.

## Conclusion

Cette documentation fournit un aperçu complet du processus de déploiement automatisé d'une application web sur AWS EC2 à l'aide d'Ansible. En suivant ces étapes, vous pouvez configurer rapidement et efficacement votre environnement d'application, réduisant ainsi le temps de déploiement et les erreurs humaines.
