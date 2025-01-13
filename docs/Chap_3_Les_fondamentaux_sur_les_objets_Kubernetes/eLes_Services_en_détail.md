---
tags:
  - "#Objet/Service"
---
***

#### **Rappels sur les Services Kubernetes**

Un **Service** dans Kubernetes sert à **exposer des Pods** (individuellement ou en groupe) à d'autres applications ou au trafic externe, tout en offrant un découplage entre les consommateurs et les producteurs de services.

---

### **Définir un Service**

Exemple de définition simple :

yaml


	apiVersion: v1
	kind: Service
	metadata:
	  name: mon-service
	spec:
	  selector:
	    app.kubernetes.io/name: MonApp
	  ports:
	    - protocol: TCP
	      port: 80
	      targetPort: 9376


- **`selector`** : Identifie les Pods cibles (ici ceux avec le label `app.kubernetes.io/name: MonApp`).
- **`port`** : Port utilisé par d'autres applications au sein du cluster.
- **`targetPort`** : Port du conteneur à l'intérieur du Pod.

---

### **Nommer les ports**

Kubernetes permet de **nommer les ports**, facilitant leur identification et la gestion des modifications.

**Exemple avec un Deployment et un Service :**

Deployment YAML :

yaml

	apiVersion: apps/v1
	kind: Deployment
	metadata:
	  name: nginx-deployment
	spec:
	  replicas: 3
	  selector:
	    matchLabels:
	      app: nginx
	  template:
	    metadata:
	      labels:
	        app: nginx
	    spec:
	      containers:
	      - name: nginx-container
	        image: nginx:alpine
	        ports:
	          - containerPort: 80
	            name: http-nginx


Service YAML :

yaml

	apiVersion: v1
	kind: Service
	metadata:
	  name: nginx-service
	spec:
	  type: NodePort
	  selector:
	    app: nginx
	  ports:
	    - protocol: TCP
	      port: 80
	      targetPort: http-nginx


Appliquer ces fichiers :

bash


`kubectl apply -f mon-premier-deploiement.yaml 
`kubectl apply -f mon-premier-service.yaml

---

### **Exposer plusieurs ports**

Pour exposer plusieurs ports dans un Service, chaque port doit avoir un **nom unique** :

yaml

	apiVersion: v1
	kind: Service
	metadata:
	  name: nginx-service
	spec:
	  type: NodePort
	  selector:
	    app: nginx
	  ports:
	    - name: http
	      protocol: TCP
	      port: 80
	      targetPort: http-nginx
	    - name: https
	      protocol: TCP
	      port: 443
	      targetPort: https-nginx


> **Remarque :** La configuration de HTTPS nécessite des réglages supplémentaires au niveau de NGINX.

---

### **Types de Services Kubernetes**

#### 1. **ClusterIP (par défaut)**

- Fournit une communication interne au cluster.
- Utilisé pour connecter des composants comme les frontends et backends.

#### 2. **NodePort**

- Permet un accès externe au cluster via `<IP-du-nœud>:<NodePort>`.
- Exemple :

yaml

	apiVersion: v1
	kind: Service
	metadata:
	  name: nginx-service
	spec:
	  type: NodePort
	  selector:
	    app: nginx
	  ports:
	    - protocol: TCP
	      port: 80
	      targetPort: 80
	      nodePort: 30007


- Plage des ports : `30000-32767`.

#### 3. **LoadBalancer**

- Nécessite un fournisseur cloud compatible (GCP, AWS, Azure, etc.).
- Crée un équilibreur de charge externe et attribue une adresse IP externe.
- Statut publié sous `status > loadBalancer`.

#### 4. **ExternalName**

- Reflète un service situé en dehors du cluster.
- Exemple : connecter une base PostgreSQL hébergée sur AWS RDS.

yaml

	apiVersion: v1
	kind: Service
	metadata:
	  name: postgres-db
	spec:
	  type: ExternalName
	  externalName: mydatabase.abcdefghij.us-east-1.rds.amazonaws.com


- Exemple pour un Bucket S3 :

yaml

	apiVersion: v1
	kind: Service
	metadata:
	  name: mon-super-s3
	spec:
	  type: ExternalName
	  externalName: my-s3-bucket.s3.amazonaws.com


---

### **Résumé des types de Services**

|**Type**|**Accessibilité**|**Cas d'usage**|
|---|---|---|
|**ClusterIP**|Interne au cluster uniquement.|Communication entre applications au sein du cluster.|
|**NodePort**|Externe via `<IP>:<NodePort>`.|Accès externe direct ou équilibrage manuel.|
|**LoadBalancer**|Externe via un équilibreur de charge.|Production sur un fournisseur cloud.|
|**ExternalName**|Résolution DNS vers un service externe.|Connexion à des bases de données ou services cloud.|

---

### **Commandes utiles avec Services**

1. **Lister les Services :**
    
    bash
    
    `kubectl get services`
    
2. **Obtenir des détails :**
    
    bash
    
    `kubectl describe service <service-name>`
    
3. **Vérifier l’accès dans Minikube :**
    
    bash
    
    `minikube service <service-name> --url`