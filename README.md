# Claude Code - Configuration Professionnelle

Une configuration Claude Code complète et réutilisable, optimisée pour le développement en équipe. Inclut un workflow EPCT structuré, des agents spécialisés, et un suivi des coûts en temps réel.

[![Claude Code](https://img.shields.io/badge/Claude%20Code-Ready-blue)](https://docs.claude.com/en/docs/claude-code)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

---

## 🎯 Pourquoi cette configuration ?

- **🚀 Productivité x3** : Workflow EPCT automatisé (Explore → Plan → Code → Test)
- **💰 Économie de tokens** : Agents isolés qui préservent votre contexte principal
- **📊 Visibilité totale** : Statusline avec coûts, tokens et métriques en temps réel
- **🔒 Sécurité renforcée** : Permissions granulaires, fichiers sensibles protégés
- **👥 Collaboration facilitée** : Configuration partageable et versionnable avec l'équipe

---

## 📋 Table des matières

- [Prérequis](#-prérequis)
- [Quick Start](#-quick-start-5-minutes)
- [Features Principales](#-features-principales)
- [Installation](#-installation)
- [Exemples & Workflows](#-exemples--workflows)
- [Documentation Détaillée](#-documentation-détaillée)
- [Troubleshooting](#-troubleshooting)
- [Ressources](#-ressources)

---

## ⚙️ Prérequis

Avant de commencer, assurez-vous d'avoir :

- **Claude Code** v1.0+ ([Installation](https://docs.claude.com/en/docs/claude-code))
- **Node.js** 18+ (`node --version`)
- **Git** (`git --version`)
- **jq** - Parser JSON (`brew install jq` sur macOS)
- **ccusage** v15.9.4+ - Suivi des coûts (`npm install -g ccusage`)

---

## 🚀 Quick Start (5 minutes)

### 1. Cloner et installer

```bash
# Cloner le repository
git clone https://github.com/votre-username/claude-code-sample.git
cd claude-code-sample

# Installer ccusage pour la statusline
npm install -g ccusage

# Rendre le script statusline exécutable
chmod +x .claude/scripts/statusline-ccusage.sh
```

### 2. Choisir votre mode d'installation

**Option A : Configuration globale (tous vos projets)**
```bash
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/scripts/* ~/.claude/scripts/
```

**Option B : Configuration par projet (ce projet uniquement)**
```bash
# La configuration est déjà dans .claude/
# Rien à faire !
```

### 3. Tester l'installation

```bash
# Lancer Claude Code dans le projet
claude

# Tester la commande /epct
/epct ajouter une fonction de logging simple

# Vérifier la statusline (s'affiche automatiquement)
```

✅ **C'est tout !** Vous êtes prêt à utiliser la configuration.

---

## ✨ Features Principales

### 1. 🔄 Workflow EPCT - Développement Structuré

La commande `/epct` automatise un workflow en 4 phases :

```bash
/epct [description-de-la-fonctionnalité]
```

**Les 4 phases :**
1. **EXPLORE** : Lance des agents en parallèle pour trouver patterns et fichiers pertinents
2. **PLAN** : Crée une stratégie détaillée basée sur l'exploration
3. **CODE** : Implémente en suivant exactement les patterns du projet
4. **TEST** : Vérifie que tout fonctionne (tests ciblés uniquement)

**Avantage clé** : L'agent explore-code travaille dans son propre contexte isolé, économisant jusqu'à 70% de tokens sur votre conversation principale.

### 2. 🤖 Agent explore-code - Spécialiste du Codebase

Un agent autonome qui analyse votre code en profondeur :

- Recherche exhaustive de patterns et conventions
- Analyse des dépendances et imports
- Identification des APIs et schémas de données
- Extraction d'exemples de code pertinents

**Automatiquement invoqué par `/epct`** - fonctionne en arrière-plan sans polluer votre contexte.

### 3. 📊 Statusline Personnalisée - Suivi en Temps Réel

Affichage en temps réel de vos métriques :

```
🌿 main* (+15 -3) | 💄 default | 📁 ~/projects/my-app | 🤖 Claude 3.5 Sonnet
💰 $0.26 (5m) | 📅 $8.03 | 🧊 $8.03 (2h 45m left) | 🧩 2.7K tokens
```

**Informations affichées :**
- 🌿 Branche Git et changements en cours
- 💰 Coût de la session actuelle
- 📅 Coût journalier total
- 🧩 Nombre de tokens (input + output uniquement)
- 🤖 Modèle Claude utilisé

### 4. 🔒 Permissions Sécurisées

Configuration de permissions granulaires :

**✅ Autorisé :**
- WebSearch (documentation)
- git commit/add (version control automatisé)
- npm/yarn (gestion de packages)
- MCP servers (Context7, Atlassian, Playwright)

**❌ Bloqué :**
- Lecture de `.env*` (protection des secrets)

**Configuration partageable** : `settings.json` versionné, `settings.local.json` personnel et gitignored.

---

## 📦 Installation

### Configuration Globale vs Par Projet

| Aspect | Globale (`~/.claude/`) | Par Projet (`./.claude/`) |
|--------|------------------------|---------------------------|
| Disponibilité | Tous vos projets | Ce projet uniquement |
| Versioning Git | ❌ Non | ✅ Oui |
| Partage équipe | ❌ Personnel | ✅ Automatique via Git |
| Maintenance | Une seule config | Config par projet |

### Installation Globale

```bash
# Copier agents, commandes et scripts
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/
cp -r .claude/scripts/* ~/.claude/scripts/

# Adapter settings.json à vos besoins
cp .claude/settings.json ~/.claude/settings.json
```

### Installation Par Projet

La configuration est déjà présente dans `.claude/`. Il suffit de :

1. Cloner le repo
2. Rendre le script statusline exécutable : `chmod +x .claude/scripts/statusline-ccusage.sh`
3. Lancer Claude Code : `claude`

### Approche Hybride (Recommandée)

Combinez les deux pour un maximum de flexibilité :

```bash
# Global : Agents et commandes réutilisables
cp -r .claude/agents/* ~/.claude/agents/
cp -r .claude/commands/* ~/.claude/commands/

# Par projet : Settings spécifiques dans ./.claude/settings.json
```

Claude Code fusionnera automatiquement les deux configurations.

---

## 💡 Exemples & Workflows

### Exemple 1 : Ajouter une nouvelle API

```bash
/epct ajouter un endpoint API pour récupérer les utilisateurs actifs
```

**Ce qui se passe :**
1. **EXPLORE** : Agent trouve les endpoints existants, patterns de routing, modèles de données
2. **PLAN** : Claude propose un plan aligné avec votre architecture
3. **CODE** : Implémentation avec les mêmes conventions que le reste du code
4. **TEST** : Vérification avec les tests pertinents uniquement

**Résultat** : Code cohérent, qui s'intègre naturellement dans votre codebase.

### Exemple 2 : Débugger un problème

```bash
/epct débugger pourquoi l'authentification échoue en production
```

**Ce qui se passe :**
1. **EXPLORE** : Agent analyse le flux d'authentification, middlewares, logs
2. **PLAN** : Hypothèses de bugs basées sur le code existant
3. **CODE** : Fix avec ajout de logging approprié
4. **TEST** : Validation que le bug est corrigé

### Exemple 3 : Refactoring complexe

```bash
/epct refactoriser le système de cache pour utiliser Redis au lieu de memcached
```

**Ce qui se passe :**
1. **EXPLORE** : Agent trouve toutes les utilisations de memcached, config, dépendances
2. **PLAN** : Stratégie de migration progressive
3. **CODE** : Remplacement avec patterns Redis existants (si déjà utilisé ailleurs)
4. **TEST** : Tests de régression sur les fonctionnalités de cache

---

## 📚 Documentation Détaillée

Pour aller plus loin :

- **[Guide Complet](docs/GUIDE.md)** : Configuration avancée, permissions, bonnes pratiques
- **[MCP Servers](docs/MCP.md)** : Installation et utilisation de Context7, Atlassian, Playwright
- **[Voice Inc.](docs/VOICE.md)** : Interaction vocale avec Claude Code
- **[CLAUDE.md](CLAUDE.md)** : Référence technique pour Claude Code

---

## 🛠️ Troubleshooting

### La statusline ne s'affiche pas

**Problème** : La statusline reste vide ou affiche des erreurs.

**Solutions** :
```bash
# Vérifier que ccusage est installé
ccusage --version  # Doit être >= v15.9.4

# Vérifier que jq est installé
jq --version

# Vérifier les permissions du script
chmod +x .claude/scripts/statusline-ccusage.sh

# Tester le script manuellement
./.claude/scripts/statusline-ccusage.sh
```

### L'agent explore-code ne se lance pas

**Problème** : `/epct` ne lance pas l'agent en phase EXPLORE.

**Solutions** :
```bash
# Vérifier que l'agent existe
ls .claude/agents/explore-code.md

# Pour installation globale
ls ~/.claude/agents/explore-code.md

# Vérifier le contenu du fichier
cat .claude/agents/explore-code.md
```

### Erreurs de permissions

**Problème** : Claude demande des permissions pour des actions autorisées.

**Solutions** :
```bash
# Vérifier settings.json
cat .claude/settings.json

# Vérifier qu'il n'y a pas d'erreurs JSON
jq . .claude/settings.json

# Si settings.local.json existe, vérifier la cohérence
cat .claude/settings.local.json | jq .
```

### La commande /epct n'existe pas

**Problème** : `/epct` n'est pas reconnue.

**Solutions** :
```bash
# Vérifier que le fichier de commande existe
ls .claude/commands/epct.md

# Pour installation globale
ls ~/.claude/commands/epct.md

# Redémarrer Claude Code
# Ctrl+C puis relancer : claude
```

### Tokens qui explosent pendant l'exploration

**Problème** : La phase EXPLORE consomme trop de tokens.

**Solution** : C'est normal ! L'agent explore-code travaille dans un contexte isolé. Les tokens d'exploration ne comptent PAS dans votre conversation principale. Votre contexte reste léger.

---

## 🔧 Configuration Avancée

### Personnaliser les permissions

Éditez `.claude/settings.json` ou créez `.claude/settings.local.json` :

```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)",
      "mcp__context7"
    ],
    "deny": [
      "Read(.env*)"
    ]
  }
}
```

### Ajouter des serveurs MCP

Voir la [documentation MCP détaillée](docs/MCP.md) pour installer :
- **Context7** : Documentation technique en temps réel
- **Atlassian** : Intégration Jira, Confluence, Bitbucket
- **Playwright** : Automatisation navigateur

### Comparaison settings.json vs settings.local.json

| Aspect | settings.json | settings.local.json |
|--------|--------------|---------------------|
| **Git** | ✅ Versionné | ❌ Gitignored |
| **Scope** | Équipe | Personnel |
| **Priorité** | Base | Override (écrase settings.json) |
| **Usage** | Standards d'équipe | Préférences personnelles |
| **Exemple** | Permissions de base | MCP personnels, API keys |

---

## 💡 Tips et Astuces

### Raccourcis Clavier

- **Échap (1x)** : Vide l'input texte en cours
- **Échap Échap (2x)** : Rewind d'une étape dans la conversation

### Commandes de Contextualisation

- **# (dièse)** : Ajouter une mémoire dans `CLAUDE.md`
  - Sauvegarde d'informations importantes sur le projet
  - Claude Code lira automatiquement ce fichier

- **@ (arobase)** : Cibler un fichier particulier
  - Usage : `@nom-du-fichier.ext`
  - Charge le contenu du fichier dans le contexte

### Bonnes Pratiques

**1. Structure des demandes**
- ✅ Formuler des demandes claires et précises
- ✅ Décomposer les tâches complexes avec `/epct`
- ✅ Vérifier les résultats après chaque modification

**2. Gestion du code**
- ✅ Préférer l'édition de fichiers existants à la création
- ✅ Laisser `/epct` découvrir les patterns avant de coder
- ✅ Demander validation du plan avant l'implémentation

**3. Utilisation des workflows**
- ✅ Toujours commencer par la phase EXPLORE avec `/epct`
- ✅ Ne pas modifier le scope pendant la phase CODE
- ✅ Exécuter uniquement les tests pertinents en phase TEST

---

## 🌟 Fonctionnalités à venir

- [ ] Support de nouveaux MCP servers
- [ ] Templates de projets préconfigurés
- [ ] Intégration CI/CD avec hooks
- [ ] Dashboard web pour métriques de session

---

## 🤝 Contribution

Les contributions sont les bienvenues ! N'hésitez pas à :

1. Fork le projet
2. Créer une branche pour votre feature (`git checkout -b feature/AmazingFeature`)
3. Commit vos changements (`git commit -m 'Add AmazingFeature'`)
4. Push vers la branche (`git push origin feature/AmazingFeature`)
5. Ouvrir une Pull Request

---

## 📄 License

Ce projet est sous licence MIT. Voir le fichier `LICENSE` pour plus de détails.

---

## 📞 Support & Ressources

### Documentation

- [Documentation Claude Code officielle](https://docs.claude.com/en/docs/claude-code)
- [Guide détaillé de cette configuration](docs/GUIDE.md)
- [Documentation MCP](docs/MCP.md)
- [ccusage GitHub](https://github.com/ryoppippi/ccusage)

### Communauté

- [GitHub Issues](https://github.com/votre-username/claude-code-sample/issues) : Signaler un bug ou demander une feature
- [Discussions](https://github.com/votre-username/claude-code-sample/discussions) : Questions et discussions

---

## 🙏 Remerciements

- **Anthropic** pour Claude Code
- **Communauté Claude Code** pour les retours et contributions
- **ryoppippi** pour l'outil ccusage

---

**Maintenu avec ❤️ par [Angelo LIMA](https://github.com/votre-username)**

*Ce projet est continuellement mis à jour avec les meilleures pratiques de développement.*
