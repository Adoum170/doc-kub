---
tags:
  - "#Intro_Kub/Mise_En_Place_Linux"
---
***

#### **Qu'est-ce que Minikube ?**

**Minikube** est un outil conçu pour exécuter un cluster Kubernetes local sur votre machine. Il est principalement destiné au développement et aux tests, permettant aux développeurs de travailler sur Kubernetes sans avoir besoin d'un cluster distant.

- **Fonctionnalités principales :**
    
    - Configure une machine virtuelle ou utilise Docker pour exécuter Kubernetes localement.
    - Compatible avec les systèmes d'exploitation **Linux**, **macOS** et **Windows**.
    - Inclut des fonctionnalités Kubernetes courantes telles que :
        - **DNS**
        - **NodePorts**
        - **ConfigMaps**
        - **Secrets**
- **Origine :** Développé par la communauté Kubernetes (avec une contribution significative de Google).
    
- **Utilisation principale :** Développement et tests locaux.
    
- **Alternatives :**
    
    - **kind** (Kubernetes IN Docker)
    - **k3s** (une version allégée de Kubernetes)

---

#### **Le CLI kubectl**

**kubectl** (prononcé "cube control") est l'outil en ligne de commande officiel pour interagir avec Kubernetes. Il sert de client pour communiquer avec le cluster en utilisant l'API Kubernetes.

- **Fonctionnement :**
    
    - Traduit des commandes en ligne ou des configurations YAML en requêtes HTTP.
    - Ces requêtes sont envoyées au serveur Kubernetes avec les données nécessaires au format JSON.
- **Principales tâches réalisables avec kubectl :**
    
    - **Gestion des ressources** : Créer, mettre à jour, supprimer des Pods, Services, Deployments, etc.
    - **Consultation d'état** : Obtenir les détails et le statut des ressources Kubernetes.
    - **Gestion réseau et stockage** : Configurer et superviser les volumes et réseaux du cluster.
    - **Interactivité** : Exécuter des commandes à l'intérieur des conteneurs d'un Pod.
    - **Débogage** : Analyser les journaux des Pods et résoudre les problèmes du cluster.

---

#### **Installation de Minikube**

**Prérequis minimaux :**

- 2 CPU
- 4 Go de RAM libre
- 20 Go d’espace disque
- Gestionnaire de machine virtuelle ou de conteneurs (par exemple Docker)

**Installation sur Linux :**

1. Téléchargez le binaire :
    
    bash
    
    `curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64`
    
2. Installez-le :
    
    bash
    
    `sudo install minikube-linux-amd64 /usr/local/bin/minikube`
    

---

#### **Lancement d’un cluster avec Minikube**

1. Lancez Docker Desktop pour fournir un environnement de conteneur.
2. Configurez Minikube pour utiliser Docker comme pilote :
    
    bash
    
    `minikube config set driver docker`
    
3. Démarrez le cluster :
    
    bash
    
    `minikube start`
    

> **Remarque** : La commande `minikube start` télécharge les pilotes nécessaires et crée une machine virtuelle ou un environnement Docker. Cela peut prendre un peu de temps lors de la première exécution.

---

#### **Installation de kubectl**

**Installation sur Linux :**

1. Téléchargez le binaire kubectl :
    
    bash
    
    `curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl`
    
2. Rendez le fichier exécutable :
    
    bash
    
    `chmod +x ./kubectl`
    
3. Déplacez-le dans votre `PATH` :
    
    bash
    
    `sudo mv ./kubectl /usr/local/bin/kubectl`
    
4. Vérifiez la version :
    
    bash
    
    `kubectl version --client`
    

---

#### **Activation de l'auto-complétion pour Bash**

1. Installez le package `bash-completion` :
    
    bash
    
    `sudo apt-get install bash-completion`
    
2. Ajoutez l’auto-complétion au fichier `.bashrc` :
    
    bash
    
    `echo 'source <(kubectl completion bash)' >>~/.bashrc`
    
3. Rechargez le terminal :
    
    bash
    
    `source ~/.bashrc`
    

**Testez l’auto-complétion :**

- Tapez `kubectl` suivi d’une lettre (par ex. `g`) puis appuyez sur la touche **Tab** pour voir les suggestions.

---

#### **Résumé**

- **Minikube** : Un outil simple et rapide pour exécuter Kubernetes localement.
- **kubectl** : Le client officiel pour interagir avec Kubernetes, indispensable pour gérer les ressources et superviser le cluster.
- **Installation et configuration** :
    - Minikube nécessite Docker ou une machine virtuelle pour fonctionner.
    - kubectl est facile à installer et son auto-complétion améliore considérablement l'expérience utilisateur.

Avec ces outils, vous disposez d’un environnement local complet pour apprendre, développer et tester Kubernetes avant de passer à des environnements de production.