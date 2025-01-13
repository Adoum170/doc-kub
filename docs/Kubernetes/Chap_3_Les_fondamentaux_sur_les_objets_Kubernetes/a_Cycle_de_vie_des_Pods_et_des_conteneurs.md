---
tags:
  - "#Objet/Cycle_de_vie_Pod_et_conteneurs"
---
***

#### **Durée de vie d'un Pod**

Les Pods sont des **objets éphémères** et non durables dans Kubernetes. Lorsqu'un Pod est créé, il reçoit un ID unique et est assigné à un nœud. Si le nœud devient indisponible, les Pods qu’il héberge sont supprimés, et Kubernetes en crée de nouveaux pour les remplacer.

> **Important** : Les Pods recréés auront un nouvel ID, ce seront donc des entités distinctes.

---

#### **Phases d'un Pod**

Un Pod passe par plusieurs phases dans son cycle de vie. Ces phases reflètent un **résumé de haut niveau** de son état :

1. **Pending** :
    
    - Le Pod est accepté par Kubernetes, mais ses conteneurs ne sont pas encore créés.
    - Cela inclut le temps nécessaire pour planifier le Pod ou télécharger les images.
2. **Running** :
    
    - Le Pod est lié à un nœud, et tous les conteneurs sont en cours d'exécution ou démarrent.
3. **Succeeded** :
    
    - Tous les conteneurs se sont terminés avec succès et ne redémarrent pas.
4. **Failed** :
    
    - Tous les conteneurs se sont arrêtés, mais au moins un d'entre eux s'est terminé avec une erreur.
5. **Unknown** :
    
    - Kubernetes ne peut pas déterminer l’état du Pod, généralement à cause d’un problème de communication avec le nœud.

> **Statut `Terminating` :** Lorsqu’un Pod est en cours de suppression, il apparaît comme `Terminating`. Cela n’est pas une phase mais un état temporaire.

---

#### **États des conteneurs dans un Pod**

Les conteneurs d'un Pod ont leurs propres états, indépendamment de la phase du Pod :

1. **Waiting** :
    
    - Le conteneur est en attente d’exécution (par exemple, téléchargement d'une image).
    - Une raison est souvent indiquée (par exemple, `ImagePullBackOff`).
2. **Running** :
    
    - Le conteneur est en cours d’exécution.
    - Informations disponibles : heure de démarrage, état actuel.
3. **Terminated** :
    
    - Le conteneur s'est arrêté, volontairement ou à cause d’une erreur.
    - Donne des détails sur le **code de sortie**, la raison, l’heure de début et de fin.

---

#### **Politique de redémarrage des conteneurs**

Le champ **RestartPolicy** dans la spécification du Pod définit comment Kubernetes doit gérer les redémarrages des conteneurs :

- **Always** (par défaut) : Les conteneurs redémarrent toujours.
- **OnFailure** : Les conteneurs redémarrent uniquement en cas d'échec.
- **Never** : Les conteneurs ne redémarrent jamais.

> **Redémarrage géré par kubelet :**  
> Si un conteneur échoue, le kubelet tente de le redémarrer avec un délai exponentiel plafonné à 5 minutes.

---

#### **Conditions des Pods**

Un Pod a un **PodStatus** qui inclut des conditions. Chaque condition reflète une étape clé dans son état :

- **PodScheduled** : Le Pod a été assigné à un nœud.
- **ContainersReady** : Tous les conteneurs sont prêts.
- **Initialized** : Toutes les initialisations nécessaires ont été effectuées.
- **Ready** : Le Pod peut traiter des requêtes et être ajouté aux pools d'équilibrage de charge.

**Commande pour visualiser les conditions :**

bash


`kubectl describe pods`

Exemple de sortie :

plaintext

Copier le code

`Conditions:   Type              Status   Initialized       True   Ready             True   ContainersReady   True   PodScheduled      True`

---

#### **Utilisation de l'option `--output` dans kubectl**

L’option **`--output`** (ou **`-o`**) permet de formater la sortie des commandes kubectl dans différents formats.

##### Formats disponibles :

1. **json** : Retourne la configuration complète au format JSON.
    
    bash
    
    `kubectl get pods -o json`
    
2. **yaml** : Retourne la configuration complète au format YAML.
    
    bash
    
    `kubectl get pods -o yaml`
    
3. **name** : Affiche uniquement les noms des Pods.
    
    bash
    
    `kubectl get pods -o name`
    
    Exemple de sortie :
    
    plaintext
    
    `pod/nginx-deployment-676dc4898c-j8dvc`
    
4. **wide** : Fournit des informations supplémentaires (IP, nœud, etc.).
    
    bash
    
    `kubectl get pods -o wide`
    
    Exemple de sortie :
    
    plaintext
    
    `NAME                                READY   STATUS    RESTARTS   AGE   IP            NODE       NOMINATED NODE   READINESS GATES nginx-deployment-676dc4898c-j8dvc   1/1     Running   0          42h   10.244.0.27   minikube   <none>           <none>`
    
5. **jsonpath** : Permet d’extraire des champs spécifiques au format JSON.
    
    bash
    
    `kubectl get pods -o jsonpath='{.items[0].metadata.name}'`
    
    Exemple de sortie :
    
    plaintext
    
    `nginx-deployment-676dc4898c-j8dvc`
    

---

#### **Résumé**

- Les **Pods** ont un cycle de vie éphémère. Leur recréation ne conserve pas leur ID.
- Les **phases des Pods** offrent un résumé de leur état, tandis que les conteneurs ont des états plus détaillés.
- Les politiques de redémarrage définissent le comportement des conteneurs en cas d'échec.
- Les **conditions des Pods** permettent de suivre leur progression vers un état opérationnel.
- L’option **`--output`** dans kubectl facilite l’extraction et la visualisation des informations sur les ressources.
- 