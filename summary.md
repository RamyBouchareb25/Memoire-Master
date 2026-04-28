# Hostifer — Plan de Mémoire (Master Informatique USTHB)

---

## Chapitre 1 — État de l'Art et Analyse de l'Existant

**Description générale:** Positionnement du projet dans le contexte technologique mondial et algérien. Justification de la nécessité de Hostifer par l'analyse critique des solutions existantes.

---

### 1.1 Le Cloud Computing et les modèles de service

Présentation des trois modèles fondamentaux IaaS, PaaS, SaaS avec leurs caractéristiques, différences, et cas d'usage. Positionner Hostifer clairement dans la catégorie PaaS.

**Figure 1.1** — Pyramide des modèles de service cloud (IaaS / PaaS / SaaS) avec exemples de plateformes pour chaque niveau.

**Tableau 1.1** — Comparaison des responsabilités client/fournisseur dans les trois modèles.

---

### 1.2 L'orchestration de conteneurs — Docker et Kubernetes

Présentation de la conteneurisation Docker, puis de Kubernetes comme standard d'orchestration. Expliquer les concepts clés : Pod, Namespace, Deployment, Service, Ingress, ResourceQuota, NetworkPolicy.

**Figure 1.2** — Architecture d'un cluster Kubernetes (Master node, Worker nodes, etcd, API Server).

**Figure 1.3** — Cycle de vie d'un Pod Kubernetes (Pending → Running → Succeeded/Failed).

---

### 1.3 Analyse des plateformes PaaS internationales

Étude comparative des leaders mondiaux : Vercel, Railway, Render, Heroku, Fly.io, Koyeb. Pour chacun : modèle de déploiement, technologies sous-jacentes, tarification, fonctionnalités clés, et limitations pour le contexte algérien.

**Tableau 1.2** — Comparaison des plateformes PaaS internationales (hébergement, prix, frameworks supportés, CI/CD, free tier).

---

### 1.4 Analyse des initiatives PaaS algériennes

Étude critique des acteurs locaux émergents : Hawiyat Cloud, Deployi, PivoCloud, Sagittario, OpenScaler. Pour chacun : ce qui est proposé, les limitations techniques constatées, et le positionnement par rapport à Hostifer.

**Tableau 1.3** — Comparaison des plateformes PaaS algériennes (fonctionnalités, infrastructure réelle, état de maturité).

---

### 1.5 Le contexte réglementaire algérien

Présentation des lois 18-07 et 25-11, leurs implications pour l'hébergement de données, et l'obligation croissante de souveraineté numérique. Analyse de l'impact de ces lois sur les startups et freelances algériens utilisant des hébergeurs étrangers.

**Tableau 1.4** — Synthèse des obligations légales imposées par les lois 18-07 et 25-11 et leur impact sur l'hébergement cloud.

---

### 1.6 Analyse du marché et identification du besoin

Quantification du marché cible : développeurs, startups, PME. Identification des pain points non adressés par les solutions existantes. Conclusion justifiant la création de Hostifer.

**Figure 1.4** — Diagramme TAM/SAM/SOM du marché algérien du cloud pour développeurs.

**Tableau 1.5** — Synthèse des besoins non satisfaits et réponse apportée par Hostifer.

---

### 1.7 Synthèse et positionnement de Hostifer

Conclusion du chapitre résumant les lacunes identifiées et positionnant Hostifer comme solution unique combinant infrastructure Kubernetes native, hébergement local, tarification DZD, et conformité réglementaire.

**Figure 1.5** — Matrice de positionnement compétitif (axes : qualité technique / conformité algérienne).

---

## Chapitre 2 — Conception et Architecture du Système

**Description générale:** Spécification complète des besoins fonctionnels et non fonctionnels, modélisation UML, et description de l'architecture technique retenue. C'est le chapitre le plus riche en diagrammes.

---

### 2.1 Recueil et spécification des besoins

#### 2.1.1 Identification des acteurs

Description de tous les acteurs interagissant avec le système : Développeur (utilisateur final), Administrateur plateforme, Système GitHub (webhook), Argo Workflows (orchestrateur), Kubernetes API.

#### 2.1.2 Besoins fonctionnels

Liste structurée de toutes les fonctionnalités attendues : authentification, gestion de projets, déploiement, logs, variables d'environnement, CI/CD, facturation, administration.

**Tableau 2.1** — Table des besoins fonctionnels (ID, description, priorité MoSCoW, acteur concerné).

#### 2.1.3 Besoins non fonctionnels

Performance (temps de déploiement < 5 min), disponibilité (SLA 99.9% plans Enterprise), sécurité (isolation réseau, chiffrement), scalabilité (multi-tenant), conformité réglementaire.

**Tableau 2.2** — Table des besoins non fonctionnels (ID, catégorie, critère de satisfaction mesurable).

---

### 2.2 Modélisation UML des cas d'utilisation

#### 2.2.1 Diagramme de cas d'utilisation global

**Figure 2.1** — Diagramme de cas d'utilisation global montrant tous les acteurs et leurs interactions avec le système (authentification, déploiement, gestion des projets, administration, CI/CD, logs).

#### 2.2.2 Cas d'utilisation détaillés

Description textuelle des cas d'utilisation principaux selon le format USTHB standard : nom, acteur, préconditions, scénario nominal, scénarios alternatifs, postconditions.

- CU01 : S'authentifier (email/password + OAuth GitHub)
- CU02 : Créer un projet depuis une URL GitHub
- CU03 : Configurer et déclencher un déploiement
- CU04 : Consulter les logs de build en temps réel
- CU05 : Gérer les variables d'environnement
- CU06 : Configurer le CI/CD (auto-deploy sur push)
- CU07 : Gérer les membres d'une équipe
- CU08 : Supprimer un projet (déprovisionning)
- CU09 : Administrer la plateforme (admin)

**Tableau 2.3** — Tableau récapitulatif des cas d'utilisation (ID, nom, acteur principal, priorité).

---

### 2.3 Modélisation des classes et données

#### 2.3.1 Diagramme de classes

**Figure 2.2** — Diagramme de classes UML complet (User, Project, Deployment, DeploymentStep, EnvVar, Plan, GitHubInstallation, GitHubRepository, ActivityLog, Notification, Domain).

#### 2.3.2 Modèle Entité-Association

**Figure 2.3** — Diagramme Entité-Association de la base de données PostgreSQL avec cardinalités.

#### 2.3.3 Description des entités principales

Tableau descriptif pour chaque entité principale : attributs, types, contraintes, relations.

**Tableau 2.4** — Description de l'entité User (attributs, types, contraintes).
**Tableau 2.5** — Description de l'entité Project.
**Tableau 2.6** — Description de l'entité Deployment.

---

### 2.4 Modélisation comportementale

#### 2.4.1 Diagrammes de séquence

**Figure 2.4** — Diagramme de séquence : Authentification (NextJS → NestJS → PostgreSQL → JWT).

**Figure 2.5** — Diagramme de séquence : Déclenchement d'un déploiement (Frontend → NestJS → Argo Workflows → Build Job → Registry → Helm → DNS).

**Figure 2.6** — Diagramme de séquence : Streaming des logs en temps réel (Builder Pod → Promtail → Loki → NestJS SSE → Browser).

**Figure 2.7** — Diagramme de séquence : Webhook CI/CD GitHub Push (GitHub → NestJS → DeploymentsService → Argo Workflows).

#### 2.4.2 Diagrammes d'activité

**Figure 2.8** — Diagramme d'activité du pipeline de déploiement complet (Build → Provision → DNS → Live).

**Figure 2.9** — Diagramme d'activité de la détection de framework et génération du plan Railpack.

#### 2.4.3 Diagramme d'états

**Figure 2.10** — Diagramme d'états d'un Deployment (QUEUED → BUILDING → PROVISIONING → CONFIGURING\_DNS → LIVE / FAILED / CANCELLED).

---

### 2.5 Architecture technique globale

#### 2.5.1 Vue d'ensemble de l'architecture

**Figure 2.11** — Schéma d'architecture générale de Hostifer : couche client (NextJS), couche API (NestJS), couche orchestration (Argo Workflows), couche infrastructure (Kubernetes, Buildkit, Registry, Loki, Vault), couche réseau (Traefik, Cloudflare, cert-manager).

#### 2.5.2 Architecture multi-tenant

Description du modèle Silo : un Namespace Kubernetes par tenant, isolation via NetworkPolicy, quotas via ResourceQuota et LimitRange.

**Figure 2.12** — Schéma d'isolation multi-tenant : Namespace par projet, NetworkPolicy bloquant le trafic inter-tenant, ressources quotas par plan.

**Tableau 2.7** — Ressources allouées par plan tarifaire (CPU request/limit, RAM request/limit, stockage, pods max).

#### 2.5.3 Architecture du pipeline de build

Description de la chaîne : clonage Git → détection framework → génération plan Railpack → construction image Buildkit → push Registry local.

**Figure 2.13** — Schéma du pipeline de build (Job Kubernetes Go binary → Railpack CLI → Buildkitd Deployment → Registry interne).

#### 2.5.4 Architecture de collecte des logs

Description de la chaîne Promtail → Loki → NestJS SSE endpoint → Browser EventSource.

**Figure 2.14** — Schéma de la chaîne de collecte et streaming des logs (Pod stdout → Promtail DaemonSet → Loki → SSE → Browser).

#### 2.5.5 Architecture de gestion des secrets

Description du flux NestJS → Vault KV v2 → ESO ExternalSecret → Kubernetes Secret → Pod envFrom.

**Figure 2.15** — Schéma de gestion des secrets avec HashiCorp Vault et External Secrets Operator.

---

### 2.6 Choix technologiques

Justification argumentée de chaque technologie retenue.

**Tableau 2.8** — Tableau des choix technologiques (composant, technologie retenue, alternatives considérées, justification du choix).

---

## Chapitre 3 — Réalisation et Implémentation

**Description générale:** Description de l'implémentation concrète, présentation des interfaces, validation par les tests, et bilan du déploiement.

---

### 3.1 Environnement de développement et de déploiement

#### 3.1.1 Infrastructure matérielle

Description du serveur homelab utilisé pour le développement et les tests : caractéristiques matérielles, système d'exploitation, k3s comme distribution Kubernetes légère.

**Tableau 3.1** — Caractéristiques de l'environnement de déploiement (CPU, RAM, stockage, OS, distribution Kubernetes).

#### 3.1.2 Outils de développement

**Tableau 3.2** — Environnement de développement (IDE, outils CLI, chaîne CI/CD, gestionnaires de paquets).

#### 3.1.3 Organisation des dépôts GitHub

Description de l'organisation GitHub : dépôt frontend NextJS, dépôt backend NestJS, dépôt builder Go, chart Helm OCI sur GHCR, images Docker sur GHCR.

**Figure 3.1** — Schéma d'organisation des dépôts GitHub et registres OCI.

---

### 3.2 Implémentation du backend NestJS

#### 3.2.1 Structure modulaire de l'application

**Figure 3.2** — Arborescence du projet NestJS avec description de chaque module (Auth, Users, Projects, Deployments, Logs, EnvVars, GitHub, Plans, Infrastructure).

#### 3.2.2 Module d'authentification

Description de l'implémentation JWT avec Passport, OAuth GitHub délégué au backend, refresh token rotation dans le callback NextAuth jwt.

**Figure 3.3** — Diagramme de séquence détaillé du refresh token (jwt callback → NestJS /auth/refresh → nouvelle paire de tokens → mise à jour session).

#### 3.2.3 Module de déploiement et orchestration Argo

Description de la soumission de workflows Argo via REST API depuis NestJS, synchronisation du statut via polling, mise à jour des enregistrements Deployment et DeploymentStep.

#### 3.2.4 Module de streaming des logs

Description de l'endpoint SSE NestJS utilisant `@Sse()`, interrogation de Loki `query_range` toutes les 2 secondes, filtrage par `build_id` et `container="main"`.

#### 3.2.5 Module de gestion des variables d'environnement avec Vault

Description de l'intégration HashiCorp Vault : authentification Kubernetes, écriture/lecture KV v2, synchronisation ESO.

---

### 3.3 Implémentation du builder Go

#### 3.3.1 Structure du binaire

**Figure 3.4** — Arborescence du projet Go builder (main.go, git.go, railpack.go, buildkit.go, logger.go).

#### 3.3.2 Détection de framework et génération du plan Railpack

Description de la génération du plan via CLI Railpack, patch du plan JSON pour UID 1000, injection de `NODE_OPTIONS` pour la gestion mémoire Node.js.

**Figure 3.5** — Extrait du plan railpack-plan.json généré pour une application Node.js avec les modifications appliquées.

#### 3.3.3 Construction et push de l'image via Buildkit

Description de la connexion au daemon Buildkit, résolution du frontend Railpack gateway, streaming des logs vertex par vertex.

---

### 3.4 Implémentation de l'infrastructure Kubernetes

#### 3.4.1 Chart Helm multi-tenant

Description des ressources créées par le chart Helm pour chaque tenant : Namespace, ResourceQuota, LimitRange, NetworkPolicy x4, ServiceAccount, Deployment, Service, IngressRoute, ExternalSecret.

**Figure 3.6** — Extrait du template NetworkPolicy illustrant l'isolation inter-tenant et l'autorisation Traefik uniquement.

#### 3.4.2 Pipeline Argo Workflows

Description du WorkflowTemplate à trois étapes séquentielles (Build → Provision → DNS), paramétrage dynamique, gestion RBAC.

**Figure 3.7** — Capture d'écran de l'interface Argo Workflows UI montrant un workflow de déploiement complet avec les trois étapes.

#### 3.4.3 Configuration de l'observabilité (Loki + Promtail)

Description de la configuration Promtail DaemonSet, relabeling pour attacher `build_id`, `tenant_id`, `log_type`, filtre `container="main"`.

---

### 3.5 Présentation des interfaces utilisateur

#### 3.5.1 Page d'accueil (Landing Page)

**Figure 3.8** — Capture d'écran de la section Hero de la landing page.
**Figure 3.9** — Capture d'écran de la section Pricing.
**Figure 3.10** — Capture d'écran de la section comparaison concurrentielle.

#### 3.5.2 Tableau de bord utilisateur

**Figure 3.11** — Capture d'écran du dashboard avec liste des projets et statuts.
**Figure 3.12** — Capture d'écran de la page de détail d'un projet.

#### 3.5.3 Assistant de déploiement (Deploy Wizard)

**Figure 3.13** — Capture d'écran de l'étape 1 : validation de l'URL GitHub et détection du framework.
**Figure 3.14** — Capture d'écran de l'étape 2 : configuration (région, plan, variables d'environnement).
**Figure 3.15** — Capture d'écran de l'étape 3 : streaming des logs de build en temps réel.
**Figure 3.16** — Capture d'écran de l'étape 4 : confirmation du déploiement réussi avec URL live.

#### 3.5.4 Gestion des variables d'environnement

**Figure 3.17** — Capture d'écran de l'interface de gestion des variables d'environnement avec masquage des secrets.

#### 3.5.5 Interface d'administration Argo Workflows

**Figure 3.18** — Capture d'écran de l'interface Argo Workflows montrant les étapes Build, Provision, DNS d'un déploiement réel.

---

### 3.6 Tests et validation

#### 3.6.1 Tests fonctionnels

Validation des cas d'utilisation principaux sur des applications réelles représentatives de différents frameworks.

**Tableau 3.3** — Résultats des tests de déploiement par framework (Express, NestJS, Next.js, Flask, Laravel) : statut, durée de build, mémoire consommée.

#### 3.6.2 Tests d'isolation multi-tenant

Validation que les NetworkPolicies empêchent effectivement la communication inter-namespace.

**Tableau 3.4** — Résultats des tests d'isolation réseau (tentative de connexion inter-tenant, résultat attendu vs observé).

#### 3.6.3 Tests de charge et performance

Mesures de temps de déploiement de bout en bout, latence des logs SSE, consommation mémoire buildkitd sous charge simultanée.

**Figure 3.19** — Graphique du temps de déploiement moyen par framework (de la soumission du workflow à l'état LIVE).

**Tableau 3.5** — Tableau de performance : temps de build moyen, latence SSE, consommation mémoire buildkitd pour 1, 3 et 5 builds simultanés.

#### 3.6.4 Tests de conformité réglementaire

Vérification que toutes les données restent dans l'infrastructure algérienne, que les logs d'audit sont correctement enregistrés, que les secrets ne transitent jamais en clair.

**Tableau 3.6** — Checklist de conformité (critère, mécanisme technique de conformité, statut vérifié).

---

### 3.7 Bilan et perspectives

#### 3.7.1 Fonctionnalités réalisées

**Tableau 3.7** — Tableau de couverture fonctionnelle (besoin identifié au chapitre 2, statut d'implémentation : réalisé / partiel / reporté).

#### 3.7.2 Difficultés rencontrées

Description honnête des défis techniques surmontés : compatibilité musl/glibc pour le binaire Railpack, parsing des logs Loki, gestion du refresh token NextAuth, isolation RBAC Argo Workflows.

#### 3.7.3 Perspectives d'évolution

Fonctionnalités futures prévues : cold start avec KEDA, bases de données managées, stockage objet S3-compatible, migration vers DjezzyCloud pour la haute disponibilité, support des organisations GitHub.

---

## Conclusion Générale

Synthèse des contributions du projet sur trois axes : contribution technique (architecture Kubernetes multi-tenant native, pipeline de build automatisé), contribution économique (modèle DZD-natif, free tier), contribution sociétale (souveraineté numérique algérienne, conformité réglementaire). Ouverture sur le potentiel de Hostifer à devenir une infrastructure critique pour l'écosystème numérique algérien.

---

## Bibliographie

Références structurées en catégories : documentation officielle Kubernetes, articles académiques sur le cloud multi-tenant, documentation des outils (Argo Workflows, Loki, Vault, Railpack, Buildkit, Traefik), textes législatifs algériens (loi 18-07, loi 25-11), rapports de marché sur le numérique en Algérie.

---

## Annexes

**Annexe A** — Manifest complet du WorkflowTemplate Argo Workflows.
**Annexe B** — Chart Helm tenant-chart : valeurs par défaut et templates principaux.
**Annexe C** — Configuration Promtail complète (relabeling rules).
**Annexe D** — Politique Vault et configuration ESO ClusterSecretStore.
**Annexe E** — Extrait du schéma Prisma complet.
**Annexe F** — Glossaire des termes techniques.