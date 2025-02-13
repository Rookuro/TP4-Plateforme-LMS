# TP4 Plateforme LMS

### Contexte : 
Vous êtes une équipe d’architectes logiciels en charge de concevoir une plateforme de gestion d’apprentissage en ligne (LMS) destinée aux universités et centres de formation. Cette plateforme doit être modulable, scalable et maintenable pour répondre aux besoins évolutifs des établissements partenaires.

Votre mission est de proposer une architecture intégrant à la fois une logique Microkernel et Microservices, en justifiant vos choix techniques et conceptuels.

### Fonctionnalités à gérer : 
- Gestion des utilisateurs : Inscription, connexion, gestion des rôles (étudiant, enseignant, administrateur)
- Gestion des cours : Création, édition, organisation en modules, partage de documents
- Système de correction automatique : Correction d’examens, évaluations de code et soumissions de devoirs
- Moteur de recommandation : Suggestions de cours basées sur l’historique des étudiants
- Forum et messagerie : Communication entre étudiants et enseignants
- Gestion des certificats et badges : Attribution et validation des acquis

## Cartographie de l'Architecture
 - Authentication Manager : Service isolé pour assurer la sécurité (hashing, gestion des tokens, authentification centralisée).
 - Ressources Manager : Gestion des outils pédagogiques en architecture microkernel (chaque plugin correspond à un type de       ressource).
 - Course Manager : Gestion et attribution des cours, intégration des ressources.

 - Assessment Manager : Gestion des examens, corrections et soumissions de devoirs.

 - Achievement Handler : Attribution et gestion des badges/certificats.

 - Recommandation Service : Recommandation de cours basés sur l'historique utilisateur.

 - Messaging System : Service de messagerie.

 - Notification Emitter : Gestion des notifications et emails.

## Distinction entre Microkernel et Microservices : 

 | Nom du Service       | MicroKernel | MicroService | BDD             |
|----------------------|------------|--------------|----------------|
| Authentication Manager |            | ✅          | Externe: LDAP  |
| User Manager        |            | ✅          | PostgreSQL     |
| Ressources Manager  | ✅         |              | PostgreSQL, AWS S3 |
| Course Manager      |            | ✅          | PostgreSQL     |
| Assessment Manager  |            | ✅          | PostgreSQL     |
| Achievement Handler |            | ✅          | PostgreSQL     |
| Recommandation Service |            | ✅          | GraphQL        |
| Messaging System    |            | ✅          | MongoDB        |
| Notification Emitter |            | ✅          | MongoDB        |


## Justification des choix d'architecture

### Pourquoi Microkernel et Microservices ?

 - Ressources Manager : Architecture en plugins permettant l'ajout facile de nouvelles ressources avec leur propre comportement (via Prism).

 - Autres services : Architecture en couches, suffisante pour la complexité métier.

### Bénéfices en maintenance et évolutivité 

- Indépendance des services → Évolution sans impacter les autres.

- Déploiement autonome des microservices.

- Tests isolés pour une meilleure fiabilité.

- Tolérance accrue aux pannes.


### Exemples d'implémentations techniques

    1. Exposition d'un Microservice : API REST/gRPC.

    2. Mécanisme d’extension Microkernel : Relation entre le noyau et ses plugins.

## Technologies utilisées

### Langages
    
- C# avec AspNetCore pour API RESTful et GrpcDotNet pour les API gRPC.

- Langage maîtrisé par l’équipe.

### Message Broker

- Mise en place d'une Event Queue assurant la communication asynchrone entre les services via le protocole AMQP.
- Amélioration de la scalabilité et de la résilience du système.
- Utilisation de RabbitMQ, parfaitement intégré à l'écosystème .NET grâce à MassTransit.

### Orchestration

- CI/CD : GitLab AutoDevOps pour déploiement sur GCP via GKE.

- GitOps & Infrastructure as Code : Terraform pour gestion du cluster.

- FluxCD : Mise à jour automatique du cluster.

- Helm : Gestion des charts Kubernetes.

- Scaling : Vertical et horizontal.

### Sécurité et Observabilité

- 🔑 Authentification et Autorisation : Sécurisation TLS/HTTPS, API Gateway avec rate limiting.

- 🔍 ZTA (Zero Trust Architecture) : Vérification stricte des identités.

- 🛡 Chiffrement des données : Protection contre les fuites.

- 📊 Monitoring et Logs :

    - ELK Stack pour centralisation des logs.

    - Grafana pour visualisation des métriques.

    - OpenTelemetry pour suivi des services.

    - Définition des SLO et alertes personnalisées.

    - Jaeger pour tracing distribué.

    - Istio pour circuit breaker et résilience.

##  Évolution future du système

- Ajout de nouveaux services et plugins pour accroître la configurabilité.

- Optimisation des performances (scaling et équilibrage).

- Sécurité renforcée avec détection et prévention des attaques.

- Intégration de l'IA pour analyse des performances utilisateurs et recommandations avancées.
