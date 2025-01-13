---
tags:
  - "#Objet/Déployments_en_détail"
---
***



#### **Modification d’un déploiement Kubernetes**

Les **déploiements Kubernetes** permettent de gérer le cycle de vie des applications, y compris leur mise à jour, mise à l'échelle et rollback.

---

#### **Mise à jour du nombre de réplicas**

1. **Exemple de fichier YAML modifié :**

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
2. **Appliquer les modifications :**
    
    bash
    
    `kubectl apply -f mon-premier-deploiement.yaml`
    
3. **Vérifier les ressources avec `kubectl get all` :**
    
    - Résultat :
        - **Pods** : 3 réplicas, tous prêts.
        - **Deployments** : 3 Pods disponibles.
        - **ReplicaSets** : 3 Pods dans le ReplicaSet.

---

#### **Détails sur la configuration**

- **`metadata > name` :** Définit le nom du déploiement, qui devient le préfixe pour les ReplicaSets et Pods.
- **`spec > replicas` :** Définit le nombre souhaité de réplicas.
- **`selector > matchLabels` :** Indique comment le ReplicaSet sélectionne les Pods qu'il doit gérer.
- **`template > metadata > labels` :** Ajoute des labels aux Pods créés.
- **`template > spec` :** Définit les détails des Pods (conteneurs, images, ports).

---

#### **Mise à jour d’un déploiement**

1. **Changer l’image utilisée :** Exemple : Passer de `nginx:alpine` à `nginx`.
    
    yaml
    
    `spec:   containers:   - name: nginx-container     image: nginx`
    
2. **Appliquer les modifications :**
    
    bash
    
    `kubectl apply -f mon-premier-deploiement.yaml`
    
3. **Observer les ReplicaSets :**
    
    bash
    
    `kubectl get rs --show-labels`
    
    Résultat :
    
    plaintext
    
    `NAME                          DESIRED   CURRENT   READY   AGE nginx-deployment-5956959495   2         3         3       2d23h nginx-deployment-758d787cb    1         1         1       70s`
    
4. **Vérifier les Pods :**
    
    bash
    
    `kubectl get pods --show-labels`
    

---

#### **Rolling update**

Lors d’une mise à jour, Kubernetes effectue un **rolling update**, garantissant une disponibilité minimale des Pods :

- **Minimum Pods disponibles :** 75% (par défaut).
- **Surplus maximal :** 125% (par défaut).

**Observer le processus :**

bash

`kubectl describe deployments`

Sortie typique des événements :

plaintext

`Scaled up replica set nginx-deployment-758d787cb to 1 Scaled down replica set nginx-deployment-5956959495 to 2 from 3 Scaled up replica set nginx-deployment-758d787cb to 3 from 2`

---

#### **Rollback**

1. **Exemple de modification entraînant un échec :**
    
    yaml
    
    `spec:   containers:   - name: nginx-container     image: nginx:1.442`
    
2. **Résultat :**
    
    - Le déploiement échoue avec `ImagePullBackOff` (image inexistante).
    - Pour vérifier :
        
        bash
        
        `kubectl rollout status deployment`
        
3. **Effectuer un rollback :**
    
    bash
    
    `kubectl rollout undo deployment`
    
4. **Vérifier le rollback :**
    
    bash
    
    `kubectl rollout status deployment`
    

---

#### **Historique des déploiements**

1. **Annoter un déploiement :**
    
    bash
    
    `kubectl annotate deployments nginx-deployment kubernetes.io/change-cause="Use image 1.24"`
    
2. **Afficher l’historique :**
    
    bash
    
    `kubectl rollout history deployment`
    
3. **Afficher les détails d’une révision spécifique :**
    
    bash
    
    `kubectl rollout history deployment --revision=2`
    
4. **Effectuer un rollback vers une révision spécifique :**
    
    bash
    
    `kubectl rollout undo deployment --to-revision=2`
    

---

#### **Mise à l’échelle**

1. **Méthode déclarative (recommandée) :**
    
    - Modifier le fichier YAML :
        
        yaml
        
        `spec:   replicas: 10`
        
    - Appliquer les modifications :
        
        bash
        
        `kubectl apply -f mon-premier-deploiement.yaml`
        
2. **Méthode impérative (non recommandée) :**
    
    bash
    
    `kubectl scale deployment nginx-deployment --replicas=10`
    
3. **Vérifier :**
    
    bash
    
    `kubectl get deployment`
    

---

#### **Résumé**

- Les déploiements Kubernetes permettent de gérer facilement la **mise à jour**, la **mise à l’échelle**, et le **rollback** des applications.
- Les **rolling updates** garantissent une disponibilité continue.
- Les **annotations** facilitent le suivi des modifications.
- Les **rollbacks** permettent de revenir à un état stable en cas d’erreur.