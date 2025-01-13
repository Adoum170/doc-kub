---
tags:
  - "#Intro_Kub/Minikube_KubeCtl"
---
***

#### **Le Dashboard de Minikube**

Minikube fournit un tableau de bord (_dashboard_) qui offre une interface graphique simple pour superviser et interagir avec votre cluster Kubernetes.

##### **Étapes pour activer et lancer le tableau de bord :**

1. **Activer le module `metrics-server`** :  
    Ce module est nécessaire pour certaines fonctionnalités, notamment la collecte de métriques.
    
    bash
    
    `minikube addons enable metrics-server`
    
2. **Lancer le tableau de bord** :  
    Cette commande crée un proxy temporaire pour rendre le tableau de bord accessible via votre navigateur.
    
    bash
    
    `minikube dashboard`
    
3. **Arrêter le tableau de bord :**
    
    - Pour arrêter le proxy, utilisez **Ctrl+C** dans le terminal.
    - Pour arrêter complètement le cluster Minikube :
        
        bash
        
        `minikube stop`
        
    - Pour supprimer tous les clusters créés avec Minikube :
        
        bash
        
        `minikube delete --all`
        

---

#### **Gestion des contextes avec kubectl**

Lorsque plusieurs environnements Kubernetes sont configurés sur une même machine (par exemple, **Minikube** et **Docker Desktop Kubernetes**), il est crucial de choisir le **contexte** actif. Le contexte détermine quel cluster Kubernetes est utilisé par `kubectl`.

##### **Commandes pour gérer les contextes :**

1. **Lister les contextes disponibles :**
    
    bash
    
    `kubectl config get-contexts`
    
2. **Changer de contexte :**
    
    - Pour utiliser **Docker Desktop** :
        
        bash
        
        `kubectl config use-context docker-desktop`
        
    - Pour utiliser **Minikube** :
        
        bash
        
        `kubectl config use-context minikube`
        
3. **Vérifier la connexion au cluster :**  
    Une fois le contexte défini, assurez-vous que `kubectl` fonctionne correctement :
    
    bash
    
    `kubectl get nodes`
    

---

#### Résumé

- **Minikube Dashboard** :
    
    - Une interface graphique pour superviser les ressources Kubernetes.
    - Nécessite l'activation du module `metrics-server`.
    - Peut être lancé avec `minikube dashboard` et arrêté avec **Ctrl+C** ou `minikube stop`.
- **Gestion des contextes** :
    
    - Indispensable lorsqu’on utilise plusieurs environnements Kubernetes (Minikube, Docker Desktop, etc.).
    - Permet de basculer facilement entre les clusters avec `kubectl config use-context`.

Avec ces outils et configurations, vous êtes prêt à explorer plus en détail les fonctionnalités de Kubernetes ! 🚀
