# Documentation du Jenkinsfile

Le fichier `Jenkinsfile` décrit une pipeline CI/CD pour un projet utilisant Jenkins.

## Les prérequis sont les suivants :

1. Avoir une machine Jenkins accessible depuis le dépôt GitHub et prête à l'emploi (la machine utilise le port 8080 par défaut pour l'interface web). [Documentation Jenkins](https://www.jenkins.io/doc/book/installing/)
2. Docker doit aussi être installé sur la machine Jenkins pour exécuter les commandes de conteneurisation. [Documentation Docker](https://docs.docker.com/get-started/get-docker/)


## La pipeline fonctionne comme suit :

1. Dans un premier temps, elle clone le dépôt GitHub sur lequel le code est hébergé.
2. Puis elle construit une image Docker à partir du Dockerfile.
3. Elle exécute l'image Docker construite précédemment.
4. Elle pousse l'image vers un registre Docker avec le tag `latest` et un numéro de version afin de garantir le versionnage de l'image.
