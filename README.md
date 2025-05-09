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
