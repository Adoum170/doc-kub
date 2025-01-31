#### **Activer un contrôleur Ingress avec Minikube**

Minikube permet de tester un Ingress localement en activant les **add-ons Ingress**. Ces modules utilisent le **contrôleur NGINX-Ingress** préconfiguré pour Minikube.

---

#### **Étapes pour activer le contrôleur Ingress**

1. **Activer les modules nécessaires :**
    
    bash
    
    `minikube addons enable ingress minikube addons enable ingress-dns`
    
2. **Vérifier le déploiement du contrôleur :**
    
    bash
    
    `kubectl get pods -n ingress-nginx`
    
    - Cette commande affiche les Pods déployés dans le namespace `ingress-nginx`, où le contrôleur est hébergé.

---

#### **Création d’une ressource Ingress**

**Fichier YAML `mon-ingress.yaml` :**

	apiVersion: networking.k8s.io/v1
	kind: Ingress
	metadata:
	  name: mon-ingress
	  annotations:
	    nginx.ingress.kubernetes.io/rewrite-target: /
	spec:
	  rules:
	  - http:
	      paths:
	      - path: /
	        pathType: Prefix
	        backend:
	          service:
	            name: k8c4-service
	            port:
	              number: 80

**Explications des champs :**

- **`metadata > annotations`** :
    - `nginx.ingress.kubernetes.io/rewrite-target: /` : Réécrit les URLs entrantes pour qu’elles commencent à la racine (`/`) avant d’être redirigées vers le backend.
- **`spec > rules`** :
    - **`path`** : Toutes les requêtes commençant par `/` sont redirigées.
    - **`backend > service > name`** : Spécifie le Service Kubernetes (`k8c4-service`) cible.
    - **`port > number`** : Port du Service backend (80).

---

#### **Déployer l’Ingress**

1. **Créer la ressource Ingress :**
    
    bash
    
    `kubectl apply -f mon-ingress.yaml`
    
2. **Ouvrir un tunnel pour accéder au cluster localement :**
    
    bash
    
    `minikube tunnel`
    
3. **Tester l’accès dans un navigateur :**
    
    - Accédez à : [http://127.0.0.1/](http://127.0.0.1/)

---

#### **Commandes utiles**

1. **Vérifier l’état de l’Ingress :**
    
    bash
    
    `kubectl get ingress`
    
2. **Détailler les configurations d’un Ingress :**
    
    bash
    
    `kubectl describe ingress mon-ingress`
    
3. **Vérifier les Pods du contrôleur NGINX :**
    
    bash
    
    `kubectl get pods -n ingress-nginx`
    

---

#### **Résumé**

- Minikube facilite le test des Ingress localement avec le contrôleur **NGINX-Ingress**.
- Le tunnel Minikube permet de rediriger les requêtes locales vers le cluster Kubernetes.
- L’Ingress YAML simplifie le routage vers les Services backend en définissant des règles claires.