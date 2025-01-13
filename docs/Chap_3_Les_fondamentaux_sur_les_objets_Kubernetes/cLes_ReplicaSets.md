---
tags:
  - "#Objet/Replicaset"
---
***


#### **Qu'est-ce qu'un ReplicaSet ?**

Un **ReplicaSet** est un objet Kubernetes utilisé pour garantir qu’un nombre spécifique de copies (**réplicas**) d’un Pod est maintenu dans le cluster. Il assure la haute disponibilité et la redondance des Pods.

- Si un Pod tombe en panne ou est supprimé, le ReplicaSet crée automatiquement un nouveau Pod pour maintenir le nombre spécifié de réplicas.
- Les ReplicaSets utilisent des **sélecteurs de labels** pour identifier les Pods à gérer.

---

#### **Fonctionnement d'un ReplicaSet**

Un ReplicaSet est défini avec les champs suivants :

1. **Sélecteur** :
    
    - Détermine quels Pods le ReplicaSet doit gérer.
    - Basé sur les **labels** des Pods.
2. **Nombre de réplicas** :
    
    - Indique combien de Pods doivent être actifs en même temps.
3. **Template de Pod** :
    
    - Modèle utilisé pour créer de nouveaux Pods si nécessaire.

##### **Relation entre ReplicaSet et Pods :**

- Les Pods créés par un ReplicaSet contiennent une référence dans leur champ `metadata.ownerReferences`, indiquant qu’ils sont gérés par ce ReplicaSet.
- Exemple de sortie avec `kubectl describe pods` :
    
    plaintext
    
    Copier le code
    
    `Controlled By:  ReplicaSet/nginx-deployment-5956959495`
    

---

#### **Quand utiliser un ReplicaSet ?**

En pratique, les **ReplicaSets** sont rarement utilisés directement. Ils sont principalement utilisés **indirectement** via des **Deployments**, qui offrent une gestion plus riche (mises à jour, rollbacks, etc.).

> **Recommandation officielle de Kubernetes** :  
> Utilisez des **Deployments** plutôt que des ReplicaSets directement.

---

#### **Exemple pratique : Observer un ReplicaSet**

1. **Vérifier le ReplicaSet associé à un Deployment :**
    
    bash
    
    Copier le code
    
    `kubectl describe replicaset`
    
    Exemple de sortie :
    
    plaintext
    
    `Name:           nginx-deployment-5956959495 Namespace:      default Selector:       app=nginx,pod-template-hash=5956959495 Labels:         app=nginx                 pod-template-hash=5956959495 Controlled By:  Deployment/nginx-deployment Replicas:       1 current / 1 desired Pods Status:    1 Running / 0 Waiting / 0 Succeeded / 0 Failed`
    
    Points clés :
    
    - **Selector** : Détermine les Pods gérés (`app=nginx`).
    - **Replicas** : Nombre actuel et souhaité de Pods.
    - **Controlled By** : Indique que ce ReplicaSet est géré par un Deployment.
2. **Simuler la suppression d’un Pod géré par un ReplicaSet :**
    
    - Listez les Pods :
        
        bash
        
        `kubectl get pods`
        
    - Supprimez un Pod spécifique :
        
        bash
        
        `kubectl delete pod <nom-du-pod>`
        
    - Observez le comportement du ReplicaSet :
        
        bash
        
        `kubectl get pods`
        
    
    **Constat :**
    
    - Le ReplicaSet recrée automatiquement le Pod supprimé pour maintenir le nombre de réplicas spécifié.

---

#### **Résumé**

- Un **ReplicaSet** garantit la disponibilité d’un nombre défini de Pods dans le cluster.
- Il est principalement utilisé via des **Deployments** pour simplifier la gestion des applications.
- Les ReplicaSets surveillent et recréent automatiquement les Pods en cas de panne ou de suppression.
- Bien que rarement manipulés directement, leur rôle est fondamental pour assurer la stabilité des applications dans Kubernetes.