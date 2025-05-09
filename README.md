# Documentation de l'Infrastructure Terraform pour GreenShop sur AWS

L’ensemble des quatre scripts Terraform constitue une infrastructure complète et cohérente pour déployer l’application GreenShop sur AWS.

## Script 1 : Fondation de l'Architecture

- **VPC Personnalisé** : Ce script crée un VPC personnalisé, point de départ fondamental pour isoler les ressources réseau.
- **Gestion des Clés SSH** : Il prépare également la gestion des clés SSH (bien que commentée), assurant un accès sécurisé aux instances.

## Script 2 : Configuration des Groupes de Sécurité

- **Groupes de Sécurité** : Ce script définit les groupes de sécurité, c’est-à-dire les règles de pare-feu, pour chaque composant :
  - Load Balancer
  - Bastion
  - Instances applicatives
  - Base de données RDS
- **Contrôle du Trafic** : Ces groupes permettent de contrôler finement le trafic entrant et sortant entre les ressources selon les ports et protocoles autorisés.

## Script 3 : Ressources de Calcul et d'Équilibrage de Charge

- **Application Load Balancer (ALB)** : Création d'un ALB pour distribuer le trafic.
- **Instances EC2 Privées** : Trois instances EC2 privées pour héberger l’application GreenShop.
- **Instance Bastion Publique** : Une instance bastion publique permettant d’y accéder en SSH de façon sécurisée.
  - **Sous-réseau Public** : Le bastion est déployé dans un sous-réseau public.
  - **Sous-réseau Privé** : Les instances applicatives résident dans un sous-réseau privé, ce qui renforce la sécurité.
- **Groupe de Cibles et Listener** : Configuration d'un groupe de cibles (Target Group) et d'un listener pour que le Load Balancer puisse distribuer le trafic HTTP aux instances EC2.
- **Instance RDS MariaDB** : Une instance RDS MariaDB, hébergée dans des sous-réseaux privés, avec une sécurité et une isolation renforcées.

## Script 4 : Structure Réseau Détaillée

- **Sous-réseaux Publics et Privés** : Définition des sous-réseaux publics et privés.
- **Zones de Disponibilité (AZ)** : Répartition sur deux zones de disponibilité (AZ) pour assurer la tolérance aux pannes.
- **Tables de Routage** : Configuration des tables de routage pour permettre aux sous-réseaux publics d'accéder à Internet via une Internet Gateway.
- **Accès Sortant vers Internet** : Permet aux ressources privées d’avoir un accès sortant vers Internet.

## Conclusion

Cette infrastructure Terraform est conçue pour déployer l'application GreenShop sur AWS de manière sécurisée, scalable et tolérante aux pannes. Chaque script joue un rôle spécifique dans la configuration et le déploiement des ressources nécessaires.
