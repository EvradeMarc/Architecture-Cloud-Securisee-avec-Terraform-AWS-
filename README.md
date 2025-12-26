# 🛡️ Architecture Cloud Sécurisée avec Terraform (AWS)

## 📌 Présentation du projet

Ce projet consiste à concevoir et déployer une **architecture réseau virtuelle sécurisée** sur **AWS**, en utilisant **Terraform** comme outil d’Infrastructure as Code (IaC).

L’objectif est de reproduire, dans un environnement Cloud, les notions vues en virtualisation classique (Hyper-V / VM) :
- segmentation réseau,
- routage,
- filtrage via ACL,
- services distribués,
- haute disponibilité applicative.

Le projet met en œuvre une **architecture client / serveur** avec un **bastion SSH**, un **load balancer HAProxy**, plusieurs **serveurs web** et un **serveur FTP**, le tout **sécurisé par des règles strictes**.

---

## 🎯 Objectifs pédagogiques

- Comprendre la **virtualisation réseau dans le Cloud**
- Mettre en place une **architecture multi-réseaux**
- Implémenter un **bastion SSH sécurisé**
- Déployer un **load balancer applicatif (HAProxy)**
- Sécuriser les flux via des **ACL (Security Groups AWS)**
- Automatiser le déploiement avec **Terraform**
- Tester et valider le bon fonctionnement des services

---

## 🏗️ Architecture globale

### 🔹 Vue d’ensemble
<img width="861" height="812" alt="image" src="https://github.com/user-attachments/assets/1242f7b7-dfa8-4653-bd56-dc6834fcda08" />


### 🔹 Composants

| Composant | Rôle |
|---------|-----|
| Bastion SSH | Point d’entrée sécurisé |
| HAProxy | Répartition de charge HTTP |
| Web 1 / Web 2 | Serveurs web Apache |
| FTP | Partage de fichiers |
| Terraform | Déploiement automatisé |
| AWS EC2 | Machines virtuelles |
| VPC / Subnets | Segmentation réseau |

---

## 🌐 Architecture réseau

### 🔹 Réseaux

| Réseau | CIDR | Description |
|------|------|------------|
| VPC | `10.0.0.0/16` | Réseau principal |
| Subnet Client | `10.0.1.0/24` | Bastion SSH, HAProxy|
| Subnet Serveur | `10.0.2.0/24` | Services internes |

### 🔹 Flux autorisés (ACL)

| Source | Destination | Port | Autorisé |
|-----|------------|-----|---------|
| Internet | Bastion | 22 | ✅ |
| Bastion | Serveurs | 22 | ✅ |
| Internet | HAProxy | 80 | ✅ |
| HAProxy | Web | 80 | ✅ |
| Web | FTP | 21 | ✅ |
| Internet | Web | 80 | ❌ |
| Internet | FTP | 21 | ❌ |

---

## 🔐 Sécurité mise en place

- **Aucun accès SSH direct aux serveurs**
- Bastion SSH comme **point d’entrée unique**
- Security Groups utilisés comme **ACL stateful**
- Serveurs web accessibles **uniquement via HAProxy**
- FTP accessible uniquement depuis le réseau serveur

---

## ⚙️ Technologies utilisées

| Technologie | Usage |
|-----------|------|
| AWS | Infrastructure Cloud |
| Terraform | Infrastructure as Code |
| EC2 | Machines virtuelles |
| VPC | Réseau virtuel |
| HAProxy | Load balancing HTTP |
| Apache | Serveur Web |
| vsftpd | Serveur FTP |
| Amazon Linux | OS |

---

## 📁 Structure du projet
 ├── provider.tf
 ├── variables.tf
 ├── network.tf
 ├── security.tf
 ├── instances.tf
 ├── outputs.tf
 ├── haproxy.sh
 ├── web.sh
 └── ftp.sh

 
---

## 🚀 Déploiement de l'architecture

### 🔹 Prérequis

- Compte AWS
- Terraform ≥ 1.5
- Clé SSH AWS : EC2 -> Key pairs -> Create key pair
- AWS CLI configuré : configuration des credentials avec la commande `aws configure` (Utiliser les Access Key : profile -> security credential -> create access key)

### 🔹 Étapes

```bash
terraform init
terraform plan
terraform apply



