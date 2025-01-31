---
tags:
  - Principaux_Objet/Introduction_Deployment
---
***
#### **Qu'est-ce qu'un déploiement d'application ?**

Un déploiement dans Kubernetes permet de gérer le cycle de vie des applications conteneurisées en créant un objet **Deployment**. Cet objet définit comment créer et mettre à jour les instances de votre application de manière déclarative.

---

#### **Étapes d'un déploiement**

1. **Définition du déploiement** :
    
    - Les caractéristiques de l'application sont spécifiées dans un fichier YAML :
        - Nombre de réplicas.
        - Images de conteneurs à utiliser.
        - Ports exposés.
    - Exemple :
        
        yaml
        
        apiVersion: apps/v1
		kind: Deployment
		metadata:
		  name: nginx-deployment
		spec:
		  replicas: 1
		  selector:
		    matchLabels:
		      app: nginx
		  template:
		    metadata:
		      labels:
		        app: nginx
		    spec:
		      containers:
				 \-name: nginx-container
			     image: nginx:alpine
		        ports:
				\- containerPort: 80
        
2. **Création du déploiement** :
    
    - On applique le fichier YAML au cluster :
        
        bash
        
        `kubectl apply -f mon-premier-deploiement.yaml`
        
3. **Planification** :
    
    - Le **kube-scheduler** décide où placer les Pods en tenant compte des ressources disponibles et des exigences du déploiement.
4. **Surveillance** :
    
    - Le **Deployment controller** surveille la disponibilité des instances.
    - En cas de panne d’un nœud, il remplace automatiquement les Pods manquants.

---

#### **Premier déploiement**

##### Création d’un déploiement Nginx

1. Créez un fichier `mon-premier-deploiement.yaml` avec les champs suivants :
    
    - **apiVersion** : Version de l'API Kubernetes utilisée (`apps/v1` ici).
    - **kind** : Type de ressource (ici, `Deployment`).
    - **metadata** : Informations sur l'objet (nom, labels).
    - **spec** : Décrit l'état souhaité (réplicas, sélecteurs, modèle de Pods).
    - **template** :
        - Définit les Pods créés par ce déploiement.
        - Contient les labels, conteneurs, et leurs configurations.
2. Appliquez-le au cluster avec `kubectl apply`.
    
3. Vérifiez l’état des déploiements :
    
    bash
    
    `kubectl get deployments`
    
    Sortie typique :
    
    plaintext
    
    `NAME               READY   UP-TO-DATE   AVAILABLE   AGE nginx-deployment   1/1     1            1           13s`
    

##### Explication des colonnes :

- **NAME** : Nom du déploiement.
- **READY** : Nombre de réplicas prêts.
- **UP-TO-DATE** : Nombre de réplicas correspondant à la dernière configuration.
- **AVAILABLE** : Réplicas disponibles pour traiter les requêtes.
- **AGE** : Temps écoulé depuis la création.

---

#### **Visualisation dans Minikube**

Pour voir les objets créés par Kubernetes :

1. Lancez le tableau de bord :
    
    bash
    
    Copier le code
    
    `minikube dashboard`
    
2. Accédez à l’onglet **Workloads** :
    - Vue d’ensemble des **Déploiements**, **Pods** et **ReplicaSets**.
    - **ReplicaSets** garantissent un nombre stable de Pods en cours d'exécution.

#### **Visualisation dans Docker Desktop**

Si vous utilisez Docker Desktop :

- Accédez à l’onglet **Containers**.
- Les conteneurs Kubernetes sont préfixés par `k8s`.
- Vous pouvez voir les journaux, ouvrir un terminal ou inspecter les conteneurs.

---

#### **Accéder à l'application**

##### Pourquoi ne fonctionne-t-elle pas sur le port 80 directement ?

- Les Pods dans Kubernetes s'exécutent dans un **réseau privé isolé**.
- Par défaut, ils ne sont accessibles qu’au sein du cluster.

##### Solution :

- Utilisez un **Service** pour exposer les Pods au réseau externe.
- Nous verrons comment configurer un Service dans les prochaines sections.

---

#### **Résumé**

- Les **Deployments** permettent de gérer les instances d'une application de manière déclarative.
- Ils offrent :
    - **Mise à l’échelle** : Augmenter ou réduire le nombre de réplicas.
    - **Mises à jour** : Mettre à jour les images des conteneurs.
    - **Auto-réparation** : Remplacer automatiquement les Pods défaillants.
- Les Pods d’un Deployment ne sont pas accessibles directement de l’extérieur du cluster, nécessitant un **Service** pour être exposés.


