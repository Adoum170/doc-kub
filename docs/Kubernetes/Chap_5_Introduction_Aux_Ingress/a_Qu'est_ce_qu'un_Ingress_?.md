---
tags:
  - "#Intro_Ingress/Qu_est_ce_que_Ingress"
---
***

#### **Qu'est-ce qu'un Ingress ?**

Un **Ingress** dans Kubernetes est une **entrée réseau** permettant d’exposer des services HTTP ou HTTPS à l'extérieur d'un cluster Kubernetes. Il sert de **point d’entrée** pour acheminer le trafic entrant vers les services appropriés.

---

#### **Possibilités offertes par un Ingress**

1. **Exposition d’applications web :**
    
    - Permet d’exposer des applications web hébergées dans le cluster sur des URLs accessibles depuis Internet ou d'autres réseaux externes.
2. **Gestion du trafic :**
    
    - Routage basé sur des règles spécifiques :
        - **URI** : Acheminer le trafic en fonction des chemins d'URL.
        - **Noms d'hôtes virtuels** : Redirection selon le domaine (ex. : `app.example.com`).
        - **En-têtes HTTP** : Acheminer selon des métadonnées spécifiques.
3. **TLS et chiffrement :**
    
    - Gère la **terminaison TLS** pour sécuriser les connexions HTTPS.
    - Assure la confidentialité et l’intégrité des données échangées.
4. **Gestion des noms d’hôtes virtuels :**
    
    - Héberge plusieurs sites ou applications sur une même adresse IP grâce à des noms d’hôtes distincts (par exemple : `site1.example.com` et `site2.example.com`).

---

#### **Fonctionnement d’un Ingress**

1. **Requête entrante :**
    
    - Un client externe (navigateur web ou application) envoie une requête HTTP ou HTTPS.
2. **Traitement par le contrôleur d’Ingress :**
    
    - Le **contrôleur d’Ingress** reçoit la requête et compare ses détails (URL, domaine, en-têtes) aux règles définies dans la ressource Ingress.
3. **Routage vers un Service Kubernetes :**
    
    - Si la requête correspond à une règle :
        - Le contrôleur achemine la requête vers le **Service** correspondant.
    - Si aucune règle ne correspond et qu’un **backend par défaut** est configuré :
        - La requête est acheminée vers ce backend par défaut.
4. **Transfert vers un Pod :**
    
    - Le Service transmet la requête au Pod approprié (en fonction de ses règles internes).
5. **Retour de la réponse :**
    
    - Le Pod gère la requête et renvoie une réponse qui suit le même chemin (en sens inverse) jusqu’au client.

---

#### **Résumé des étapes :**

1. **Requête client → Ingress.**
2. **Ingress Controller** applique les règles.
3. **Routage** vers le Service correspondant.
4. **Service → Pod approprié.**
5. **Pod → Réponse retournée au client.**

---

#### **Cas d’usage des Ingress**

- **Hébergement multi-domaines :** Héberger plusieurs applications sur une même adresse IP, différenciées par leurs noms d’hôtes.
- **Sécurisation :** Utilisation du protocole HTTPS pour les communications.
- **Gestion de trafic avancée :** Acheminer le trafic vers différentes versions d'une application ou appliquer des règles spécifiques selon les besoins.

---

#### **Illustration**

Voici une représentation schématique d’un Ingress dans Kubernetes :

plaintext


`[Client externe] ---> [Ingress] ---> [Service] ---> [Pod(s)]`

---

#### **Prochaines étapes dans la formation**

Dans les prochaines leçons, nous approfondirons :

- **Configuration d’un Ingress** (création et gestion des règles).
- **Intégration avec des certificats TLS**.
- **Installation et gestion d’un contrôleur d’Ingress**.