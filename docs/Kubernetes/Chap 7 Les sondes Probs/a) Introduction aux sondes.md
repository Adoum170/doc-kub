---
tags:
  - "#Intro_Sondes/Introduction_aux_sondes"
---
***


#### **Les types de sondes (Probes) dans Kubernetes**

Les **Probes** sont des mécanismes utilisés par Kubernetes pour surveiller l'état des conteneurs au sein d'un Pod. Il existe trois types principaux de sondes :

---

#### **1. Liveness Probes**

- **Objectif :** Détecter si un conteneur est "mort" ou bloqué et nécessitant un redémarrage.
- **Cas d’usage :** Redémarrer un conteneur pour améliorer la disponibilité, même en cas de bugs.
- **Risques :** Une mauvaise configuration peut entraîner des échecs en cascade.

---

#### **2. Readiness Probes**

- **Objectif :** Déterminer si un conteneur est prêt à recevoir du trafic.
- **Cas d’usage :**
    - Gérer la disponibilité des Pods dans un **Service**.
    - Retirer temporairement un Pod des équilibreurs de charge s'il n'est pas prêt.
- **Condition :** Tous les conteneurs d’un Pod doivent être prêts pour que le Pod soit considéré comme prêt.

---

#### **3. Startup Probes**

- **Objectif :** Vérifier si une application conteneurisée a bien démarré.
- **Cas d’usage :**
    - Gérer les applications ayant des temps de démarrage longs.
    - Suspendre les sondes de liveness et readiness tant que la startup probe n’a pas réussi.
- **Avantage :** Évite de tuer des conteneurs en cours de démarrage.

---

#### **Configuration des sondes**

Toutes les sondes partagent les mêmes paramètres de configuration. Voici un aperçu des paramètres les plus importants :

|**Paramètre**|**Description**|**Valeur par défaut**|
|---|---|---|
|**`initialDelaySeconds`**|Temps en secondes avant de démarrer les sondes après le lancement du conteneur.|0 secondes|
|**`periodSeconds`**|Fréquence des vérifications en secondes.|10 secondes|
|**`timeoutSeconds`**|Temps maximum d’attente d’une réponse avant de considérer que la sonde a échoué.|1 seconde|
|**`successThreshold`**|Nombre de succès consécutifs nécessaires pour considérer la sonde comme réussie. (Applicable uniquement aux readiness.)|1|
|**`failureThreshold`**|Nombre d’échecs consécutifs avant que la sonde soit considérée comme échouée.|3|
|**`terminationGracePeriodSeconds`**|Temps d'attente avant d'arrêter de force un conteneur défaillant (SIGKILL).|30 secondes (Pod)|

#### **Exemple de configuration d’un fichier YAML**

##### **Liveness Probe**

	livenessProbe:
	  httpGet:
	    path: /healthz
	    port: 8080
	  initialDelaySeconds: 3
	  periodSeconds: 5

##### **Readiness Probe**

yaml

	readinessProbe:
	  exec:
	    command:
	    - cat
	    - /tmp/ready
	  initialDelaySeconds: 5
	  periodSeconds: 10

##### **Startup Probe**

yaml


	startupProbe:
	  tcpSocket:
	    port: 8080
	  initialDelaySeconds: 10
	  periodSeconds: 10
	  failureThreshold: 30

#### **Impact de la configuration**

1. **Pour les sondes de liveness et readiness :**
    
    - Échecs répétés (selon `failureThreshold`) :
        - Liveness : redémarre le conteneur.
        - Readiness : retire temporairement le Pod des équilibreurs de charge, mais le conteneur reste en cours d’exécution.
2. **Pour la startup probe :**
    
    - Liveness et readiness sont suspendues tant que la startup probe n’a pas réussi.
3. **Avec `terminationGracePeriodSeconds` :**
    
    - Fournit du temps au conteneur pour effectuer un arrêt propre (ex. : sauvegarder les données, fermer les connexions).

---

#### **Résumé**

|**Type de sonde**|**Objectif**|**Impact en cas d’échec**|
|---|---|---|
|**Liveness**|Détecter si un conteneur est bloqué ou mort.|Redémarrage automatique du conteneur.|
|**Readiness**|Vérifier si un conteneur est prêt à recevoir du trafic.|Le Pod est retiré des équilibreurs de charge.|
|**Startup**|Vérifier si l’application a correctement démarré.|Suspend liveness/readiness jusqu’au succès.|

Ces sondes permettent une meilleure résilience des applications, mais nécessitent une configuration soigneuse pour éviter des comportements indésirables.