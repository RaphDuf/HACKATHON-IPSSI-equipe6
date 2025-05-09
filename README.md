# HACKATHON-IPSSI-equipe6

## Déploiement de l'infrastructure 
L'objectif est de provisionner l'infrastructure cloud sur AWS en incluant un VPC, les instances EC2 nécessaires, un load balancer et une base de données. 
Pour cela, nous utilisons Terraform. Pour plus de détails, se rendre dans la branche [infrastructure](https://github.com/RaphDuf/HACKATHON-IPSSI-equipe6/tree/infrastructure).

## Déploiement de l'application
L'application et la configuration des instances EC2 sont automatiquement déployées avec Ansible. Pour plus de détails, se rendre sur la branche [infrastructure](https://github.com/RaphDuf/HACKATHON-IPSSI-equipe6/tree/infrastructure).

## Conteneurisation de l'application 
L'application fournie par le client a été conteneurisée pour simplifier le déploiement et sa gestion sur le cloud. Cette conteneurisation repose sur Docker. Pour plus de détails, se rendre sur la branche [conteneur](https://github.com/RaphDuf/HACKATHON-IPSSI-equipe6/tree/conteneur).

## Pipeline CI/CD
Une pipeline de déploiement continu (CI/CD) est mise en place en utilisant Jenkins. Pour chaque modification du code source sur le dépôt GitHub, une nouvelle image est créée sur Docker Hub. De plus, les webhooks GitHub permettent de déclencher le processus de build et de déploiement sur Jenkins. Pour plus de détails, se rendre sur la branche [pipeline-ci-cd](https://github.com/RaphDuf/HACKATHON-IPSSI-equipe6/tree/pipeline-ci-cd).
