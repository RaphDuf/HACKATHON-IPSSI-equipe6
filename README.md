# HACKATHON-IPSSI-equipe6
## main.tf

Ce module Terraform commence par la déclaration du **provider AWS**, en spécifiant la région d’hébergement des ressources (ici, `us-east-1`). Il crée ensuite un **Virtual Private Cloud (VPC)**, élément central du réseau dans AWS. Ce VPC utilise la plage d’adresses IP privée `10.0.0.0/16`, avec l’activation du support **DNS** et des **noms d’hôtes**, ce qui est essentiel pour la résolution de noms au sein du réseau. Le VPC est également tagué pour une meilleure lisibilité dans la console AWS (`Name = "main-vpc"`).

La section commentée du script (non active mais prête à l’usage) permet de générer automatiquement une **paire de clés SSH RSA 4096 bits** à l’aide de Terraform et du provider `tls`. Cette clé permettrait un accès sécurisé aux futures instances EC2. La **clé publique** est alors importée dans AWS sous forme d’un `aws_key_pair` nommé `connexion`. La **clé privée**, quant à elle, est enregistrée localement dans un fichier `.pem`, avec des **permissions strictes** pour garantir la sécurité.

Ce mécanisme est pratique pour **automatiser la génération et la distribution des clés** sans intervention manuelle.

## Security_group.tf

Ce script Terraform configure plusieurs **groupes de sécurité** essentiels pour protéger et segmenter les différentes couches de l’infrastructure réseau de **GreenShop** déployée sur AWS.

- Le **premier groupe de sécurité**, destiné au **Load Balancer**, autorise les connexions entrantes sur les ports **HTTP (80)** et **HTTPS (443)** depuis n'importe quelle adresse IP, ce qui permet aux utilisateurs finaux d'accéder à l’application via le web. Les connexions sortantes sont également autorisées.

- Le **second groupe**, celui du **bastion host**, permet uniquement les connexions **SSH (port 22)** depuis l'extérieur, offrant un point d'accès sécurisé à l'infrastructure privée pour les administrateurs.

- Le **troisième groupe**, utilisé pour les **instances applicatives privées**, accepte les connexions **SSH, HTTP et HTTPS**, afin de permettre à ces instances de communiquer avec le bastion et le Load Balancer.

- Enfin, le groupe de sécurité pour la **base de données RDS** autorise le trafic entrant sur le **port 3306** (utilisé par MySQL), permettant uniquement les connexions nécessaires à partir des services internes.

Cette configuration assure une **isolation réseau stricte** et un **contrôle fin des flux** entre les différentes ressources cloud.

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
