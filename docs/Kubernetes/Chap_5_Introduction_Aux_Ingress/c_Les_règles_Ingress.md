---
tags:
  - "#Intro_Ingress/Règle_Ingress"
---
***

#### **Les règles Ingress**

Les **règles Ingress** définissent comment acheminer le trafic entrant vers les services Kubernetes. Ces règles sont configurées dans une ressource Ingress à l'aide d'un fichier YAML.

Structure de base d’un fichier Ingress

	apiVersion: networking.k8s.io/v1
	kind: Ingress
	metadata:
	  name: exemple-ingress
	  annotations:
	    nginx.ingress.kubernetes.io/rewrite-target: /
	spec:
	  rules:
	  - host: mon-app.mon-domaine.fr
	    http:
	      paths:
	      - pathType: Prefix
	        path: "/un-chemin"
	        backend:
	          service:
	            name: mon-service
	            port:
	              number: 8080

##### **Explications :**

1. **`metadata`** :
    
    - **`name`** : Nom de la ressource Ingress.
    - **`annotations`** : Ajoute des métadonnées pour configurer le comportement du contrôleur d’Ingress (par exemple, réécriture d’URL avec NGINX).
2. **`spec`** :
    
    - **`rules`** : Contient les règles de routage (basées sur le **chemin** et/ou l'**hôte**).
        - **`host`** : Domaine ou sous-domaine pour les règles (facultatif).
        - **`paths`** : Définit les chemins à surveiller.
            - **`pathType`** : Type de correspondance (voir détails ci-dessous).
            - **`backend`** : Définit le Service cible et le port.

---

#### **Types de correspondance des chemins (`pathType`)**

1. **`ImplementationSpecific`** :
    
    - Dépend du comportement du contrôleur d’Ingress.
    - Utile lorsque plusieurs classes d’Ingress sont utilisées dans le même cluster (avancé).
2. **`Exact`** :
    
    - Correspond exactement au chemin URL spécifié.
    - Sensible à la casse.
3. **`Prefix`** (le plus courant) :
    
    - Correspond à tous les chemins qui commencent par le préfixe spécifié.
    - Sensible à la casse.
    - La correspondance se fait élément par élément.

##### **Exemple :**

	spec:
	  rules:
	  - http:
	      paths:
	      - pathType: Prefix
	        path: "/service1"
	        backend:
	          service:
	            name: service1
	            port:
	              number: 80
	      - pathType: Prefix
	        path: "/service2"
	        backend:
	          service:
	            name: service2
	            port:
	              number: 80

---
#### **Routage basé sur l’hôte**

Le routage basé sur l’hôte est utile pour héberger plusieurs applications ou sites web sur un même cluster Kubernetes.

##### **Exemple :**

	spec:
	  rules:
	  - host: "exemple1.fr"
	    http:
	      paths:
	      - pathType: Prefix
	        path: "/"
	        backend:
	          service:
	            name: service1
	            port:
	              number: 80
	  - host: "exemple2.fr"
	    http:
	      paths:
	      - pathType: Prefix
	        path: "/"
	        backend:
	          service:
	            name: service2
	            port:
	              number: 80

---

#### **Backend par défaut**

Un **backend par défaut** gère toutes les requêtes qui ne correspondent à aucune règle définie. C'est une solution de repli pour gérer les requêtes imprévues ou mal configurées.

##### **Exemple :**

	spec:
	  defaultBackend:
	    service:
	      name: default-service
	      port:
	        number: 80
	  rules:
	  - http:
	      paths:
	      - pathType: Prefix
	        path: "/service1"
	        backend:
	          service:
	            name: service1
	            port:
	              number: 80

---

#### **Résumé des règles Ingress**

|**Aspect**|**Description**|
|---|---|
|**Routage basé sur le chemin**|Acheminer en fonction d’un chemin URL (`/service1`, `/service2`).|
|**Routage basé sur l’hôte**|Acheminer en fonction d’un domaine ou sous-domaine (`exemple1.fr`, `exemple2.fr`).|
|**Backend par défaut**|Service de repli pour les requêtes non correspondantes.|
|**Annotations**|Configurer des comportements spécifiques (ex. : réécriture d'URL avec `nginx.ingress.kubernetes.io/rewrite-target`).|

---

#### **Commandes utiles**

1. **Créer ou appliquer un fichier Ingress :**
    
    bash
    
    `kubectl apply -f mon-ingress.yaml`
    
2. **Lister les Ingress dans le cluster :**
    
    bash
    
    `kubectl get ingress`
    
3. **Détailler un Ingress :**
    
    bash
    
    `kubectl describe ingress <nom-ingress>`