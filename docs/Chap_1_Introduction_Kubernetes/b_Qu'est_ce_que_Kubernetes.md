---
tags:
  - "#Intro_Kub/Qu_est_ce_que_K8s"
---
***

#### Qu'est ce que Kubernetes ?

**Kubernetes**, souvent abrégé en **K8s** (un numéronyme où 8 représente les lettres entre "K" et "s", comme i18n pour internationalisation), est une plateforme **open-source** écrite en Go, utilisée pour l'orchestration de conteneurs. Il permet de déployer, gérer et superviser des applications conteneurisées à grande échelle.

- **Origines** : Conçu par **Google**, Kubernetes s'inspire de leur système interne appelé **Borg**. Open-sourcé en 2014, il est aujourd’hui soutenu par la **Cloud Native Computing Foundation (CNCF)**, une organisation dépendante de la Fondation Linux, comptant plus de 700 membres (Google, Microsoft, Amazon, Apple, IBM, etc.).
- **Étymologie** : Son nom provient du grec ancien _κυβερνήτης_ (kubernetes), qui signifie "pilote" ou "capitaine", reflétant son rôle de gestionnaire dans un environnement distribué.
- **Surnom** : Certains qualifient Kubernetes de **"système d’exploitation du cloud"** pour sa capacité à automatiser et gérer des déploiements applicatifs dans des environnements complexes.

#### **Principales fonctionnalités de Kubernetes**

1. **Orchestration avancée** :
    
    - Gestion de l'état des applications (de l'état actuel vers l'état désiré).
    - Automatisation des déploiements avec prise en charge des **rolling updates** et **rollback**.
2. **Portabilité** :
    
    - Support des déploiements sur des machines locales, dans des clouds publics, hybrides ou multi-clouds.
3. **Service Discovery & Load Balancing** :
    
    - Fournit des adresses IP ou DNS pour les conteneurs.
    - Gère l'équilibrage de charge pour un trafic réseau élevé.
4. **Gestion du stockage** :
    
    - Intégration avec des systèmes de stockage variés : local, cloud public (GCP, AWS, Azure), ou réseaux de stockage (NFS, Ceph, etc.).
5. **Surveillance et auto-réparation** :
    
    - Redémarrage automatique des conteneurs défaillants.
    - Remplacement ou arrêt des conteneurs non conformes aux **health checks**.
6. **Gestion des secrets et de la configuration** :
    
    - Mise à jour des configurations sans reconstruire les conteneurs ni exposer les secrets.

#### **Pourquoi utiliser Kubernetes ?**

- **Automatisation** : Simplifie la gestion des applications conteneurisées en automatisant leur déploiement, mise à l'échelle, et maintenance.
- **Évolutivité** : Ajuste dynamiquement les ressources selon la demande grâce à l'**auto-scaling**.
- **Résilience** : Assure la haute disponibilité des services avec des mécanismes d’auto-guérison.
- **Interopérabilité** : Permet un déploiement cohérent entre développement, tests et production.

#### **Services managés Kubernetes**

De nombreux fournisseurs cloud proposent des services Kubernetes managés. Ces plateformes simplifient l'utilisation de Kubernetes en prenant en charge les tâches liées à l'infrastructure (installation, maintenance, mises à jour, etc.).

##### Principaux services managés :

- **Google Kubernetes Engine (GKE)** : Intégration native avec Google Cloud.
- **Amazon Elastic Kubernetes Service (EKS)** : S’intègre avec des services AWS comme ELB et RDS.
- **Azure Kubernetes Service (AKS)** : Lié à Azure DevOps et Azure Monitor.
- **IBM Cloud Kubernetes Service** : Connecté aux services d'IBM Cloud, comme Watson.
- **DigitalOcean Kubernetes (DOKS)** : Offre simple et efficace pour les développeurs.
- **Red Hat OpenShift** : Basé sur Kubernetes, avec des fonctionnalités avancées (registre d'images intégré, outils de CI/CD).
- **OVH Managed Kubernetes** : Service de Kubernetes managé par OVHcloud.
- **Scaleway Kubernetes Kapsule** : Service managé avec focus sur la simplicité.
- **Scaleway Kubernetes Kosmos** : Permet d'exploiter des ressources multi-cloud.

##### Alternative notable :

- **Anthos** de Google : Fonctionne sur le principe du multi-cloud et hybride.

---

**En résumé**, Kubernetes a révolutionné le monde des applications modernes grâce à sa capacité à simplifier l’orchestration de conteneurs, à offrir une grande résilience et à s’intégrer facilement dans des environnements variés. Sa popularité repose sur une communauté active et un écosystème robuste soutenu par les plus grands acteurs de l'industrie.
