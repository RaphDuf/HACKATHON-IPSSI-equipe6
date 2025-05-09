# Documentation du Jenkinsfile

Le fichier `Jenkinsfile` décrit une pipeline CI/CD pour un projet utilisant Jenkins.

## Les prérequis sont les suivants :

1. Avoir une machine Jenkins accessible depuis le dépôt GitHub et prête à l'emploi (la machine utilise le port 8080 par défaut pour l'interface web). [Documentation Jenkins](https://www.jenkins.io/doc/book/installing/)
2. Docker doit également être installé sur la machine Jenkins pour exécuter les commandes de conteneurisation. [Documentation Docker](https://docs.docker.com/get-started/get-docker/)

## La pipeline fonctionne comme suit :

1. Dans un premier temps, elle clone le dépôt GitHub sur lequel le code est hébergé.
2. Puis, elle construit une image Docker à partir du Dockerfile.
3. Elle exécute l'image Docker construite précédemment.
4. Elle pousse l'image vers un registre Docker avec le tag `latest` et un numéro de version afin de garantir le versionnage de l'image.

## Mise en place de la pipeline :

1. Créer un dépôt GitHub public ainsi qu'un registre sur Docker Hub avec des noms identiques (ici, le nom est HACKATHON-IPSSI-equipe6).
2. Ajouter les identifiants Docker Hub dans Jenkins (Administrer Jenkins → Credentials → Global), car l'ID est réutilisé dans le Jenkinsfile pour se connecter au compte Docker Hub.
3. Créer un nouvel item Pipeline sur Jenkins (le nom est au choix) et renseigner les informations suivantes durant la création :
   - Description (optionnelle)
   - Dans le champ Triggers, sélectionner "GitHub hook trigger for GITScm polling"
   - Dans le champ Pipeline, sélectionner "Pipeline script from SCM" puis sélectionner Git en tant que SCM.
   - Dans le champ Repository URL, indiquer l'URL du repository GitHub.
   - Spécifier la branche (ici, par exemple, il s'agit de la branche pipeline-ci-cd).
4. Dans GitHub, se rendre dans le repository, puis dans Settings, sélectionner l'onglet Webhooks et ajouter un webhook.  
   Mettre l’IP de la machine Jenkins : `http://<ip-machine>:8080/github-webhook/` dans le champ Payload URL.  
   Sélectionner le type d’événement qui déclenchera la pipeline dans le champ "Which events would you like to trigger this webhook?"
5. Lancer la pipeline manuellement ou en déclenchant un événement.
