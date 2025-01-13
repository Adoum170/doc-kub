---
tags:
  - "#Objet/Namespace"
---
#### **Qu'est-ce qu'un Namespace  ?**

Un **Namespace** est une couche d’isolation logique dans Kubernetes, permettant de **partitionner un cluster** en environnements virtuels distincts.

- **Portée des ressources :**
    - Les noms des ressources dans un Namespace doivent être uniques.
    - Certains objets, comme les **Nodes** ou les **PersistentVolumes**, sont globaux et ne sont pas affectés par les Namespaces.

---

#### **Namespaces par défaut**

Kubernetes crée automatiquement plusieurs Namespaces :

1. **`default`** :
    
    - Namespace par défaut où les ressources sont placées si aucun Namespace n’est spécifié.
2. **`kube-system`** :
    
    - Contient les objets système gérés par Kubernetes (par ex., contrôleurs, planificateur).
3. **`kube-public`** :
    
    - Lisible par tous les utilisateurs.
    - Utilisé pour des ressources accessibles à tous.
4. **`kube-node-lease`** :
    
    - Contient des objets **Node Lease**, qui servent aux **heartbeats** des nœuds pour signaler leur état au plan de contrôle.

---

#### **Pourquoi utiliser des Namespaces ?**

Les Namespaces sont utiles dans des environnements complexes pour :

1. **Isolation des ressources** :
    
    - Permet de regrouper les ressources par projet, équipe ou application.
2. **Gestion des quotas** :
    
    - Associez des quotas de ressources (CPU, mémoire) à chaque Namespace.
3. **Contrôle d’accès (RBAC)** :
    
    - Les règles de contrôle d'accès basées sur les rôles peuvent être appliquées au niveau du Namespace.

---

#### **Bonnes pratiques**

- Kubernetes recommande de n'utiliser les Namespaces que dans des environnements avec plusieurs dizaines d’utilisateurs ou projets.
- Pour différencier les environnements comme **test**, **staging**, ou **production**, préférez utiliser des **labels** au sein d’un même Namespace.

---

#### **Commandes liées aux Namespaces**

1. **Créer un Namespace :**
    
    bash
    
    `kubectl create namespace mon-namespace`
    
2. **Lister les Namespaces :**
    
    bash
    
    `kubectl get namespaces`
    
3. **Créer une ressource dans un Namespace spécifique :**
    
    bash

    `kubectl create -f mon-pod.yaml -n mon-namespace`
    
4. **Lister les ressources dans un Namespace spécifique :**
    
    bash
    
    `kubectl get pods -n mon-namespace`
    
5. **Supprimer un Namespace :**
    
    bash
    
    `kubectl delete namespaces mon-namespace`
    

---

#### **Résumé des avantages des Namespaces**

|**Avantage**|**Description**|
|---|---|
|**Isolation des ressources**|Partitionner les ressources pour différents projets ou applications.|
|**Gestion des quotas**|Limiter l’utilisation de CPU et de mémoire pour chaque Namespace.|
|**Contrôle d’accès (RBAC)**|Appliquer des politiques de sécurité spécifiques à chaque Namespace.|
