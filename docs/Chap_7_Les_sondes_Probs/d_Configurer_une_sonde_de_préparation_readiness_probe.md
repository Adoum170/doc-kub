---
tags:
  - "#Intro_Sondes/readiness_probe"
---
***

#### **Les sondes de préparation (Readiness Probes)**

Les **readiness probes** dans Kubernetes sont utilisées pour indiquer si un conteneur est prêt à recevoir du trafic. Elles permettent de gérer la disponibilité des Pods au sein d’un Service Kubernetes.

---

#### **Fonctionnement**

- **Succès de la sonde :**
    - Kubernetes commence à diriger le trafic vers le conteneur.
- **Échec de la sonde :**
    - Kubernetes cesse de diriger le trafic vers le conteneur jusqu’à ce que la sonde réussisse à nouveau.

> Cela est particulièrement utile pour :

- Les applications nécessitant un temps d’initialisation avant de devenir disponibles.
- Les applications temporairement indisponibles pendant leur exécution (ex. : maintenance, mises à jour).

---

#### **Exemple pratique**

##### **Configuration YAML d’un Pod avec une readiness probe :**

	apiVersion: v1
	kind: Pod
	metadata:
	  name: mon-application
	spec:
	  containers:
	  - name: mon-application
	    image: mon-image
	    readinessProbe:
	      httpGet:
	        path: /ready
	        port: 8080
	      initialDelaySeconds: 30
	      periodSeconds: 10

##### **Explications des champs :**

- **`httpGet`** :
    
    - Effectue une requête HTTP sur le chemin `/ready` pour vérifier la disponibilité.
    - Redirige le trafic uniquement si cette requête réussit.
- **`initialDelaySeconds`** :
    
    - Délai avant d’exécuter la première sonde (30 secondes ici).
- **`periodSeconds`** :
    
    - Fréquence d’exécution des sondes après la première tentative (toutes les 10 secondes ici).

---

#### **Cas d’utilisation**

1. **Application nécessitant un temps d’initialisation :**
    
    - Exemple : Une application qui charge des données volumineuses en mémoire au démarrage.
    - La readiness probe s’assure que l’application est pleinement opérationnelle avant de recevoir des requêtes.
2. **Application temporairement indisponible :**
    
    - Exemple : Une application effectuant une mise à jour de la base de données qui la rend indisponible temporairement.
    - La readiness probe signale l’indisponibilité à Kubernetes, qui cesse de diriger le trafic jusqu’à ce que la sonde réussisse à nouveau.

---

#### **Commandes utiles**

1. **Créer ou mettre à jour une ressource avec readiness probe :**
    
    bash
    
    Copier le code
    
    `kubectl apply -f mon-pod.yaml`
    
2. **Vérifier l’état des Pods :**
    
    bash
    
    Copier le code
    
    `kubectl get pods`
    
3. **Afficher les détails des sondes pour un Pod :**
    
    bash
    
    Copier le code
    
    `kubectl describe pod mon-application`
    

---

#### **Résumé des sondes de préparation**

|**Aspect**|**Description**|
|---|---|
|**Objectif**|Vérifier si un conteneur est prêt à recevoir du trafic.|
|**Succès de la sonde**|Kubernetes commence à rediriger le trafic vers le conteneur.|
|**Échec de la sonde**|Kubernetes cesse temporairement de diriger le trafic vers le conteneur jusqu’à nouvel ordre.|
|**Cas d’utilisation**|Initialisation lente, maintenance ou indisponibilité temporaire de l’application.|

---

Les readiness probes jouent un rôle clé dans la gestion de la disponibilité des applications Kubernetes, assurant une meilleure résilience et une expérience utilisateur optimisée.