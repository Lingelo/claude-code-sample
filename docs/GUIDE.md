# Guide Complet - Configuration Claude Code

Ce guide détaillé couvre tous les aspects de la configuration Claude Code, de l'installation à l'utilisation avancée.

---

## Table des matières

- [Architecture de Configuration](#architecture-de-configuration)
- [Installation Détaillée](#installation-détaillée)
- [Système de Permissions](#système-de-permissions)
- [Commande /epct](#commande-epct)
- [Agent explore-code](#agent-explore-code)
- [Statusline Personnalisée](#statusline-personnalisée)
- [Configuration Avancée](#configuration-avancée)
- [Bonnes Pratiques](#bonnes-pratiques)
- [Workflows Recommandés](#workflows-recommandés)

---

## Architecture de Configuration

### Structure des Fichiers

```
.claude/
├── agents/                      # Agents personnalisés
│   └── explore-code.md         # Agent d'exploration du codebase
├── commands/                    # Commandes slash personnalisées
│   └── epct.md                 # Workflow EPCT
├── scripts/                     # Scripts utilitaires
│   └── statusline-ccusage.sh   # Script de statusline
├── settings.json               # Configuration partagée (Git)
├── settings.local.json         # Configuration personnelle (gitignored)
└── .mcp.json                   # Configuration des serveurs MCP
```

### Principes de Design

#### settings.json - Configuration d'Équipe

Le fichier `settings.json` définit les standards pour toute l'équipe :

**Caractéristiques :**
- ✅ **Versionné dans Git** : Partagé avec tous les membres
- ✅ **Permissions communes** : Règles de sécurité et accès de base
- ✅ **Point de référence** : Configuration minimale pour le projet

**Exemple complet :**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)",
      "Bash(git init)"
    ],
    "deny": [
      "Read(.env*)"
    ],
    "ask": []
  },
  "enableAllProjectMcpServers": true,
  "enabledMcpjsonServers": [
    "mcp__context7",
    "atlassian",
    "playwright"
  ],
  "statusLine": {
    "type": "script",
    "command": "./.claude/scripts/statusline-ccusage.sh"
  }
}
```

#### settings.local.json - Configuration Personnelle

Le fichier `settings.local.json` permet les personnalisations individuelles :

**Caractéristiques :**
- ❌ **Non versionné** : Ajouté au `.gitignore`
- ✅ **Prioritaire** : Écrase les paramètres de `settings.json`
- ✅ **Flexible** : MCP personnels, scripts locaux, permissions additionnelles

**Exemple :**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "mcp__context7",
      "mcp__custom_tool",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)"
    ],
    "deny": [
      "Read(.env*)",
      "Read(**/secrets/**)"
    ]
  }
}
```

### Tableau Comparatif

| Aspect | settings.json | settings.local.json |
|--------|--------------|---------------------|
| **Versioning Git** | ✅ Commité | ❌ Gitignored |
| **Visibilité** | Équipe entière | Développeur individuel |
| **Priorité** | Base | Override (écrase settings.json) |
| **Use Case** | Standards d'équipe | Préférences personnelles |
| **Permissions** | Minimales sécurisées | Additionnelles selon besoins |
| **MCP Servers** | Communs au projet | Personnels (Context7 API key) |
| **Scripts** | Partagés | Locaux |

---

## Installation Détaillée

### Prérequis Système

#### Obligatoires

1. **Claude Code**
   ```bash
   # Vérifier l'installation
   claude --version

   # Installation si nécessaire
   npm install -g @anthropic-ai/claude-code
   ```

2. **Node.js 18+**
   ```bash
   # Vérifier la version
   node --version

   # Installation via nvm (recommandé)
   nvm install 18
   nvm use 18
   ```

3. **Git**
   ```bash
   # Vérifier l'installation
   git --version
   ```

#### Optionnels (pour statusline)

4. **jq** - Parser JSON
   ```bash
   # macOS
   brew install jq

   # Linux (Debian/Ubuntu)
   sudo apt-get install jq

   # Linux (RHEL/CentOS)
   sudo yum install jq
   ```

5. **ccusage** v15.9.4+
   ```bash
   # Installation globale
   npm install -g ccusage

   # Vérifier la version
   ccusage --version
   ```

### Méthode 1 : Via Plugin Marketplace ⚠️ Expérimental

> **⚠️ AVERTISSEMENT - Installation Incomplète**
>
> **Problème connu :** La marketplace Lingelo n'installe actuellement que les commandes. Les agents et scripts ne s'installent pas correctement.
>
> **Ce qui fonctionne :**
> - ✅ **Commandes** : S'installent dans `~/.claude/commands/`
>
> **Ce qui ne fonctionne PAS :**
> - ❌ **Agents** : Ne s'installent PAS dans `~/.claude/agents/`
> - ❌ **Scripts** : Ne s'installent PAS dans `~/.claude/scripts/`
> - ❌ **Settings** : Ne s'installent PAS dans `~/.claude/settings.json`
>
> **Status :** Problème en cours d'investigation avec l'équipe Claude Code.
>
> **Recommandation forte :** Utilisez la Méthode 2 (Installation Manuelle) pour une installation complète et fonctionnelle.

#### Installation via Marketplace (expérimental)

**Si vous souhaitez quand même tester :**

```bash
# 1. Ajouter la Lingelo Marketplace
/plugin marketplace add Lingelo/lingelo-marketplace

# 2. Installer le plugin lingelo-base
/plugin install lingelo-base
```

#### Vérification (attendez-vous à des échecs)

```bash
# Vérifier l'installation
ls ~/.claude/agents/explore-code.md        # ❌ MANQUANT
ls ~/.claude/commands/epct.md              # ✅ PRÉSENT
ls ~/.claude/scripts/statusline-ccusage.sh # ❌ MANQUANT
```

#### Compléter l'installation manquante

**Vous DEVREZ installer manuellement les agents et scripts manquants :**

```bash
# Cloner le repository
git clone https://github.com/Lingelo/claude-code-sample.git
cd claude-code-sample

# Installer les agents manquants
cp -r .claude/agents/* ~/.claude/agents/

# Installer les scripts manquants
cp -r .claude/scripts/* ~/.claude/scripts/
chmod +x ~/.claude/scripts/*.sh
```

**📦 [Lingelo Marketplace](https://github.com/Lingelo/lingelo-marketplace)** - En développement actif

---

### Méthode 2 : Installation Manuelle

#### Installation Globale (Tous les projets)

##### Étape 1 : Cloner le Repository

```bash
# Cloner via HTTPS
git clone https://github.com/Lingelo/claude-code-sample.git

# OU cloner via SSH
git clone git@github.com:Lingelo/claude-code-sample.git

cd claude-code-sample
```

##### Étape 2 : Copier les Configurations

```bash
# Créer les dossiers si nécessaire
mkdir -p ~/.claude/{agents,commands,scripts}

# Copier les agents
cp -r .claude/agents/* ~/.claude/agents/

# Copier les commandes
cp -r .claude/commands/* ~/.claude/commands/

# Copier les scripts
cp -r .claude/scripts/* ~/.claude/scripts/

# Rendre les scripts exécutables
chmod +x ~/.claude/scripts/*.sh
```

##### Étape 3 : Configurer les Settings (Optionnel)

```bash
# Copier settings.json comme base
cp .claude/settings.json ~/.claude/settings.json

# Adapter selon vos besoins
nano ~/.claude/settings.json
```

##### Étape 4 : Vérification

```bash
# Vérifier la structure
tree ~/.claude -L 2

# Résultat attendu :
# ~/.claude
# ├── agents
# │   └── explore-code.md
# ├── commands
# │   └── epct.md
# └── scripts
#     └── statusline-ccusage.sh
```

#### Installation Par Projet

##### Étape 1 : Cloner le Repository

```bash
git clone https://github.com/Lingelo/claude-code-sample.git
cd claude-code-sample
```

##### Étape 2 : Rendre les Scripts Exécutables

```bash
chmod +x .claude/scripts/statusline-ccusage.sh
```

##### Étape 3 : Configuration Personnelle (Optionnel)

```bash
# Créer settings.local.json pour vos préférences
cp .claude/settings.json .claude/settings.local.json

# Ajouter au .gitignore
echo ".claude/settings.local.json" >> .gitignore

# Éditer settings.local.json
nano .claude/settings.local.json
```

##### Étape 4 : Lancer Claude Code

```bash
claude
```

#### Approche Hybride

Cette approche combine le meilleur des deux mondes :

```bash
# 1. Configurations réutilisables en global
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/scripts/* ~/.claude/scripts/
chmod +x ~/.claude/scripts/*.sh

# 2. Settings spécifiques par projet
# Garder ./.claude/settings.json dans chaque projet

# 3. Personnalisations locales
# Créer ./.claude/settings.local.json si besoin
```

**Résultat :**
- Agents et commandes disponibles partout
- Settings adaptés à chaque projet
- Personnalisations locales préservées

---

## Système de Permissions

### Philosophie des Permissions

Le système de permissions de Claude Code fonctionne selon trois niveaux :

1. **allow** : Permissions accordées automatiquement
2. **deny** : Permissions refusées automatiquement
3. **ask** : Claude demande confirmation avant d'exécuter

### Permissions Recommandées

#### Permissions de Base (settings.json)

```json
{
  "permissions": {
    "allow": [
      "WebSearch",                    // Recherche web pour documentation
      "Bash(git commit:*)",          // Commits Git
      "Bash(git add:*)",             // Staging Git
      "Bash(npm:*)",                 // Toutes commandes npm
      "Bash(yarn:*)",                // Toutes commandes yarn
      "Bash(git init)",              // Initialisation Git
      "Bash(git push)"               // Push vers remote
    ],
    "deny": [
      "Read(.env*)"                  // Protection des secrets
    ],
    "ask": []
  }
}
```

#### Permissions Étendues (settings.local.json)

```json
{
  "permissions": {
    "allow": [
      // Permissions de base (héritées)
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)",

      // MCP personnels
      "mcp__context7",
      "mcp__atlassian",
      "mcp__playwright",

      // Commandes supplémentaires
      "Bash(docker:*)",
      "Bash(kubectl:*)",

      // Outils spécifiques
      "Read(~/.claude/scripts/**)"
    ],
    "deny": [
      // Protections de sécurité
      "Read(.env*)",
      "Read(**/secrets/**)",
      "Read(**/*.key)",
      "Read(**/*.pem)",

      // Fichiers sensibles projet
      "Read(config/credentials.json)"
    ]
  }
}
```

### Patterns de Permissions

#### Wildcards

```json
{
  "allow": [
    "Bash(git:*)",           // Toutes les commandes git
    "Bash(npm:*)",           // Toutes les commandes npm
    "Read(src/**/*.js)",     // Tous les JS dans src/
    "Write(tests/**)"        // Écriture dans tests/
  ]
}
```

#### Chemins Spécifiques

```json
{
  "allow": [
    "Read(/Users/angelolima/.claude/scripts/**)",
    "Bash(git mv .claude-plugin marketplace/.claude-plugin)"
  ]
}
```

#### Protection Multi-niveaux

```json
{
  "deny": [
    "Read(.env*)",                    // Tous les .env*
    "Read(**/.env*)",                 // .env dans sous-dossiers
    "Read(**/secrets/**)",            // Dossiers secrets
    "Read(**/*.key)",                 // Fichiers .key
    "Read(**/*.pem)",                 // Certificats
    "Read(**/credentials.json)"       // Credentials
  ]
}
```

### Exemples de Configuration par Contexte

#### Projet Frontend (React/Vue)

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(npm:*)",
      "Bash(yarn:*)",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "mcp__playwright"
    ],
    "deny": [
      "Read(.env*)",
      "Read(public/config.json)"
    ]
  }
}
```

#### Projet Backend (Node.js/Express)

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(npm:*)",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(docker:*)",
      "mcp__atlassian"
    ],
    "deny": [
      "Read(.env*)",
      "Read(config/database.json)",
      "Read(**/*.key)"
    ]
  }
}
```

#### Projet DevOps

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(docker:*)",
      "Bash(kubectl:*)",
      "Bash(terraform:*)"
    ],
    "deny": [
      "Read(.env*)",
      "Read(**/secrets/**)",
      "Read(**/*.pem)"
    ],
    "ask": [
      "Bash(kubectl delete:*)",
      "Bash(terraform destroy:*)"
    ]
  }
}
```

---

## Commande /epct

### Philosophie EPCT

Le workflow EPCT (**E**xplore → **P**lan → **C**ode → **T**est) est une méthodologie de développement structurée qui garantit :

- ✅ Code aligné avec les patterns existants
- ✅ Contexte principal préservé (économie de tokens)
- ✅ Planification avant implémentation
- ✅ Tests ciblés uniquement

### Les 4 Phases Détaillées

#### Phase 1 : EXPLORE

**Objectif** : Découvrir tout le contexte pertinent sans polluer la conversation principale.

**Actions :**
- Lance l'agent `explore-code` en parallèle
- Recherche patterns, conventions, exemples
- Analyse dépendances et imports
- Identifie APIs et schémas de données

**Résultat :** Rapport structuré des découvertes.

**Avantage clé :** L'exploration se fait dans le contexte de l'agent, pas dans votre conversation.

#### Phase 2 : PLAN

**Objectif** : Créer une stratégie détaillée basée sur l'exploration.

**Actions :**
- Analyse des informations découvertes
- Création d'un plan d'implémentation étape par étape
- Identification des fichiers à modifier
- Questions de clarification si nécessaire

**Résultat :** Plan validé par l'utilisateur avant de coder.

**Point de contrôle :** L'utilisateur peut ajuster ou refuser le plan.

#### Phase 3 : CODE

**Objectif** : Implémenter en suivant strictement le plan et les patterns découverts.

**Actions :**
- Implémentation selon le plan validé
- Utilisation des patterns et conventions trouvés
- Respect du scope défini

**Règle d'or :** Ne JAMAIS dépasser le scope du plan.

#### Phase 4 : TEST

**Objectif** : Vérifier que l'implémentation fonctionne.

**Actions :**
- Exécution des tests pertinents uniquement
- Pas de test de l'ensemble de la suite
- Validation des changements

**En cas d'échec :** Retour à la phase PLAN.

### Utilisation Pratique

#### Syntaxe

```bash
/epct [description-de-la-fonctionnalité]
```

#### Exemples Complets

**Exemple 1 : Ajouter une feature**
```bash
/epct ajouter un système de pagination aux listes d'utilisateurs
```

**Déroulement :**
1. **EXPLORE** : Agent trouve les listes existantes, patterns de pagination, composants réutilisables
2. **PLAN** : "Je vais ajouter la pagination en utilisant le composant Paginator existant, modifier UserList.jsx, et ajouter les hooks usePagination"
3. **CODE** : Implémentation avec les mêmes patterns que les autres listes paginées
4. **TEST** : Tests des nouvelles fonctions de pagination uniquement

**Exemple 2 : Refactoring**
```bash
/epct refactoriser le module d'authentification pour utiliser JWT au lieu de sessions
```

**Déroulement :**
1. **EXPLORE** : Agent trouve toutes les utilisations de sessions, middleware auth, routes protégées
2. **PLAN** : Stratégie de migration progressive sans casser l'existant
3. **CODE** : Implémentation JWT en parallèle avec sessions (feature flag)
4. **TEST** : Tests de non-régression + tests JWT

**Exemple 3 : Debugging**
```bash
/epct débugger le problème de memory leak dans le worker de traitement des emails
```

**Déroulement :**
1. **EXPLORE** : Agent analyse le worker, ses dépendances, les patterns de gestion mémoire
2. **PLAN** : Hypothèses de bugs, ajout de profiling, inspection des event listeners
3. **CODE** : Fix + ajout de monitoring mémoire
4. **TEST** : Tests de charge pour vérifier que le leak est résolu

### Règles Critiques

#### ⚠️ À FAIRE

- ✅ Toujours laisser la phase EXPLORE se terminer
- ✅ Valider le PLAN avant de passer au CODE
- ✅ Rester strictement dans le scope défini
- ✅ Exécuter uniquement les tests pertinents
- ✅ Retourner au PLAN si les tests échouent

#### ❌ À NE PAS FAIRE

- ❌ Sauter la phase EXPLORE
- ❌ Coder avant validation du plan
- ❌ Ajouter des features hors scope pendant CODE
- ❌ Exécuter toute la suite de tests
- ❌ Continuer si les tests échouent

---

## Agent explore-code

### Fonctionnement

L'agent `explore-code` est un agent spécialisé qui s'exécute dans son propre contexte isolé.

**Caractéristiques :**
- 🔍 Recherche exhaustive dans le codebase
- 🧠 Analyse intelligente des patterns
- 📊 Rapports structurés
- 💰 Économie de tokens (contexte isolé)

### Capacités

#### 1. Recherche de Patterns

```markdown
**Question :** Comment sont gérées les routes API dans ce projet ?

**Découvertes :**
- Utilisation d'Express.js avec Router
- Routes organisées dans src/routes/
- Pattern : routes/{resource}/index.js
- Middleware de validation avec express-validator
- Exemple : src/routes/users/index.js
```

#### 2. Analyse de Dépendances

```markdown
**Question :** Quelles bibliothèques sont utilisées pour l'authentification ?

**Découvertes :**
- passport.js (stratégie locale + JWT)
- bcrypt pour le hashing de mots de passe
- jsonwebtoken pour les tokens
- Configuration dans src/config/passport.js
```

#### 3. Identification de Conventions

```markdown
**Question :** Quelles sont les conventions de nommage ?

**Découvertes :**
- Fichiers : kebab-case (user-service.js)
- Classes : PascalCase (UserService)
- Fonctions : camelCase (getUserById)
- Constantes : UPPER_SNAKE_CASE (MAX_RETRY_COUNT)
- Tests : *.test.js ou *.spec.js
```

#### 4. Documentation d'APIs

```markdown
**Question :** Quelle est la structure de l'API users ?

**Découvertes :**
GET    /api/users          - Liste des utilisateurs
GET    /api/users/:id      - Détails utilisateur
POST   /api/users          - Créer utilisateur
PUT    /api/users/:id      - Mettre à jour
DELETE /api/users/:id      - Supprimer

Schéma User :
- id: string (UUID)
- email: string (unique)
- name: string
- role: enum ['user', 'admin']
- createdAt: timestamp
```

### Invocation

#### Automatique (via /epct)

L'agent est automatiquement invoqué en phase EXPLORE :

```bash
/epct ajouter une fonctionnalité X
# → Lance automatiquement explore-code
```

#### Manuelle (avancé)

Pour des explorations spécifiques :

```bash
# Lancer Claude Code
claude

# Dans la conversation
> Lance l'agent explore-code pour analyser comment sont gérés les webhooks dans ce projet
```

### Personnalisation

Le fichier `.claude/agents/explore-code.md` peut être personnalisé :

```markdown
---
name: explore-code
description: Spécialiste de l'exploration du codebase
color: blue
model: claude-sonnet-4-5
---

# Agent explore-code

[Instructions personnalisées pour votre projet]

## Focus Spécifique

- Analyser uniquement les fichiers dans src/
- Ignorer les dépendances npm
- Privilégier les exemples récents (< 3 mois)

...
```

---

## Statusline Personnalisée

### Fonctionnement

Le script `statusline-ccusage.sh` s'exécute à chaque rafraîchissement et affiche des métriques en temps réel.

### Configuration

#### Dans settings.json

```json
{
  "statusLine": {
    "type": "script",
    "command": "./.claude/scripts/statusline-ccusage.sh"
  }
}
```

#### Chemins Possibles

**Relatif au projet :**
```json
"command": "./.claude/scripts/statusline-ccusage.sh"
```

**Absolu (installation globale) :**
```json
"command": "/Users/<username>/.claude/scripts/statusline-ccusage.sh"
```

**Tilde (home directory) :**
```json
"command": "~/.claude/scripts/statusline-ccusage.sh"
```

### Métriques Affichées

```
🌿 main* (+15 -3) | 💄 default | 📁 ~/projects/my-app | 🤖 Claude 3.5 Sonnet
💰 $0.26 (5m) | 📅 $8.03 | 🧊 $8.03 (2h 45m left) | 🧩 2.7K tokens
```

**Ligne 1 :**
- 🌿 **Branche Git** : `main*` (le `*` indique des changements non commités)
- (+15 -3) : 15 lignes ajoutées, 3 lignes supprimées
- 💄 **Style de sortie** : `default`
- 📁 **Répertoire** : `~/projects/my-app`
- 🤖 **Modèle** : `Claude 3.5 Sonnet`

**Ligne 2 :**
- 💰 **Coût session** : `$0.26` en 5 minutes
- 📅 **Coût journalier** : `$8.03` total aujourd'hui
- 🧊 **Coût bloc actif** : `$8.03` avec 2h 45m restantes dans le bloc
- 🧩 **Tokens** : 2.7K tokens de conversation (input + output uniquement)

### Note sur les Tokens

Le script affiche uniquement les tokens de **conversation** :
- ✅ Input tokens (vos messages)
- ✅ Output tokens (réponses de Claude)
- ❌ Cache tokens (contexte stocké en interne)

**Pourquoi ?** Les tokens de cache représentent le contexte en mémoire mais ne reflètent pas la taille réelle de votre conversation.

### Personnalisation du Script

Le script peut être modifié pour afficher d'autres informations :

```bash
#!/bin/bash

# Exemple : Ajouter le nombre de fichiers modifiés
MODIFIED_FILES=$(git status --short | wc -l | tr -d ' ')

# Ajouter à la ligne de statut
echo "📝 $MODIFIED_FILES fichiers modifiés"
```

---

## Configuration Avancée

### MCP Servers

Voir [docs/MCP.md](MCP.md) pour la documentation complète des serveurs MCP (Context7, Atlassian, Playwright).

### Hooks Personnalisés

Les hooks permettent d'exécuter des commandes à des moments clés.

**Exemple : Hook pre-commit**

`.claude/settings.json` :
```json
{
  "hooks": {
    "preCommit": "npm run lint && npm test"
  }
}
```

### Agents Multiples

Vous pouvez créer plusieurs agents spécialisés :

```
.claude/agents/
├── explore-code.md      # Exploration du code
├── review-pr.md         # Revue de pull requests
├── write-tests.md       # Génération de tests
└── optimize-perf.md     # Optimisation de performance
```

Chaque agent a son propre contexte isolé.

---

## Bonnes Pratiques

### 1. Gestion des Settings

**✅ À FAIRE**

- Committer `settings.json` avec le projet
- Ajouter `settings.local.json` au `.gitignore`
- Documenter les permissions dans README
- Tester localement avant de commit settings.json

**❌ À NE PAS FAIRE**

- Committer `settings.local.json`
- Mettre des API keys dans `settings.json`
- Permettre l'accès aux fichiers `.env*`
- Donner des permissions trop larges

### 2. Utilisation de /epct

**✅ À FAIRE**

- Toujours commencer par la phase EXPLORE
- Valider le plan avant d'implémenter
- Rester dans le scope défini
- Exécuter uniquement les tests pertinents

**❌ À NE PAS FAIRE**

- Sauter la phase EXPLORE
- Modifier le scope pendant CODE
- Exécuter toute la suite de tests
- Continuer si les tests échouent

### 3. Agents

**✅ À FAIRE**

- Laisser les agents travailler en parallèle
- Faire confiance aux rapports des agents
- Utiliser des agents pour l'exploration intensive

**❌ À NE PAS FAIRE**

- Lancer manuellement des recherches pendant EXPLORE
- Ignorer les patterns découverts
- Utiliser les agents pour des tâches simples

### 4. Sécurité

**✅ À FAIRE**

- Protéger tous les fichiers `.env*`
- Bloquer l'accès aux secrets et credentials
- Utiliser `settings.local.json` pour les API keys
- Réviser régulièrement les permissions

**❌ À NE PAS FAIRE**

- Permettre `Read(.env*)`
- Committer des secrets
- Donner accès aux dossiers `secrets/`
- Autoriser des commandes destructrices sans `ask`

---

## Workflows Recommandés

### Workflow 1 : Nouvelle Feature

```bash
# 1. Exploration
/epct ajouter un système de notifications push

# 2. Validation du plan
# (Lire le plan, demander des clarifications si nécessaire)

# 3. Implémentation
# (Claude code selon le plan)

# 4. Tests
# (Tests pertinents uniquement)

# 5. Commit
git add .
git commit -m "feat: add push notifications system

Implements push notifications using Firebase Cloud Messaging.
- Add NotificationService
- Add push subscription endpoint
- Add notification UI component

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
```

### Workflow 2 : Debugging

```bash
# 1. Exploration du problème
/epct débugger le problème de lenteur sur la page de liste

# 2. Analyse et hypothèses
# (Plan avec hypothèses de bugs)

# 3. Fix
# (Implémentation des corrections)

# 4. Vérification
# (Tests de performance)

# 5. Commit
git add .
git commit -m "fix: improve list page performance

Fix N+1 query issue and add pagination.
- Optimize database queries with eager loading
- Add pagination to reduce data transfer
- Add database indexes

Performance: List load time reduced from 2.3s to 0.4s

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
```

### Workflow 3 : Refactoring

```bash
# 1. Exploration du code existant
/epct refactoriser le module de gestion des fichiers pour utiliser S3 au lieu du filesystem local

# 2. Planification de la migration
# (Stratégie de migration progressive)

# 3. Implémentation progressive
# (Ajout de S3 en parallèle, feature flag)

# 4. Tests de régression
# (Vérifier que tout fonctionne)

# 5. Cleanup
# (Supprimer l'ancien code une fois validé)

# 6. Commit
git add .
git commit -m "refactor: migrate file storage to S3

Replace local filesystem storage with AWS S3.
- Add S3Service for file operations
- Add feature flag for gradual rollout
- Update tests for S3 storage
- Keep backward compatibility with local storage

Migration will be done progressively using the STORAGE_BACKEND env var.

🤖 Generated with Claude Code
Co-Authored-By: Claude <noreply@anthropic.com>"
```

---

## Résolution de Problèmes Avancés

### Problème : Contexte qui explose

**Symptôme :** Le contexte principal devient très lourd avec beaucoup de fichiers lus.

**Solutions :**
1. Utiliser `/epct` pour exploration isolée
2. Créer un agent spécifique pour l'exploration
3. Limiter le scope de la tâche

### Problème : Permissions refusées

**Symptôme :** Claude demande permission pour actions autorisées.

**Solutions :**
```bash
# Vérifier settings.json
cat .claude/settings.json | jq .

# Vérifier settings.local.json
cat .claude/settings.local.json | jq .

# Vérifier la fusion
# settings.local écrase settings.json

# Redémarrer Claude Code
# Ctrl+C puis relancer
```

### Problème : Statusline ne s'affiche pas

**Solutions :**
```bash
# Vérifier ccusage
ccusage --version

# Installer/mettre à jour ccusage
npm install -g ccusage@latest

# Tester le script manuellement
./.claude/scripts/statusline-ccusage.sh

# Vérifier les permissions
chmod +x .claude/scripts/statusline-ccusage.sh

# Vérifier settings.json
cat .claude/settings.json | jq .statusLine
```

---

**[← Retour au README](../README.md)** | **[Documentation MCP →](MCP.md)**
