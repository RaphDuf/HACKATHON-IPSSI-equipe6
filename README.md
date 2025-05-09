# HACKATHON-IPSSI-equipe6
## main.tf

Ce module Terraform commence par la déclaration du **provider AWS**, en spécifiant la région d’hébergement des ressources (ici, `us-east-1`). Il crée ensuite un **Virtual Private Cloud (VPC)**, élément central du réseau dans AWS. Ce VPC utilise la plage d’adresses IP privée `10.0.0.0/16`, avec l’activation du support **DNS** et des **noms d’hôtes**, ce qui est essentiel pour la résolution de noms au sein du réseau. Le VPC est également tagué pour une meilleure lisibilité dans la console AWS (`Name = "main-vpc"`).

La section commentée du script (non active mais prête à l’usage) permet de générer automatiquement une **paire de clés SSH RSA 4096 bits** à l’aide de Terraform et du provider `tls`. Cette clé permettrait un accès sécurisé aux futures instances EC2. La **clé publique** est alors importée dans AWS sous forme d’un `aws_key_pair` nommé `connexion`. La **clé privée**, quant à elle, est enregistrée localement dans un fichier `.pem`, avec des **permissions strictes** pour garantir la sécurité.

Pour générer une clé SSH et l'utiliser pour se connecter à une instance EC2 dans AWS, commencez par vous connecter à la Console AWS. Dans la section EC2, allez dans Key Pairs sous Network & Security et cliquez sur Create key pair. Donnez un nom à votre clé, choisissez le type RSA et la taille de la clé (2048 ou 4096 bits), puis téléchargez le fichier de clé privée .pem généré. Cette clé sera utilisée pour vous connecter en SSH à vos instances EC2.   Pour gérer vos ressources AWS via la ligne de commande, installez AWS CLI et configurez-le avec aws configure. Lors de la configuration, entrez votre AWS Access Key ID, AWS Secret Access Key que vous allez trouver dans Vous pouvez installer AWS CLI via pip, puis le configurer avec vos identifiants AWS, disponibles dans la section "AWS details" de votre compte AW, la région et le format de sortie. Une fois configuré, vous pouvez l'utiliser pour gérer vos instances EC2

## Instances.tf

Ce script Terraform déploie l’infrastructure complète de l’application **GreenShop** sur AWS en automatisant plusieurs composants clés :

- Il commence par la création d’un **Load Balancer de type application (ALB)**, placé dans deux **sous-réseaux publics** pour assurer la haute disponibilité, et associé à un **groupe de sécurité** autorisant le trafic web.

- Ensuite, il instancie une **machine bastion EC2** dans un sous-réseau public, permettant aux administrateurs d'accéder aux instances privées via **SSH**, avec **copie de fichiers** via le provisioner `file` et exécution d’un **script d’initialisation** (`userdata.sh`).

- Trois **instances EC2 privées** hébergeant l'application sont ensuite déployées dans un **sous-réseau privé**, sans adresse IP publique, pour des raisons de sécurité.

- Ces instances sont rattachées à un **groupe cible (Target Group)** afin que le Load Balancer puisse répartir le **trafic HTTP** entre elles.

- Le script configure également un **écouteur (listener)** sur le **port 80** pour rediriger les requêtes vers ces instances via le groupe cible.

- Enfin, une **base de données MariaDB managée (RDS)** est provisionnée dans un sous-réseau dédié, avec :
  - Nom de la base,
  - Identifiants,
  - Type de moteur,
  - Paramètres de sécurité.

L’ensemble constitue une **architecture cloud scalable, sécurisée et automatisée** pour accueillir l'application **GreenShop**.

## infra_reseau.tf

Ce script Terraform met en place l’infrastructure réseau de base dans **AWS** pour héberger l’application **GreenShop**.

- Il commence par la création de **trois sous-réseaux publics** (un pour le bastion et deux pour le Load Balancer), répartis sur **deux zones de disponibilité** afin de garantir une **haute disponibilité**.

- Ensuite, il crée **deux sous-réseaux privés** qui accueilleront les **instances EC2** de l'application et la **base de données**, également répartis sur deux zones de disponibilité.

- Une **passerelle Internet (Internet Gateway)** est attachée au **VPC** pour permettre aux sous-réseaux publics d'accéder à Internet, accompagnée d’une **table de routage** pour rediriger le trafic externe.

- Chaque sous-réseau public est ensuite **associé à cette table de routage**.

- Pour permettre aux ressources privées (comme les EC2 d’application ou la base de données) d’accéder à Internet **sans être exposées directement**, le script crée :
  - une **Elastic IP**,
  - un **NAT Gateway**,
  - une **table de routage privée** associée aux sous-réseaux privés.

- Enfin, un **DB Subnet Group** est défini pour le service **RDS**, requis pour le bon déploiement de la base de données dans un environnement **multi-AZ sécurisé**.

L'ensemble garantit une **architecture réseau bien segmentée, sécurisée et évolutive**.

## Liens entre les 4 fichiers Terraform

Les fichiers Terraform `main.tf`, `Security_group.tf`, `Instances.tf` et `infra_reseau.tf` créent une infrastructure Cloud sécurisée sur AWS pour l'application **GreenShop**, avec une architecture hautement disponible et segmentée.

- **`main.tf`** : Déclare le **VPC** (réseau privé) et configure la base SSH pour un accès sécurisé. Ce fichier sert de point d’entrée principal pour la création des ressources dans AWS.

- **`infra_reseau.tf`** : Déploie la **topologie réseau**, créant des sous-réseaux publics (pour le bastion et le Load Balancer) et privés (pour les instances et la base de données), avec des tables de routage pour un accès sécurisé via **Internet Gateway** et **NAT Gateway**.

- **`Security_group.tf`** : Configure les **groupes de sécurité** :
  - Bastion : **SSH (port 22)** depuis une IP précise.
  - Load Balancer : **HTTP (port 80)** depuis l’extérieur.
  - Instances privées : Connexions **internes** uniquement.
  - Base de données : Connexions **MySQL (port 3306)** depuis les instances applicatives.

- **`Instances.tf`** : Déploie les **ressources de calcul** : un **bastion** pour accéder aux instances privées, un **ALB** pour gérer le trafic HTTP vers 3 instances EC2 privées, et une **base de données RDS MariaDB** dans les sous-réseaux privés. Utilisation d’un **Target Group** et **Listener** pour la répartition du trafic.

---

### En résumé

Ces fichiers construisent une **infrastructure AWS sécurisée**, avec un **trafic filtré**, des **ressources privées** et une **haute disponibilité**.




