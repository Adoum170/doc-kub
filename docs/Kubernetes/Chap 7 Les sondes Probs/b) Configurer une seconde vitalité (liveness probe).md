---
tags:
  - "#Intro_Sondes/Config_sonde_de_vitalité"
---
***

#### **Création d'un déploiement avec une sonde Liveness**

Les sondes **Liveness** dans Kubernetes sont utilisées pour détecter si un conteneur est bloqué ou non fonctionnel. En cas d'échec, Kubernetes redémarre le conteneur.

---

### **Exemple de configuration non fonctionnelle**

Créez un fichier `mon-deploiement.yaml` avec une sonde Liveness mal configurée :

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
	        image: dymafr/k8c4
	        ports:
	        - containerPort: 80
	        livenessProbe:
	          httpGet:
	            path: /test.html
	            port: 80
	          initialDelaySeconds: 5
	          periodSeconds: 5
	          failureThreshold: 1
	          successThreshold: 1
	          timeoutSeconds: 1
	          
##### **Explications :**

- La sonde effectue une requête HTTP GET sur `/test.html`.
- Comme ce chemin n'existe pas dans l'application, la sonde échoue systématiquement, entraînant des redémarrages du conteneur.

---

### **Résultats observés**

1. **Vérification des Pods :**
    
    bash
    
    Copier le code
    
    `kubectl get pods`
    
    Exemple de sortie :
    
    plaintext
    
    Copier le code
    
    `NAME                                READY   STATUS             RESTARTS      AGE nginx-deployment-6f68c7c7c5-hpgkz   0/1     CrashLoopBackOff   6 (6s ago)    3m45s`
    
2. **Message d'erreur dans le dashboard Minikube :**
    
    - État : `CrashLoopBackOff`
    - Échec de la sonde : `Liveness probe failed: HTTP probe failed with statuscode: 404`
3. **Cycle des événements :**
    
    - Le conteneur est démarré.
    - La sonde Liveness échoue.
    - Kubernetes tue le conteneur et tente de le redémarrer.

---

### **Correction et configuration fonctionnelle**

Modifiez le fichier YAML pour utiliser un chemin existant (`/index.html`) :

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
	        image: dymafr/k8c4
	        ports:
	        - containerPort: 80
	        livenessProbe:
	          httpGet:
	            path: /index.html
	            port: 80
	          initialDelaySeconds: 5
	          periodSeconds: 5
	          failureThreshold: 1
	          successThreshold: 1
	          timeoutSeconds: 1

1. **Appliquer les modifications :**
    
    bash
    
    `kubectl apply -f mon-deploiement.yaml`
    
2. **Vérifier les Pods :**
    
    bash
    
    `kubectl get pods`
    
    Les Pods sont désormais dans l'état `Running`.
    

---

### **Paramètres pour les sondes HTTP (`httpGet`)**

|**Paramètre**|**Description**|**Valeur par défaut**|
|---|---|---|
|**`path`**|Chemin pour effectuer la requête HTTP.|`/`|
|**`port`**|Port sur lequel la requête est envoyée.|**Obligatoire**|
|**`scheme`**|Schéma utilisé pour la requête (`HTTP` ou `HTTPS`).|`HTTP`|
|**`httpHeaders`**|En-têtes personnalisés à inclure dans la requête.|Aucune|
|**`host`**|Nom de l'hôte auquel se connecter. Par défaut, l'IP du Pod est utilisée (généralement inutile).|IP du Pod|

---

### **Détails des sondes HTTP**

1. **Codes de réponse considérés comme un succès :**
    
    - Tout code compris entre **200** et **399**.
2. **En-têtes envoyés par défaut :**
    
    - **`User-Agent`** : `kube-probe/<version>` (ex. : `kube-probe/1.27`).
    - **`Accept`** : `*/*`.
3. **Sonde HTTPS :**
    
    - Si `scheme` est défini sur `HTTPS`, le kubelet ignore la vérification du certificat SSL.

---

### **Résumé**

- Les sondes Liveness permettent de redémarrer un conteneur non fonctionnel.
- Les sondes HTTP utilisent des requêtes GET pour vérifier l’état d’un conteneur.
- Une mauvaise configuration peut provoquer des redémarrages répétés (CrashLoopBackOff).
- En cas de démarrage lent, envisagez une **Startup Probe** pour éviter des redémarrages intempestifs.