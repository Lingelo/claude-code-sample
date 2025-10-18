# Claude Code - Configuration et Bonnes Pratiques

Ce repository présente ma configuration personnelle de Claude Code, incluant les bonnes pratiques, scripts, et configurations que j'utilise pour améliorer mon expérience de développement.

## Table des matières

- [Utilisation de Voice Inc.](#utilisation-de-voice-inc)
- [Configuration Claude Code](#configuration-claude-code)
- [Statusline Personnalisée](#statusline-personnalisée)
- [Commandes Slash Personnalisées](#commandes-slash-personnalisées)
- [Agents Personnalisés](#agents-personnalisés)
- [Plugin Marketplace](#plugin-marketplace)
- [MCP - Model Context Protocol](#mcp---model-context-protocol)
- [Tips et Astuces](#tips-et-astuces)
- [Bonnes Pratiques](#bonnes-pratiques)

---

## Utilisation de Voice Inc.

J'utilise l'application **Voice Inc.** pour interagir avec Claude Code via la voix. Cela me permet de :

- Dicter mes commandes au lieu de les taper au clavier
- Gagner en productivité et en confort lors de longues sessions de développement
- Travailler de manière plus ergonomique en réduisant la fatigue liée à la saisie au clavier

### Avantages de l'interaction vocale

- **Rapidité** : La dictée vocale peut être plus rapide que la saisie au clavier pour des instructions complexes
- **Accessibilité** : Permet de coder même en situation de mobilité réduite ou de fatigue des mains
- **Multitâche** : Possibilité d'interagir avec Claude Code tout en gardant les mains libres pour d'autres tâches

### Workflow vocal

- Parler clairement et à un rythme régulier
- Épeler les noms de variables ou de fichiers complexes si nécessaire
- Confirmer visuellement que la transcription est correcte avant d'envoyer la commande

---

## Configuration Claude Code

Claude Code utilise deux types de fichiers de configuration pour gérer les permissions et les accès de manière flexible.

### Utilisation Globale vs Par Projet

Ce repository contient une configuration Claude Code complète qui peut être utilisée de deux façons :

#### 🌍 Configuration Globale (Tous vos projets)

Copiez le contenu du dossier `.claude/` de ce repo vers votre répertoire home `~/.claude/` :

```bash
# Copier les agents
cp -r .claude/agents/* ~/.claude/agents/

# Copier les commandes
cp -r .claude/commands/* ~/.claude/commands/

# Copier les scripts
cp -r .claude/scripts/* ~/.claude/scripts/

# Copier la configuration (optionnel - adapter selon vos besoins)
cp .claude/settings.json ~/.claude/settings.json
```

**Avantages :**
- ✅ Agent `explore-code` disponible dans tous vos projets
- ✅ Commande `/epct` accessible partout
- ✅ Statusline personnalisée sur tous vos projets
- ✅ Une seule configuration à maintenir

#### 📁 Configuration Par Projet (Ce projet uniquement)

Gardez la configuration dans le dossier `.claude/` du projet :

**Avantages :**
- ✅ Configuration versionnée avec le projet
- ✅ Partagée avec l'équipe via Git
- ✅ Adaptée aux besoins spécifiques du projet
- ✅ Pas d'impact sur vos autres projets

#### 🔄 Approche Hybride (Recommandée)

Combinez les deux approches pour un maximum de flexibilité :

1. **Global** : Agents et commandes génériques dans `~/.claude/`
2. **Par projet** : Configuration spécifique (settings.json) dans `.claude/`

Claude Code fusionnera automatiquement :
- Les agents de `~/.claude/agents/` avec ceux de `./.claude/agents/`
- Les commandes de `~/.claude/commands/` avec celles de `./.claude/commands/`
- Les settings locaux écrasent les settings globaux

### Architecture de Configuration

#### settings.json - Configuration Partagée

Le fichier **`.claude/settings.json`** est la configuration de base pour toute l'équipe :

- **Versionnée dans Git** : Partagée avec tous les membres de l'équipe
- **Permissions communes** : Définit les règles de sécurité et accès standards
- **Point de référence** : Configuration minimale pour travailler sur le projet

**Contenu type :**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)"
    ],
    "deny": [
      "Read(.env*)"
    ]
  }
}
```

#### settings.local.json - Configuration Personnelle

Le fichier **`.claude/settings.local.json`** permet de surcharger ou étendre la configuration partagée :

- **Non versionnée** : Ajoutée au `.gitignore`, propre à chaque développeur
- **Personnalisations** : MCP personnels, scripts locaux, permissions additionnelles
- **Prioritaire** : Les paramètres locaux écrasent ceux du fichier partagé

**Contenu type :**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "mcp__context7",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "Bash(yarn:*)"
    ],
    "deny": [
      "Read(.env*)"
    ]
  }
}
```

### Permissions Configurées

#### Commandes autorisées (Allow)

**Configuration d'équipe :**
- **WebSearch** : Recherche web pour accéder à la documentation
- **git commit/add** : Gestion de version automatisée
- **npm** : Toutes les commandes npm via wildcard (`npm:*`)
- **yarn** : Toutes les commandes yarn via wildcard (`yarn:*`)

**Ajouts personnels (exemple) :**
- **mcp__context7** : Serveur MCP pour documentation enrichie

#### Fichiers protégés (Deny)

Pour des raisons de sécurité, l'accès en lecture aux fichiers d'environnement est explicitement bloqué :

- `.env`
- `.env.local`
- `.env.test`
- `.env.development`
- `.env.production`
- Et autres variantes de fichiers d'environnement

Cette configuration permet de travailler efficacement avec les gestionnaires de packages tout en protégeant les informations sensibles comme les clés API, tokens, et autres secrets.

### Bonnes Pratiques

1. **Committer settings.json** : Configuration de base pour l'équipe
2. **Ignorer settings.local.json** : Ajoutez-le au `.gitignore`
3. **Documenter les permissions** : Expliquez pourquoi certaines permissions sont activées/bloquées
4. **Tester localement** : Validez vos permissions personnelles avant de les proposer à l'équipe

---

## Statusline Personnalisée

Une statusline personnalisée qui affiche des informations utiles sur votre session Claude Code.

### Fonctionnalités

- 🌿 **Statut Git** avec la branche actuelle et indicateurs de changements (+/-)
- 💄 **Style de sortie** configuré (si applicable)
- 📁 **Répertoire courant** avec chemin complet
- 🤖 **Modèle Claude** utilisé
- 💰 **Coût de session** avec durée
- 📅 **Coût journalier** total
- 🧊 **Coût du bloc actif** avec temps restant
- 🧩 **Nombre de tokens** pour la session actuelle (input + output uniquement)

### Prérequis

- [ccusage](https://github.com/ryoppippi/ccusage) v15.9.4 ou supérieur (pour le support `--id`)
- `jq` pour le parsing JSON
- `git` pour les informations de repository

### Installation

```bash
# Installer ccusage
npm install -g ccusage

# Le script est disponible dans .claude/scripts/statusline-ccusage.sh
# Il doit être rendu exécutable :
chmod +x .claude/scripts/statusline-ccusage.sh
```

### Configuration

Le script statusline est déjà configuré dans `settings.json`. Le chemin peut être :
- **Relatif au projet** : `./.claude/scripts/statusline-ccusage.sh`
- **Absolu depuis le home** : `~/.claude/scripts/statusline-ccusage.sh` (si copié globalement)

### Exemple de Sortie

```
🌿 main* (+15 -3) | 💄 default | 📁 ~/projects/my-app | 🤖 Claude 3.5 Sonnet
💰 $0.26 (5m) | 📅 $8.03 | 🧊 $8.03 (2h 45m left) | 🧩 2.7K tokens
```

### Note sur les Tokens

Le script affiche uniquement les tokens de conversation (input + output) et exclut les tokens de cache, car ceux-ci représentent le contexte stocké en interne et ne reflètent pas la taille réelle de la conversation

---

## Commandes Slash Personnalisées

### /epct - Workflow EPCT (Explore, Plan, Code, Test)

Une commande slash pour suivre une méthodologie de développement structurée.

**Usage :** `/epct [description-de-la-fonctionnalité]`

Le workflow EPCT se décompose en 4 phases :

1. **EXPLORE** : Trouver tous les fichiers pertinents pour l'implémentation
2. **PLAN** : Créer une stratégie d'implémentation détaillée
3. **CODE** : Implémenter en suivant les patterns existants
4. **TEST** : Vérifier que les changements fonctionnent correctement

Voir le fichier `.claude/commands/epct.md` pour plus de détails.

---

## Agents Personnalisés

Les agents sont des assistants spécialisés qui fonctionnent de manière autonome pour accomplir des tâches spécifiques. Ils s'exécutent en arrière-plan, dans leur propre contexte isolé.

### explore-code - Spécialiste de l'Exploration du Codebase

Un agent dédié à la découverte et l'analyse du code existant.

**Usage :** Automatiquement invoqué par `/epct` dans la phase EXPLORE

**Capacités :**
- Recherche exhaustive de fichiers et patterns dans le codebase
- Analyse des dépendances et imports
- Identification des conventions et patterns existants
- Documentation des APIs et schémas de base de données
- Extraction d'exemples de code pertinents

### Avantages des Agents

#### 1. Préservation du Contexte Principal

Les agents fonctionnent dans leur propre contexte isolé, ce qui signifie :
- **Pas de pollution du contexte** : Les recherches et explorations intensives ne remplissent pas votre conversation principale
- **Économie de tokens** : Le contexte principal reste léger et focalisé sur la tâche
- **Meilleure organisation** : Séparation claire entre exploration et implémentation

#### 2. Exécution en Parallèle

- **Gain de temps** : Plusieurs agents peuvent fonctionner simultanément
- **Exploration multi-dimensionnelle** : Recherche simultanée dans différentes parties du codebase
- **Efficacité** : Pendant qu'un agent explore, d'autres peuvent rechercher de la documentation

#### 3. Spécialisation et Expertise

- **Focus** : Chaque agent est optimisé pour une tâche spécifique
- **Profondeur** : Analyse plus approfondie dans leur domaine d'expertise
- **Résultats structurés** : Rapports formatés et organisés selon des templates définis

### Combiner /epct et explore-code pour un Développement Optimal

La puissance de cette configuration réside dans la combinaison synergique de `/epct` et de l'agent `explore-code`.

#### Workflow Recommandé

**1. Phase EXPLORE**
```
/epct ajouter un système d'authentification OAuth
```

La commande `/epct` lance automatiquement l'agent `explore-code` qui :
- Cherche les patterns d'authentification existants
- Identifie les fichiers de configuration pertinents
- Trouve les middlewares et guards utilisés
- Localise les exemples de flux d'authentification
- **Tout cela sans polluer votre contexte principal**

**2. Phase PLAN**

Avec les informations recueillies par l'agent, Claude crée un plan détaillé :
- Basé sur les patterns existants découverts
- Aligné avec les conventions du projet
- Incluant les fichiers spécifiques à modifier
- **Le contexte reste clair et focalisé sur la planification**

**3. Phase CODE**

L'implémentation suit les patterns découverts :
- Utilise les mêmes conventions de nommage
- Réutilise les utilitaires existants
- S'intègre naturellement dans l'architecture
- **Contexte concentré uniquement sur le code à écrire**

**4. Phase TEST**

Validation basée sur les tests existants trouvés :
- Suit les patterns de test du projet
- Réutilise les fixtures et mocks existants
- **Contexte minimal pour les vérifications**

#### Avantages de cette Combinaison

**Contexte Principal Optimisé**
- Seules les informations essentielles sont dans la conversation principale
- Les détails de l'exploration restent dans le contexte de l'agent
- Plus de place pour le code et les décisions importantes

**Développement Plus Pertinent**
- Code aligné avec l'existant dès le départ
- Moins d'allers-retours pour ajuster le style
- Réutilisation maximale des patterns éprouvés

**Efficacité Maximale**
- Exploration parallèle de multiples aspects
- Pas de temps perdu à chercher manuellement
- Focus sur l'implémentation plutôt que la découverte

#### Exemple Concret

Sans agents :
```
User: Ajoute un système de cache
Claude: [Lit 20 fichiers, remplit le contexte, cherche des patterns...]
Claude: [Propose une solution potentiellement différente du style du projet]
User: Non, on utilise Redis ici, pas memcached
Claude: [Recommence avec Redis...]
```

Avec /epct + explore-code :
```
User: /epct ajoute un système de cache
Claude: [Lance explore-code qui trouve que le projet utilise Redis]
Claude: [Présente un plan basé sur les patterns Redis existants]
User: Parfait, go!
Claude: [Implémente en suivant exactement le style du projet]
```

---

## Plugin Marketplace

### Qu'est-ce qu'un Plugin Marketplace ?

Un **Plugin Marketplace** est un dépôt centralisé qui permet de distribuer et d'installer facilement des collections de configurations Claude Code. Introduit en octobre 2025, ce système permet de partager des commandes slash, agents, serveurs MCP, et hooks avec un seul fichier de configuration.

### Structure du Marketplace

Ce repository contient un marketplace local dans le dossier `marketplace/.claude-plugin/` :

```
marketplace/
├── .claude-plugin/
│   └── marketplace.json    # Fichier de configuration du marketplace
└── claude-code-config/     # Plugin lingelo-base
    ├── agents/
    ├── commands/
    ├── scripts/
    └── settings.json
```

Le fichier `marketplace.json` définit :
- **Informations du marketplace** : Nom, propriétaire, contact
- **Liste des plugins** : Plugins disponibles avec leurs métadonnées
- **Sources des plugins** : Chemins locaux ou URLs distants

### Configuration du Marketplace

Le fichier `marketplace/.claude-plugin/marketplace.json` de ce repository :

```json
{
  "name": "lingelo-tools",
  "owner": {
    "name": "Angelo LIMA",
    "email": "angelomiguellima@gmail.com"
  },
  "plugins": [
    {
      "name": "lingelo-base",
      "source": "./claude-code-config",
      "description": "Lingelo base configurations for Claude Code.",
      "version": "1.0.0",
      "author": {
        "name": "Angelo LIMA",
        "email": "angelomiguellima@gmail.com"
      }
    }
  ]
}
```

### Qu'est-ce qu'un Plugin ?

Un plugin est une collection réutilisable qui peut inclure :

- **📋 Slash commands** : Raccourcis personnalisés pour des opérations fréquentes
- **🤖 Agents (subagents)** : Assistants spécialisés pour des tâches de développement spécifiques
- **🔌 Serveurs MCP** : Connexions à des outils et sources de données externes
- **🪝 Hooks** : Personnalisation du comportement de Claude Code à des points clés

### Utilisation du Marketplace

#### Installer un Plugin

Pour installer le plugin `lingelo-base` depuis ce marketplace local :

```bash
# Si le marketplace est dans le répertoire courant
/plugin

# Puis sélectionner "lingelo-base" dans le menu
```

Pour un marketplace distant (GitHub) :

```bash
# Ajouter un marketplace distant
/plugin marketplace add user-or-org/repo-name

# Exemple avec un marketplace GitHub public
/plugin marketplace add ananddtyagi/claude-code-marketplace

# Puis installer un plugin via le menu
/plugin
```

#### Gérer les Marketplaces

```bash
# Lister les marketplaces configurés
/plugin marketplace list

# Supprimer un marketplace
/plugin marketplace remove lingelo-tools
```

### Créer votre Propre Marketplace

Pour créer un marketplace partageable :

1. **Créez la structure de base** :
```bash
mkdir -p marketplace/.claude-plugin
mkdir -p marketplace/mon-plugin
```

2. **Créez le fichier marketplace.json** :
```json
{
  "name": "mon-marketplace",
  "owner": {
    "name": "Votre Nom",
    "email": "votre@email.com"
  },
  "plugins": [
    {
      "name": "mon-plugin",
      "source": "./path/to/plugin",
      "description": "Description de votre plugin",
      "version": "1.0.0",
      "author": {
        "name": "Votre Nom",
        "email": "votre@email.com"
      }
    }
  ]
}
```

3. **Organisez vos plugins** :
```
marketplace/
├── .claude-plugin/
│   └── marketplace.json
└── mon-plugin/              # Dossier du plugin
    ├── agents/              # Agents personnalisés
    │   └── explore-code.md
    ├── commands/            # Commandes slash
    │   └── epct.md
    ├── scripts/             # Scripts utilitaires
    │   └── statusline.sh
    └── settings.json        # Configuration partagée
```

4. **Publiez sur GitHub** (optionnel) :
```bash
git init
git add .
git commit -m "Initial marketplace setup"
git push origin main
```

5. **Partagez avec d'autres** :
```bash
# Autres développeurs peuvent l'ajouter avec :
/plugin marketplace add votre-username/votre-repo
```

### Avantages des Marketplaces

**🚀 Installation Simplifiée**
- Un seul fichier `marketplace.json` pour distribuer plusieurs plugins
- Installation en une commande via `/plugin`
- Pas besoin de copier manuellement les fichiers

**📦 Packaging Centralisé**
- Regroupez agents, commandes, et scripts dans un seul package
- Versionnement clair de vos configurations
- Distribution facile au sein d'une équipe

**🔄 Mise à Jour Facilitée**
- Mettez à jour le marketplace, pas chaque configuration individuellement
- Les utilisateurs peuvent réinstaller pour obtenir les dernières versions
- Gestion centralisée des dépendances

**🌐 Partage Communautaire**
- Publiez vos workflows sur GitHub
- Découvrez les configurations d'autres développeurs
- Contribuez à l'écosystème Claude Code

### Marketplaces Publics Recommandés

Quelques marketplaces communautaires populaires :

```bash
# Marketplace officiel communautaire
/plugin marketplace add ananddtyagi/claude-code-marketplace

# Every-Env plugin marketplace
/plugin marketplace add EveryInc/every-marketplace

# Plugins Plus (227+ plugins)
/plugin marketplace add jeremylongshore/claude-code-plugins-plus
```

### Plugin lingelo-base

Le plugin `lingelo-base` inclus dans ce marketplace contient :

- **Agent explore-code** : Exploration approfondie du codebase
- **Commande /epct** : Workflow Explore-Plan-Code-Test
- **Statusline personnalisée** : Affichage des métriques de session
- **Configuration partagée** : Permissions et settings d'équipe

Pour installer :
```bash
/plugin
# Sélectionner "lingelo-base" dans le menu
```

---

## MCP - Model Context Protocol

### Qu'est-ce que MCP ?

Le **Model Context Protocol (MCP)** est un système qui permet à Claude Code d'accéder à des sources de données et services externes. Les serveurs MCP étendent les capacités de Claude en lui donnant accès à des APIs, bases de données, outils de recherche, et autres ressources.

### MCP Recommandés

#### Context7

**Context7** est un service MCP qui permet à Claude Code d'accéder à de la documentation technique en temps réel. Il fournit un contexte enrichi et à jour sur les frameworks, bibliothèques, et outils de développement.

**Avantages de Context7 :**
- Accès à la documentation officielle des frameworks populaires
- Informations à jour sur les APIs et meilleures pratiques
- Réponses contextuelles basées sur les versions spécifiques des bibliothèques
- Améliore la précision des suggestions de Claude pour les technologies récentes

**Installation :**

1. Obtenez une clé API sur [context7.com](https://context7.com)

2. Installez le serveur MCP Context7 :
```bash
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: YOUR_API_KEY"
```

Remplacez `YOUR_API_KEY` par votre clé API obtenue sur le site de Context7.

**Utilisation :**

Une fois installé, Claude Code pourra automatiquement consulter Context7 pour obtenir de la documentation et des exemples de code à jour lorsque vous travaillez sur des projets utilisant des frameworks supportés.

#### Atlassian

**Atlassian MCP** est un serveur MCP qui permet à Claude Code d'interagir directement avec les outils de l'écosystème Atlassian (Jira, Confluence, Bitbucket, etc.). Il facilite l'intégration du développement avec la gestion de projet et la documentation.

**Avantages d'Atlassian MCP :**
- Accès direct à vos tickets Jira depuis Claude Code
- Lecture et recherche dans la documentation Confluence
- Intégration avec Bitbucket pour la gestion de code
- Création et mise à jour de tickets/issues directement depuis vos sessions de code
- Synchronisation du contexte entre votre code et vos projets Atlassian
- Améliore la traçabilité entre les tâches et le code

**Installation :**

```bash
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse
```

**Note :** Ce MCP utilise le transport SSE (Server-Sent Events) pour une communication en temps réel avec les services Atlassian.

**Utilisation :**

Une fois installé, vous pourrez demander à Claude Code de :
- Récupérer les informations d'un ticket Jira
- Rechercher dans votre documentation Confluence
- Créer des tickets basés sur des bugs découverts
- Mettre à jour le statut de vos tâches
- Lier automatiquement votre code aux issues correspondantes

### Gestion des MCP

```bash
# Lister les MCP installés
claude mcp list

# Supprimer un MCP
claude mcp remove context7

# Mettre à jour la configuration d'un MCP
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: NEW_API_KEY"
```

---

## Tips et Astuces

### Raccourcis Clavier

- **Échap (1x)** : Vide le contenu de l'input texte
  - Utile pour annuler rapidement ce que vous êtes en train de taper
  - Permet de recommencer une saisie sans avoir à tout effacer manuellement

- **Échap Échap (2x)** : Rewind d'une étape dans la conversation
  - Permet de revenir à l'état précédent de la conversation
  - Idéal pour annuler la dernière interaction et repartir d'un point antérieur
  - Pratique si vous voulez explorer une direction différente dans la conversation

### Commandes de Contextualisation

- **# (dièse)** : Ajouter une mémoire dans CLAUDE.md
  - Permet de sauvegarder des informations importantes sur le projet
  - Les notes sont stockées dans le fichier `CLAUDE.md` à la racine du projet
  - Idéal pour documenter les décisions de design, conventions, ou informations récurrentes
  - Claude Code lira automatiquement ce fichier pour avoir le contexte du projet

- **@ (arobase)** : Cibler un fichier particulier
  - Permet de contextualiser un fichier spécifique dans la conversation
  - Usage : `@nom-du-fichier.ext` pour référencer un fichier
  - Claude Code chargera le contenu du fichier dans le contexte
  - Utile pour discuter d'un fichier précis ou demander des modifications ciblées

---

## Bonnes Pratiques

### 1. Structure des demandes

- Formuler des demandes claires et précises
- Décomposer les tâches complexes en étapes plus simples
- Vérifier les résultats après chaque modification importante

### 2. Gestion du code

- Toujours préférer l'édition de fichiers existants à la création de nouveaux fichiers
- Utiliser les outils spécialisés (Read, Edit, Write) plutôt que les commandes bash
- Demander une revue du code pour les changements importants

### 3. Communication

- Être explicite sur les attentes et les objectifs
- Demander des explications quand quelque chose n'est pas clair
- Utiliser les références de code (chemin:ligne) pour une navigation facile

### 4. Utilisation des workflows

- Utiliser `/epct` pour des tâches de développement structurées
- Toujours commencer par la phase EXPLORE avant de coder
- Demander validation du plan avant d'implémenter

---

## Outils et Technologies

- **Claude Code** : Assistant CLI pour le développement logiciel
- **Voice Inc.** : Application de dictée vocale pour l'interaction sans clavier
- **ccusage** : Outil de suivi des coûts et usage de Claude
- **Git** : Gestion de version
- **jq** : Parser JSON en ligne de commande

## Ressources

- [Documentation Claude Code](https://docs.claude.com/en/docs/claude-code)
- [ccusage GitHub](https://github.com/ryoppippi/ccusage)
- [Voice Inc.](https://voiceinc.app/)

---

*Ce README est maintenu à jour avec mes pratiques de développement évolutives.*
