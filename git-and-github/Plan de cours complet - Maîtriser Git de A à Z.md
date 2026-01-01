# Plan de cours complet : Maîtriser Git de A à Z

**Git est devenu l'outil incontournable du développement moderne.** Ce cours vous guidera des fondamentaux jusqu'aux méthodologies professionnelles, en passant par la pratique avec GitHub Desktop. À l'issue de cette formation, vous serez capable de versionner vos projets, collaborer efficacement en équipe et gérer les situations complexes comme les conflits de merge.

---

## 📋 Informations générales

| Élément | Détail |
|---------|--------|
| **Public cible** | Développeurs débutants, étudiants en informatique |
| **Prérequis** | Connaissances de base en ligne de commande |
| **Durée estimée** | 12-16 heures |
| **Objectif principal** | Maîtriser Git pour le travail individuel et collaboratif |

---

## 🎯 Objectifs pédagogiques

À la fin de ce cours, l'apprenant sera capable de :

- Comprendre le concept et l'importance du contrôle de version
- Utiliser les commandes Git essentielles en ligne de commande
- Créer, gérer et fusionner des branches
- Résoudre les conflits de merge de manière autonome
- Appliquer la méthodologie Git-flow dans un projet d'équipe
- Utiliser GitHub Desktop pour les opérations courantes
- Connaître les alternatives à GitHub (GitLab, Gitea)

---

## 📚 Structure du cours

### Module 1 : Le principe du versioning (contrôle de version)
**Durée : 1h30 | Théorie**

#### 1.1 Introduction au contrôle de version

**Objectif** : Comprendre pourquoi le versioning est essentiel dans le développement logiciel.

**Contenu :**
- **Définition** : Qu'est-ce que le contrôle de version ?
- **Problématiques résolues** :
  - Le syndrome du "projet_final_v2_vraiment_final.zip"
  - La perte de code et la récupération de versions antérieures
  - La collaboration simultanée sur un même projet
  - La traçabilité des modifications (qui, quoi, quand, pourquoi)

**Exercice pratique** : Réflexion sur les problèmes rencontrés sans versioning dans les projets précédents.

#### 1.2 Les différents systèmes de contrôle de version

**Contenu :**

| Type | Exemples | Caractéristiques |
|------|----------|------------------|
| **Local** | RCS | Historique sur la machine locale uniquement |
| **Centralisé** | SVN, CVS | Un serveur central, dépendance réseau |
| **Distribué** | Git, Mercurial | Chaque copie est un dépôt complet |

**Focus sur Git** :
- Créé par Linus Torvalds en 2005 pour Linux
- Système distribué : travail hors-ligne possible
- Rapidité et efficacité (snapshots, pas de différences)
- Version actuelle : Git 2.52 (2025)

#### 1.3 Concepts fondamentaux de Git

**Le workflow Git en 4 zones** :

```
┌─────────────────┐    git add     ┌─────────────────┐   git commit   ┌─────────────────┐   git push    ┌─────────────────┐
│  Working        │ ─────────────► │  Staging Area   │ ─────────────► │  Local          │ ────────────► │  Remote         │
│  Directory      │                │  (Index)        │                │  Repository     │               │  Repository     │
└─────────────────┘                └─────────────────┘                └─────────────────┘               └─────────────────┘
```

**Concepts clés à maîtriser** :
- **Repository (dépôt)** : Le projet versionné avec tout son historique
- **Commit** : Un instantané (snapshot) des modifications
- **Staging Area** : Zone de préparation avant le commit
- **HEAD** : Pointeur vers le commit actuel
- **Remote** : Version distante du dépôt (GitHub, GitLab...)

**Évaluation Module 1** : QCM sur les concepts fondamentaux (10 questions)

---

### Module 2 : Les commandes principales de Git
**Durée : 3h | Théorie + Pratique**

#### 2.1 Configuration initiale

**Objectif** : Configurer Git correctement avant la première utilisation.

```bash
# Configuration obligatoire (identité)
git config --global user.name "Votre Nom"
git config --global user.email "votre.email@exemple.com"

# Configurations recommandées
git config --global init.defaultBranch main
git config --global core.editor "code --wait"  # VS Code comme éditeur

# Vérifier la configuration
git config --list
```

#### 2.2 Commandes de base essentielles

**Tableau de référence des commandes fondamentales** :

| Commande | Syntaxe | Description |
|----------|---------|-------------|
| `git init` | `git init` | Initialise un nouveau dépôt Git |
| `git clone` | `git clone <url>` | Clone un dépôt distant |
| `git status` | `git status` | Affiche l'état des fichiers |
| `git add` | `git add <fichier>` ou `git add .` | Ajoute au staging |
| `git commit` | `git commit -m "message"` | Enregistre les modifications |
| `git log` | `git log --oneline` | Affiche l'historique |
| `git diff` | `git diff` | Montre les différences non stagées |

**Exercice pratique 2.1** : 
1. Créer un dossier `mon-premier-projet`
2. Initialiser un dépôt Git
3. Créer un fichier `README.md`
4. Faire son premier commit

#### 2.3 Commandes de synchronisation

**Travailler avec un dépôt distant** :

```bash
# Ajouter un dépôt distant
git remote add origin <url>

# Envoyer les modifications
git push origin main
git push -u origin main  # -u définit l'upstream par défaut

# Récupérer les modifications
git pull                  # fetch + merge automatique
git fetch                 # récupère sans fusionner
```

**⚠️ Règle d'or** : Toujours faire `git pull` avant de commencer à travailler !

#### 2.4 Commandes de navigation et annulation

```bash
# Voir l'historique détaillé
git log --oneline --graph --all

# Annuler des modifications
git restore <fichier>           # Annule les modifs non stagées
git restore --staged <fichier>  # Retire du staging
git revert <commit>             # Annule un commit (crée un nouveau commit)
git reset --soft HEAD~1         # Annule le dernier commit (garde les modifs)
```

**Exercice pratique 2.2** :
1. Cloner un dépôt existant depuis GitHub
2. Modifier un fichier et créer un commit
3. Pusher les modifications
4. Simuler une erreur et utiliser `git restore`

**Évaluation Module 2** : TP noté - Workflow complet init → commit → push (30 min)

---

### Module 3 : Les branches
**Durée : 2h | Théorie + Pratique**

#### 3.1 Comprendre le concept de branche

**Définition** : Une branche est un pointeur mobile vers un commit. Elle permet de travailler sur des fonctionnalités de manière isolée.

```
          feature-login
               │
               ▼
    ○──○──○──○──○
   /           
──○──○──○──○──○──○── main
              │
              ▼
         develop
```

**Pourquoi utiliser des branches ?**
- Développer des fonctionnalités sans impacter le code principal
- Permettre le travail parallèle en équipe
- Faciliter les revues de code via Pull Requests
- Isoler les corrections de bugs (hotfix)

#### 3.2 Commandes de gestion des branches

**Tableau des commandes de branchement** :

| Action | Commande |
|--------|----------|
| Lister les branches | `git branch` |
| Créer une branche | `git branch <nom>` |
| Changer de branche | `git switch <nom>` ⭐ (recommandé) |
| Créer et basculer | `git switch -c <nom>` |
| Supprimer une branche | `git branch -d <nom>` |
| Renommer une branche | `git branch -m <ancien> <nouveau>` |

> **Note 2025** : La commande `git switch` est désormais préférée à `git checkout` pour changer de branche car elle est plus intuitive et moins ambiguë.

#### 3.3 Fusionner des branches (merge)

**Types de merge** :

```bash
# Fast-forward merge (linéaire)
git switch main
git merge feature-login

# Merge avec commit de fusion
git merge feature-login --no-ff
```

**Schéma de fusion** :
```
Avant merge:                    Après merge (--no-ff):
                                
feature ──○──○──○               feature ──○──○──○
         /                               /       \
main ───○──○                    main ───○──○──────●── (commit de merge)
```

**Exercice pratique 3.1** :
1. Créer une branche `feature-about-page`
2. Ajouter une page About avec plusieurs commits
3. Revenir sur `main`
4. Fusionner la branche feature
5. Supprimer la branche fusionnée

**Évaluation Module 3** : Exercice de création et fusion de 3 branches distinctes

---

### Module 4 : La gestion des conflits de merge
**Durée : 2h | Théorie + Pratique intensive**

#### 4.1 Comprendre les conflits

**Qu'est-ce qu'un conflit ?**
Un conflit survient quand Git ne peut pas fusionner automatiquement deux modifications qui touchent les mêmes lignes d'un fichier.

**Scénario typique** :
```
Développeur A (main):     "Hello World"  →  "Bonjour le Monde"
Développeur B (feature):  "Hello World"  →  "Hello Universe"

Résultat: CONFLIT! Git ne sait pas quelle version garder.
```

#### 4.2 Anatomie d'un conflit

**Structure d'un fichier en conflit** :
```
<<<<<<< HEAD
Bonjour le Monde
=======
Hello Universe
>>>>>>> feature-branch
```

| Marqueur | Signification |
|----------|---------------|
| `<<<<<<< HEAD` | Début de votre version (branche actuelle) |
| `=======` | Séparateur |
| `>>>>>>> feature` | Fin de la version entrante |

#### 4.3 Résolution étape par étape

**Processus de résolution** :

```bash
# 1. Identifier les fichiers en conflit
git status

# 2. Ouvrir et résoudre manuellement chaque fichier
#    - Choisir une version OU
#    - Combiner les deux versions OU
#    - Réécrire complètement

# 3. Supprimer les marqueurs de conflit

# 4. Marquer comme résolu
git add <fichier-résolu>

# 5. Finaliser le merge
git commit
```

**Bonnes pratiques pour éviter les conflits** :
- Faire des commits petits et fréquents
- Synchroniser régulièrement avec `git pull`
- Communiquer avec l'équipe sur les fichiers en cours de modification
- Découper le travail pour minimiser les chevauchements

#### 4.4 Outils de résolution de conflits

**Options disponibles** :
- **VS Code** : Interface intégrée "Accept Current / Accept Incoming / Accept Both"
- **GitHub Desktop** : Interface visuelle de résolution
- **git mergetool** : Ouvre un outil de diff configurable

```bash
# Configurer un outil de merge
git config --global merge.tool vscode
```

**Exercice pratique 4.1** (Simulation de conflit) :
1. Créer deux branches depuis main
2. Modifier la même ligne différemment dans chaque branche
3. Fusionner la première branche
4. Tenter de fusionner la seconde → CONFLIT
5. Résoudre le conflit manuellement
6. Finaliser le merge

**Évaluation Module 4** : TP noté - Résolution de 3 conflits différents (45 min)

---

### Module 5 : Git-flow (méthodologie)
**Durée : 2h | Théorie + Étude de cas**

#### 5.1 Introduction aux stratégies de branching

**Pourquoi une méthodologie ?**
Sans convention, chaque développeur crée des branches selon sa logique → chaos organisationnel.

**Les 3 stratégies principales en 2025** :

| Stratégie | Complexité | Idéal pour |
|-----------|------------|------------|
| **GitHub Flow** | Simple | Petites équipes, startups, déploiement continu |
| **Git-flow** | Complexe | Grandes équipes, releases planifiées |
| **Trunk-Based** | Moyenne | DevOps avancé, CI/CD mature |

> **💡 Conseil pour débutants** : Commencez par GitHub Flow, puis passez à Git-flow si nécessaire.

#### 5.2 Git-flow en détail

**Les branches Git-flow** :

```
                    hotfix/urgent-fix
                          │
    ┌─────────────────────┼─────────────────────┐
    │                     │                     │
    ▼                     ▼                     ▼
───●────●────●────●────●────●────●────●────●────●─── main (production)
    \         \         /         /
     \         \       /         /
      \         \     /         /
       ○────○────○───○────○────○──────────────────── develop
            \       /    \
             \     /      \
              ○───○        ○───○───○
                │              │
                │              │
          release/v1.0    feature/login
```

**Description des branches** :

| Branche | Rôle | Durée de vie |
|---------|------|--------------|
| `main` | Code en production | Permanente |
| `develop` | Intégration des features | Permanente |
| `feature/*` | Développement de fonctionnalités | Temporaire |
| `release/*` | Préparation d'une release | Temporaire |
| `hotfix/*` | Correction urgente en production | Temporaire |

#### 5.3 Workflow Git-flow pratique

**Cycle de vie d'une feature** :
```bash
# 1. Créer la feature depuis develop
git switch develop
git switch -c feature/user-authentication

# 2. Développer (plusieurs commits)
git commit -m "Add login form"
git commit -m "Add password validation"

# 3. Fusionner dans develop
git switch develop
git merge --no-ff feature/user-authentication
git branch -d feature/user-authentication
```

**Cycle de vie d'une release** :
```bash
# 1. Créer la release depuis develop
git switch -c release/v1.2.0 develop

# 2. Corrections mineures, mise à jour version
git commit -m "Bump version to 1.2.0"

# 3. Fusionner dans main ET develop
git switch main
git merge --no-ff release/v1.2.0
git tag -a v1.2.0 -m "Version 1.2.0"

git switch develop
git merge --no-ff release/v1.2.0
```

#### 5.4 Quand utiliser Git-flow vs alternatives

**Tableau de décision** :

| Contexte | Stratégie recommandée |
|----------|----------------------|
| Projet personnel / études | GitHub Flow |
| Startup, équipe < 5 personnes | GitHub Flow |
| Application avec versions explicites | Git-flow |
| Logiciel bancaire / médical | Git-flow |
| Équipe DevOps, déploiement quotidien | Trunk-Based |

**Exercice pratique 5.1** : 
Simuler un cycle complet Git-flow :
1. Feature → develop → release → main
2. Hotfix depuis main

**Évaluation Module 5** : Étude de cas - Proposer une stratégie de branching pour un projet donné

---

### Module 6 : GitHub avec GitHub Desktop
**Durée : 2h30 | Pratique guidée**

#### 6.1 Introduction à GitHub

**GitHub c'est quoi ?**
- Plateforme d'hébergement de dépôts Git
- Réseau social pour développeurs
- Outils de collaboration (Issues, Pull Requests, Projects)
- CI/CD avec GitHub Actions

**Création de compte et premier dépôt** (démonstration guidée)

#### 6.2 GitHub Desktop - Installation et configuration

**Caractéristiques** :
- Application gratuite et open-source (licence MIT)
- Disponible sur Windows et macOS
- Interface graphique intuitive pour Git

**Installation** :
1. Télécharger depuis [desktop.github.com](https://desktop.github.com)
2. Installer et lancer l'application
3. Se connecter avec son compte GitHub
4. Configurer Git (nom, email)

#### 6.3 Fonctionnalités principales de GitHub Desktop

**Interface de GitHub Desktop** :

| Zone | Fonction |
|------|----------|
| Barre latérale | Liste des dépôts locaux |
| Zone centrale | Fichiers modifiés et diff |
| Historique | Liste des commits |
| Branches | Sélecteur de branche actuelle |

**Opérations courantes** :

| Action | Comment faire dans GitHub Desktop |
|--------|-----------------------------------|
| Cloner un repo | File → Clone Repository |
| Créer un commit | Cocher fichiers + message + "Commit to main" |
| Changer de branche | Menu déroulant "Current Branch" |
| Créer une branche | Branch → New Branch |
| Push | "Push origin" (bouton en haut) |
| Pull | "Fetch origin" puis "Pull origin" |
| Voir les diff | Cliquer sur un fichier modifié |
| Résoudre conflits | Interface visuelle intégrée |

#### 6.4 Workflow complet avec GitHub Desktop

**Exercice pratique 6.1** - Workflow collaboratif :
1. Cloner un dépôt depuis GitHub
2. Créer une branche `feature/ma-contribution`
3. Modifier des fichiers et visualiser les diff
4. Créer un commit avec un message descriptif
5. Pusher la branche
6. Créer une Pull Request sur GitHub
7. Reviewer et merger la PR

#### 6.5 Fonctionnalités avancées

**Fonctionnalités utiles** :
- **Commit partiel** : Sélectionner uniquement certaines lignes à committer
- **Stash** : Mettre de côté des modifications temporairement
- **Historique** : Naviguer et voir les détails de chaque commit
- **Co-auteur** : Ajouter des co-auteurs sur un commit
- **Ouvrir dans VS Code** : Intégration directe avec l'éditeur

**Limites à connaître** :
- Pas de support Linux officiel
- Certaines opérations avancées (rebase interactif) nécessitent la CLI
- Moins flexible que la ligne de commande pour les utilisateurs avancés

**Évaluation Module 6** : TP complet - Collaboration via Pull Request (1h)

---

### Module 7 : Alternatives à GitHub
**Durée : 30 min | Présentation**

#### 7.1 GitLab

**Présentation** :
GitLab est une plateforme DevOps complète, alternative majeure à GitHub, particulièrement appréciée en entreprise.

| Aspect | Détail |
|--------|--------|
| **Type** | Plateforme DevOps tout-en-un |
| **CI/CD** | Intégré nativement (GitLab CI) |
| **Hébergement** | Cloud (gitlab.com) ou self-hosted |
| **Licence** | MIT (Community) / Propriétaire (Enterprise) |
| **Point fort** | DevSecOps intégré, pipelines puissants |

**Quand choisir GitLab ?**
- Besoin de CI/CD intégré sans configuration externe
- Entreprise souhaitant héberger son propre serveur Git
- Projets nécessitant des fonctionnalités de sécurité avancées

#### 7.2 Gitea

**Présentation** :
Gitea est un service Git léger et auto-hébergé, idéal pour les petites équipes et les projets personnels.

| Aspect | Détail |
|--------|--------|
| **Type** | Service Git minimaliste |
| **Ressources** | Très léger (fonctionne sur Raspberry Pi) |
| **Installation** | Exécutable unique en Go |
| **Licence** | MIT (100% gratuit) |
| **CI/CD** | Non natif (Gitea Actions disponible) |

**Slogan** : *"Git with a cup of tea!"* ☕

**Quand choisir Gitea ?**
- Serveur avec ressources limitées
- Projet personnel ou homelab
- Besoin d'une solution simple et légère

#### 7.3 Comparatif rapide

| Critère | GitHub | GitLab | Gitea |
|---------|--------|--------|-------|
| **Facilité** | ★★★★★ | ★★★★☆ | ★★★★★ |
| **CI/CD intégré** | Actions | Natif | Non |
| **Self-hosted** | Enterprise | Oui | Oui |
| **Ressources** | N/A | Élevées | Minimales |
| **Idéal pour** | Open source | Entreprises | Petits projets |

---

### Module 8 : Projet final et évaluation
**Durée : 2h | Évaluation pratique**

#### 8.1 Projet récapitulatif

**Consigne** :
En équipe de 2-3 personnes, créer un projet web simple en appliquant tous les concepts appris :

**Critères d'évaluation** :
- [ ] Dépôt Git correctement initialisé
- [ ] Utilisation de branches (minimum 3 features)
- [ ] Application de Git-flow ou GitHub Flow
- [ ] Au moins une Pull Request par membre
- [ ] Résolution d'au moins un conflit
- [ ] Historique de commits propre et messages descriptifs
- [ ] README.md complet

#### 8.2 QCM final (20 questions)

Couvre tous les modules :
- Concepts de versioning
- Commandes Git
- Gestion des branches
- Résolution de conflits
- Méthodologie Git-flow
- Utilisation de GitHub Desktop

---

## 📎 Annexes

### Aide-mémoire des commandes Git

```bash
# Configuration
git config --global user.name "Nom"
git config --global user.email "email@exemple.com"

# Bases
git init                          # Initialiser
git clone <url>                   # Cloner
git status                        # État des fichiers
git add .                         # Tout ajouter au staging
git commit -m "message"           # Committer
git log --oneline                 # Historique condensé

# Branches
git branch                        # Lister
git switch <branche>              # Changer
git switch -c <nouvelle>          # Créer et changer
git merge <branche>               # Fusionner
git branch -d <branche>           # Supprimer

# Synchronisation
git remote add origin <url>       # Ajouter remote
git push -u origin main           # Premier push
git pull                          # Récupérer et fusionner
git fetch                         # Récupérer sans fusionner

# Annulation
git restore <fichier>             # Annuler modifs
git restore --staged <fichier>    # Désindexer
git revert <commit>               # Annuler un commit
```

### Ressources complémentaires

**Documentation officielle** :
- [git-scm.com/doc](https://git-scm.com/doc) - Documentation Git
- [docs.github.com](https://docs.github.com) - Documentation GitHub

**Outils d'apprentissage interactif** :
- [learngitbranching.js.org](https://learngitbranching.js.org) - Visualisation interactive
- [githowto.com](https://githowto.com) - Tutoriel guidé

**Éditeurs avec intégration Git** :
- Visual Studio Code (extension GitLens recommandée)
- JetBrains IDEs (intégration native)

---

## 📊 Grille d'évaluation globale

| Module | Coefficient | Type d'évaluation |
|--------|-------------|-------------------|
| Module 1 - Versioning | 1 | QCM |
| Module 2 - Commandes | 2 | TP pratique |
| Module 3 - Branches | 2 | Exercice |
| Module 4 - Conflits | 2 | TP pratique |
| Module 5 - Git-flow | 1 | Étude de cas |
| Module 6 - GitHub Desktop | 2 | TP collaboratif |
| Module 8 - Projet final | 3 | Projet + QCM |

**Total : 100 points**

---

*Document créé pour un cours sur les bases de Git - Niveau débutant à intermédiaire*