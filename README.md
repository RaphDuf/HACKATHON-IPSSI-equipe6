# HACKATHON-IPSSI-equipe6
## main.tf

Ce module Terraform commence par la déclaration du **provider AWS**, en spécifiant la région d’hébergement des ressources (ici, `us-east-1`). Il crée ensuite un **Virtual Private Cloud (VPC)**, élément central du réseau dans AWS. Ce VPC utilise la plage d’adresses IP privée `10.0.0.0/16`, avec l’activation du support **DNS** et des **noms d’hôtes**, ce qui est essentiel pour la résolution de noms au sein du réseau. Le VPC est également tagué pour une meilleure lisibilité dans la console AWS (`Name = "main-vpc"`).

La section commentée du script (non active mais prête à l’usage) permet de générer automatiquement une **paire de clés SSH RSA 4096 bits** à l’aide de Terraform et du provider `tls`. Cette clé permettrait un accès sécurisé aux futures instances EC2. La **clé publique** est alors importée dans AWS sous forme d’un `aws_key_pair` nommé `connexion`. La **clé privée**, quant à elle, est enregistrée localement dans un fichier `.pem`, avec des **permissions strictes** pour garantir la sécurité.

Ce mécanisme est pratique pour **automatiser la génération et la distribution des clés** sans intervention manuelle.
