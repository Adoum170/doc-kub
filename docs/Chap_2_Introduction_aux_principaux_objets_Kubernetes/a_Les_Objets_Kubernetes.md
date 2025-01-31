---
title: Les Objets Kubernetes
tags:
  - "#Principaux_Objet/Fonctionnement_Général_Objet"
---

#### **Fonctionnement général des objets Kubernetes**

Les **objets Kubernetes** sont des entités persistantes utilisées pour représenter l’état de votre cluster. Ils servent à définir l’état désiré du système, et Kubernetes s’efforce constamment de faire correspondre l’état actuel à cet état désiré.

- **Définition :**  
    Un objet Kubernetes est un **"enregistrement d'intention"**. Une fois un objet créé, Kubernetes travaille automatiquement pour le maintenir en place.

- **Exemples d'état désiré :**
  - Faire fonctionner un conteneur (via des Pods).
  - Gérer l’accès réseau aux applications (via des Services).
  - Maintenir un certain nombre de réplicas en cours d’exécution (via des Deployments).
- **Interaction avec les objets :** Les objets sont créés, modifiés ou supprimés en utilisant l’**API Kubernetes**. Les outils comme **kubectl** servent d’interface avec cette API.

---

#### **Principaux objets Kubernetes**

Voici une liste des objets fondamentaux que vous rencontrerez et utiliserez régulièrement :

1. **Pod** :
    - Représente l'unité de base d'exécution dans Kubernetes.
    - Contient un ou plusieurs conteneurs partageant un même réseau et un espace de stockage.
2. **Service** :
    - Abstraction définissant un ensemble logique de Pods.
    - Fournit des mécanismes pour la découverte de services et l'équilibrage de charge.
3. **Volume** :
    - Abstraction pour le stockage des données des conteneurs.
    - Persiste même en cas de redémarrage des conteneurs.
4. **Namespace** :
    - Permet de diviser un cluster Kubernetes en plusieurs clusters virtuels isolés.
    - Utile pour organiser les ressources ou gérer des environnements multiples (dev, test, prod).
5. **Deployment** :
    - Gère les déploiements d'applications sans état.
    - Facilite les mises à jour, l’évolutivité et la gestion des réplicas de Pods.
6. **StatefulSet** :

    - Gère les applications avec état (comme les bases de données).
    - Maintient une identité unique pour chaque Pod.
7. **DaemonSet** :
    - Assure qu’un Pod spécifique est en cours d’exécution sur tous (ou certains) nœuds d’un cluster.
    - Utilisé pour des tâches comme la journalisation ou la supervision.
8. **Job** :
    - Représente une tâche ponctuelle exécutée jusqu’à son achèvement.
9. **CronJob** :
    - Programme l’exécution de tâches répétées à des intervalles définis.

---

#### **Spécification et statut des objets Kubernetes**

Tous les objets Kubernetes comportent deux champs principaux :

1. **Spec (Specification)** :
    - Définit les caractéristiques et l’état souhaité de l’objet.
    - Par exemple : nombre de réplicas pour un Deployment.
2. **Status (Statut)** :
    - Décrit l’état actuel de l’objet.
    - Mis à jour automatiquement par le système Kubernetes pour refléter l’état réel.

##### **Exemple : Le Deployment**

- **Spec (État souhaité)** :
  - Vous configurez un Deployment pour qu’il exécute trois réplicas d’une application.
- **Status (État actuel)** :
  - Si l’un des Pods échoue, Kubernetes détecte la différence entre l’état souhaité (3 Pods) et l’état actuel (2 Pods). Il démarre alors une nouvelle instance pour rétablir la conformité avec la spécification.

---

#### **Résumé**

- **Objets Kubernetes :** Entités persistantes utilisées pour représenter et gérer l’état du cluster.
- **Principaux objets :** Pods, Services, Deployments, StatefulSets, Volumes, etc.
- **Spécification vs Statut :**
  - La **spec** déclare l’état souhaité.
  - Le **status** reflète l’état réel, et Kubernetes travaille constamment à réduire l’écart entre les deux.

Kubernetes automatise ainsi la gestion des ressources, permettant de maintenir un état stable même en cas de défaillance ou de changement.
