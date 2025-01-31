---
title: Principaux objets et description d'un objet Kubernetes
tags:
  - "#Principaux_Objet/Desc_Objet_Kub"
---

## **Créer des objets Kubernetes**

Pour créer un objet dans Kubernetes, vous devez fournir une **spécification (spec)** qui décrit son **état souhaité**, ainsi que des informations de base sur l’objet (comme son nom). Kubernetes utilise ces spécifications pour maintenir automatiquement l'état désiré.

### **Méthodes de création :**

1. **Directement via l’API Kubernetes** :
    - En envoyant des requêtes HTTP avec un corps au format **JSON**.
2. **Via des fichiers YAML avec kubectl** (méthode la plus courante) :
    - YAML est traduit par kubectl en requêtes API JSON.
    - Exemple de commande :

        ```bash
        kubectl apply -f mon_fichier.yaml
        ```

---

### **Endpoints principaux de l'API Kubernetes**

Voici les endpoints couramment utilisés pour interagir avec l’API Kubernetes :

- **/healthz** : Vérification de l’état de santé (vivacité et disponibilité) du serveur API.
  - **Liveness** : Vérifie si le serveur fonctionne.
  - **Readiness** : Vérifie si le serveur est prêt à traiter des requêtes.
- **/metrics** : Expose des métriques au format Prometheus, telles que l’utilisation de la mémoire, du CPU, et les temps de réponse.
- **/api** : Accès aux ressources de base comme les Pods, Services, et Volumes.
- **/apis** : Extension de l’API pour des ressources supplémentaires ou avancées.
- **/version** : Retourne la version du serveur API.
- **/openapi/v3** : Fournit les spécifications OpenAPI.
- **/readyz** et **/livez** : Endpoints spécifiques pour les probes de vivacité et de disponibilité.

> **Accès aux endpoints :**  
> Pour y accéder, lancez un proxy local avec :

  ```bash
  kubectl proxy --port=8080
  ```

Ensuite, visitez une URL comme :

> http://localhost:8080/apis/apps/v1

---

### **Versionnement des API Kubernetes**

Les versions des API permettent de gérer l’évolution et la compatibilité entre différentes versions de Kubernetes.

#### **Niveaux de versionnement :**

1. **Alpha** :
    - Indiqué par `alpha` (ex. : `v1alpha1`).
    - Désactivé par défaut.
    - Instable, susceptible de changer ou d’être supprimé.
2. **Beta** :
    - Indiqué par `beta` (ex. : `v2beta3`).
    - Bien testé, mais le schéma peut encore évoluer.
    - Désactivé par défaut.
3. **Stable** :
    - Indiqué par `vX` (ex. : `v1`).
    - Garantit une compatibilité future pour toutes les versions mineures de Kubernetes.

---

### **Groupes d’API Kubernetes**

Les groupes d'API permettent d’organiser les types de ressources et leurs versions.

1. **Groupe "Core" (Legacy)** :
    - Contient les ressources de base comme Pods, Services, et ConfigMaps.
    - Accessible via `/api/v1`.
2. **Groupes nommés** :
    - Introduits pour gérer des ressources avancées.
    - Exemple : `/apis/apps/v1` pour les Deployments, ReplicaSets, etc.
    - Le champ `apiVersion` combine le groupe et la version, par exemple :

      ```yaml
      apiVersion: batch/v1
      ```

---

### **Bref rappel sur YAML**

Kubernetes utilise YAML pour les fichiers de configuration. Voici les concepts clés :

- **Indentation :** Utilisez des espaces, pas des tabulations.
- **Listes :** Chaque élément commence par un tiret (`-`).
- **Dictionnaires :** Clés et valeurs séparées par `:`.

#### **Exemple de fichier YAML :**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: node-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: node
  template:
    metadata:
      labels:
        app: node
    spec:
      containers:
      - name: node
        image: node:alpine
        ports:
        - containerPort: 80
```

---

### **Champs obligatoires pour les objets Kubernetes**

1. **apiVersion** : La version de l'API utilisée (ex. : `apps/v1`).
2. **kind** : Le type d'objet (ex. : `Deployment`, `Pod`, `Service`).
3. **metadata** : Identifie l’objet de manière unique, avec des champs comme :
    - `name` : Nom de l'objet.
    - `namespace` : Optionnel, pour organiser les objets.
4. **spec** : Décrit l’état souhaité pour l’objet.

Chaque type d'objet a des champs spécifiques dans la section **spec**, adaptés à son rôle (ex. : réplicas pour un Deployment, ports pour un Service).

---

### **Résumé**

- Les objets Kubernetes sont créés via des fichiers YAML ou des requêtes API.
- Les endpoints API permettent un contrôle direct mais sont souvent masqués par des outils comme **kubectl**.
- Le versionnement et les groupes d'API assurent la compatibilité et l’évolutivité.
- YAML est le format privilégié pour la configuration des objets, avec des champs obligatoires tels que **apiVersion**, **kind**, **metadata**, et **spec**.

Tu es maintenant prêt à configurer et gérer des objets Kubernetes en toute confiance !
