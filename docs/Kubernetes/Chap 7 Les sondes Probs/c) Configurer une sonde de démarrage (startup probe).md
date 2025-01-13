---
tags:
  - "#Intro_Sondes/Config_une_sonde_de_preparation"
---
***

#### **Les sondes de démarrage (Startup Probes)**

Les **startup probes** permettent de vérifier si une application conteneurisée a correctement démarré, notamment pour les applications qui nécessitent un temps de démarrage prolongé. Elles évitent que des sondes **Liveness** ou **Readiness** ne redémarrent ou déclarent l'application défaillante pendant sa phase d'initialisation.

---

#### **Quand utiliser une startup probe ?**

- **Applications lentes à démarrer** :
    - Par exemple, des applications Java ou celles nécessitant une initialisation complexe (connexion à une base de données, chargement de fichiers volumineux, etc.).
- **Problèmes de redémarrages infinis** :
    - Une **Liveness Probe** mal configurée peut considérer une application comme défaillante alors qu'elle est en cours de démarrage, entraînant un cycle de redémarrages.

---

#### **Caractéristiques des startup probes**

- Désactivent les sondes **Liveness** et **Readiness** jusqu’à ce qu’elles réussissent.
- Fournissent une période de grâce pour les applications lentes à démarrer.
- Échec configuré via un seuil (`failureThreshold`).

Exemple de configuration complète 

	apiVersion: v1
	kind: Pod
	metadata:
	  name: mon-application
	spec:
	  containers:
	  - name: mon-application
	    image: mon-image
	    livenessProbe:
	      httpGet:
	        path: /health
	        port: 8080
	      initialDelaySeconds: 60
	      periodSeconds: 10
	    readinessProbe:
	      httpGet:
	        path: /ready
	        port: 8080
	      initialDelaySeconds: 30
	      periodSeconds: 5
	    startupProbe:
	      httpGet:
	        path: /health
	        port: 8080
	      failureThreshold: 30
	      periodSeconds: 10

#### **Explication des champs**

1. **`startupProbe`** :
    
    - **`httpGet`** : La sonde effectue une requête HTTP GET sur `/health`.
    - **`failureThreshold`** : Le nombre d'échecs consécutifs avant de considérer que le conteneur n'a pas démarré. Ici, après 30 échecs consécutifs (5 minutes), Kubernetes considère l'application comme défaillante.
    - **`periodSeconds`** : Intervalle entre les sondes (10 secondes dans cet exemple).
2. **`livenessProbe`** :
    
    - Utilisée pour vérifier que l’application reste opérationnelle après son démarrage.
    - Configurée pour commencer 60 secondes après le démarrage du conteneur.
3. **`readinessProbe`** :
    
    - Détermine si le conteneur est prêt à accepter du trafic.
    - Configurée pour commencer 30 secondes après le démarrage du conteneur.

---

#### **Cycle des sondes avec une startup probe**

1. **Phase de démarrage :**
    
    - Kubernetes exécute uniquement la **Startup Probe**.
    - Les sondes **Liveness** et **Readiness** sont ignorées.
2. **Une fois que la startup probe réussit :**
    
    - La sonde **Liveness** surveille la santé générale du conteneur.
    - La sonde **Readiness** vérifie si le Pod est prêt à recevoir du trafic.
3. **En cas d'échec de la startup probe :**
    
    - Kubernetes considère que le conteneur a échoué à démarrer et peut tenter de le redémarrer selon la politique de redémarrage du Pod.

---

#### **Bonnes pratiques**

- **Définir `failureThreshold` et `periodSeconds` avec soin** :
    - Adaptez ces valeurs au temps nécessaire pour que l'application démarre.
- **Ne pas utiliser de startup probe pour des applications démarrant rapidement** :
    - Cela alourdit inutilement la configuration et peut retarder l’activation des sondes **Liveness** et **Readiness**.

---

#### **Résumé des sondes**

|**Sonde**|**Objectif**|**Déclenchement**|
|---|---|---|
|**Startup**|Vérifie que l'application a correctement démarré.|Avant Liveness/Readiness|
|**Liveness**|Vérifie que l'application est toujours fonctionnelle.|Après succès de Startup|
|**Readiness**|Vérifie que l'application est prête à recevoir du trafic.|Après succès de Startup|

---

#### **Commandes utiles**

1. **Appliquer une configuration YAML contenant une startup probe :**
    
    bash
    
    `kubectl apply -f mon-pod.yaml`
    
2. **Vérifier l’état du Pod :**
    
    bash
    
    `kubectl get pods`
    
3. **Voir les détails des sondes dans un Pod :**
    
    bash
    
    `kubectl describe pod mon-application`
