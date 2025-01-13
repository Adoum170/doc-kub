---
tags:
  - "#Intro_Ingress/Composants_Ingress"
---
***

#### **La ressource Ingress**

Un **Ingress** est une ressource Kubernetes contenant les **règles de routage** pour le trafic entrant dans le cluster. Ces règles déterminent comment les requêtes HTTP(S) sont acheminées en fonction de :

- **L’hôte** : Le domaine ou le sous-domaine.
- **Le chemin** : L'URI ou le préfixe de l’URL.
- **Le Service backend** : Le service Kubernetes vers lequel diriger la requête.

---

#### **Qu'est-ce qu'un contrôleur Ingress ?**

Le **contrôleur Ingress** est un composant essentiel qui gère le fonctionnement de la ressource Ingress. Son rôle principal est de :

1. **Surveiller les ressources Ingress** dans le cluster.
2. **Configurer dynamiquement un proxy inverse** (reverse proxy) pour rediriger le trafic vers les Services backend.
3. **Gérer les modifications** apportées aux Ingress et adapter la configuration en temps réel.

---

#### **Contrôleurs Ingress officiels et populaires**

- **Officiels maintenus par Kubernetes** :
    
    - **AWS ALB** : Spécifique à AWS.
    - **Google Cloud Load Balancer** : Intégré à GCP.
    - **NGINX Ingress** : Généraliste et largement utilisé.
- **Autres contrôleurs populaires** (au moins 30 disponibles) :
    
    - **Traefik** : Intégré avec Docker et Kubernetes.
    - **HAProxy Ingress** : Performances élevées.
    - **Contour** : Spécialisé pour les architectures modernes.

> **Remarque :** Chaque contrôleur a ses spécificités et convient à des besoins différents. Le choix dépend de l'infrastructure et des fonctionnalités requises.

---

#### **Exemple de configuration d’une ressource Ingress**

Voici un fichier YAML de base pour une ressource Ingress :

	apiVersion: networking.k8s.io/v1
	kind: Ingress
	metadata:
	  name: minimal-ingress
	  annotations:
	    nginx.ingress.kubernetes.io/rewrite-target: /
	spec:
	  ingressClassName: nginx-example
	  rules:
	  - http:
	      paths:
	      - path: /testpath
	        pathType: Prefix
	        backend:
	          service:
	            name: test
	            port:
	              number: 80

##### **Explication des champs :**

1. **`metadata`** :
    
    - **`name`** : Nom de la ressource Ingress (`minimal-ingress` ici).
    - **`annotations`** : Ajout de métadonnées, ici pour indiquer au contrôleur nginx de réécrire l’URL.
2. **`spec`** :
    
    - **`ingressClassName`** : Spécifie le contrôleur Ingress à utiliser (`nginx-example` dans cet exemple).
    - **`rules`** : Définit les règles de routage.
        - **`path`** : Spécifie le chemin à surveiller (`/testpath`).
        - **`pathType`** : Indique le type de correspondance du chemin (`Prefix` signifie que toutes les URL commençant par `/testpath` sont incluses).
        - **`backend`** :
            - **`service.name`** : Nom du Service cible (`test` ici).
            - **`service.port.number`** : Port exposé par le Service (`80` ici).

---

#### **Flux d'une requête avec Ingress**

1. **Requête client → Ingress Controller** :
    
    - Le contrôleur d’Ingress reçoit la requête HTTP(S) d'un client externe.
2. **Contrôleur → Service backend** :
    
    - Il achemine la requête vers le Service correspondant, basé sur les règles spécifiées dans la ressource Ingress.
3. **Service backend → Pods** :
    
    - Le Service redirige la requête vers un Pod disponible qui traite la requête.
4. **Réponse client** :
    
    - La réponse suit le chemin inverse jusqu’au client.

---

#### **Résumé**

- **Ressource Ingress** : Contient les règles pour router le trafic entrant vers les Services backend.
- **Contrôleur Ingress** : Implémente les règles d'Ingress en configurant un proxy inverse.
- **Exemple basique** : Configuration d’une règle de routage pour acheminer `/testpath` vers le Service `test` sur le port 80.

Dans les prochaines leçons, nous explorerons :

- L’installation et la configuration de contrôleurs Ingress.
- La gestion avancée du trafic avec des règles plus complexes.