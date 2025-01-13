---
tags:
  - "#Intro_Sondes/Utilisation_d_autre_sondes"
---

***
#### **Les types de sondes supplémentaires dans Kubernetes**

En plus des sondes HTTP (**httpGet**), Kubernetes propose trois autres types pour surveiller l'état des conteneurs : **exec**, **tcpSocket**, et **gRPC**.

---

### **1. La sonde exec**

- **Principe :**
    
    - Exécute une commande dans le conteneur.
    - Si la commande retourne un **code de sortie 0**, la sonde est considérée comme réussie.
- **Cas d’usage :**
    
    - Vérification de l’état d’un fichier, d’un processus ou d’un script personnalisé.
- **Exemple YAML :**
	
		apiVersion: v1
		kind: Pod
		metadata:
		  name: mon-application
		spec:
		  containers:
		  - name: mon-application
		    image: mon-image
		    livenessProbe:
		      exec:
		        command:
		        - cat
		        - /tmp/is_alive
		      initialDelaySeconds: 5
		      periodSeconds: 5

- - **Explications :**
        - La commande `cat /tmp/is_alive` est exécutée toutes les 5 secondes.
        - Si le fichier `/tmp/is_alive` existe, la sonde réussit.

---

### **2. La sonde tcpSocket**

- **Principe :**
    
    - Tente d’établir une connexion TCP sur un port spécifié.
    - Si la connexion est réussie, la sonde est considérée comme réussie.
- **Cas d’usage :**
    
    - Vérification de la disponibilité des services qui n'ont pas de point d'entrée HTTP.
- **Exemple YAML :**

		apiVersion: v1
		kind: Pod
		metadata:
		  name: mon-application
		spec:
		  containers:
		  - name: mon-application
		    image: mon-image
		    readinessProbe:
		      tcpSocket:
		        port: 8080
		      initialDelaySeconds: 5
		      periodSeconds: 10

- - **Explications :**
        - Kubernetes tente une connexion TCP sur le port `8080` toutes les 10 secondes.
        - Si la connexion est réussie, Kubernetes considère le conteneur comme prêt.

---

### **3. La sonde gRPC**

- **Principe :**
    
    - Utilise le protocole **gRPC** pour effectuer des contrôles de santé sur des services gRPC.
    - La sonde vérifie si le port gRPC est opérationnel.
- **Cas d’usage :**
    
    - Applications utilisant le protocole gRPC.
- **Exemple YAML :**

		apiVersion: v1
		kind: Pod
		metadata:
		  name: etcd-with-grpc
		spec:
		  containers:
		  - name: etcd
		    image: registry.k8s.io/etcd:3.5.1-0
		    command: [ "/usr/local/bin/etcd", "--data-dir",  "/var/lib/etcd", "--listen-client-urls", "http://0.0.0.0:2379", "--advertise-client-urls", "http://127.0.0.1:2379", "--log-level", "debug"]
		    ports:
		    - containerPort: 2379
		    livenessProbe:
		      grpc:
		        port: 2379
		      initialDelaySeconds: 10

- - **Explications :**
        - La sonde vérifie si le port `2379` est accessible via gRPC.
        - Si l’application gRPC (ici `etcd`) répond, la sonde réussit.

---

### **Résumé des sondes**

|**Type de sonde**|**Principe**|**Cas d’usage**|
|---|---|---|
|**exec**|Exécute une commande dans le conteneur.|Vérifications internes spécifiques (ex. : fichiers, scripts).|
|**tcpSocket**|Tente une connexion TCP sur un port.|Vérification d’un service réseau sans HTTP.|
|**gRPC**|Utilise le protocole gRPC pour tester un port ou une méthode gRPC.|Applications gRPC.|

---

### **Bonnes pratiques**

1. **Choix de la sonde :**
    
    - Utilisez **httpGet** pour les services HTTP/HTTPS.
    - Préférez **exec** pour les vérifications spécifiques à l’application.
    - Utilisez **tcpSocket** si le service n’expose pas d’interface HTTP.
    - **gRPC** est réservé aux applications utilisant ce protocole.
2. **Configuration minutieuse :**
    
    - Ajustez `initialDelaySeconds`, `periodSeconds`, et `failureThreshold` selon les besoins spécifiques de votre application.
    - Testez les sondes sur un environnement local avant de les déployer en production.
3. **Sondes multiples :**
    
    - Combinez différents types de sondes (ex. : readiness avec httpGet et liveness avec exec) pour maximiser la couverture.