# Documentation MCP - Model Context Protocol

Guide complet pour installer et utiliser les serveurs MCP avec Claude Code.

---

## Table des matières

- [Qu'est-ce que MCP ?](#quest-ce-que-mcp-)
- [Serveurs MCP Recommandés](#serveurs-mcp-recommandés)
  - [Context7](#context7)
  - [Atlassian](#atlassian)
  - [Playwright](#playwright)
- [Installation](#installation)
- [Configuration](#configuration)
- [Gestion des Serveurs MCP](#gestion-des-serveurs-mcp)
- [Troubleshooting](#troubleshooting)
- [Cas d'Usage](#cas-dusage)

---

## Qu'est-ce que MCP ?

Le **Model Context Protocol (MCP)** est un système qui permet à Claude Code d'accéder à des sources de données et services externes. Les serveurs MCP étendent les capacités de Claude en lui donnant accès à :

- 📚 **APIs externes** : Documentation, wikis, bases de connaissances
- 🗄️ **Bases de données** : SQL, NoSQL, GraphQL
- 🔍 **Outils de recherche** : Elasticsearch, Algolia
- 🌐 **Services web** : Jira, Confluence, GitHub
- 🤖 **Automatisation** : Playwright, Puppeteer
- 📊 **Métriques** : Datadog, Prometheus

**Avantages :**
- ✅ Contexte enrichi et à jour
- ✅ Accès à la documentation en temps réel
- ✅ Automatisation de tâches répétitives
- ✅ Intégration avec votre stack existant

---

## Serveurs MCP Recommandés

### Context7

**Context7** est un service MCP qui fournit un accès à de la documentation technique en temps réel.

#### Qu'est-ce que Context7 ?

Context7 maintient une base de connaissances à jour de la documentation de frameworks, bibliothèques et outils populaires. Lorsque vous travaillez sur un projet, Claude peut automatiquement consulter Context7 pour obtenir :

- Documentation officielle des frameworks (React, Vue, Angular, etc.)
- APIs de bibliothèques populaires (lodash, axios, etc.)
- Meilleures pratiques actuelles
- Exemples de code à jour
- Informations sur les versions spécifiques

#### Avantages

- 📚 **Documentation à jour** : Informations synchronisées avec les sources officielles
- 🎯 **Contexte précis** : Réponses basées sur les versions spécifiques que vous utilisez
- 🚀 **Amélioration des suggestions** : Claude fait des recommandations plus précises
- 🔄 **Support multi-framework** : React, Vue, Angular, Node.js, Python, etc.

#### Installation

**Étape 1 : Obtenir une clé API**

1. Visitez [context7.com](https://context7.com)
2. Créez un compte ou connectez-vous
3. Générez une clé API depuis votre dashboard
4. Copiez la clé API

**Étape 2 : Installer le serveur MCP**

```bash
# Installer Context7 MCP
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: YOUR_API_KEY"
```

Remplacez `YOUR_API_KEY` par votre clé API obtenue à l'étape 1.

**Étape 3 : Configurer les permissions**

Ajoutez Context7 aux permissions autorisées dans `.claude/settings.json` ou `.claude/settings.local.json` :

```json
{
  "permissions": {
    "allow": [
      "mcp__context7",
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)"
    ]
  }
}
```

**Étape 4 : Vérifier l'installation**

```bash
# Lister les MCP installés
claude mcp list

# Résultat attendu :
# context7 (http://mcp.context7.com/mcp)
```

#### Utilisation

Une fois installé, Context7 fonctionne automatiquement. Claude l'interroge quand nécessaire :

**Exemple :**
```
User: Comment utiliser les hooks useEffect avec cleanup dans React 18 ?

Claude: [Consulte Context7 pour les meilleures pratiques React 18]

En React 18, useEffect avec cleanup fonctionne ainsi :

```jsx
import { useEffect } from 'react';

function MyComponent() {
  useEffect(() => {
    // Setup
    const subscription = api.subscribe();

    // Cleanup function
    return () => {
      subscription.unsubscribe();
    };
  }, []); // Dependencies
}
```

[Réponse basée sur la documentation officielle via Context7]
```

#### Variables d'Environnement

Pour une configuration plus flexible, vous pouvez définir la clé API comme variable d'environnement :

```bash
# Dans ~/.bashrc ou ~/.zshrc
export CONTEXT7_API_KEY="votre-clé-api"

# Installer sans spécifier la clé directement
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: $CONTEXT7_API_KEY"
```

---

### Atlassian

**Atlassian MCP** permet à Claude Code d'interagir directement avec les outils de l'écosystème Atlassian.

#### Qu'est-ce qu'Atlassian MCP ?

Atlassian MCP connecte Claude Code à :

- **Jira** : Gestion de tickets et issues
- **Confluence** : Documentation et wikis
- **Bitbucket** : Gestion de code source

#### Avantages

- 🎫 **Accès direct aux tickets Jira** : Lire et créer des issues depuis Claude Code
- 📖 **Recherche Confluence** : Consulter la documentation d'équipe
- 🔗 **Lien code-tâches** : Traçabilité entre le code et les issues
- ✍️ **Création automatique** : Créer des tickets basés sur des bugs découverts
- 📊 **Synchronisation** : Contexte projet aligné avec Jira

#### Installation

```bash
# Installer Atlassian MCP avec transport SSE
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse
```

**Note :** Atlassian MCP utilise le transport SSE (Server-Sent Events) pour une communication en temps réel.

#### Configuration

Ajoutez Atlassian aux permissions :

```json
{
  "permissions": {
    "allow": [
      "mcp__atlassian",
      "WebSearch"
    ]
  }
}
```

#### Utilisation

**Récupérer un ticket Jira :**
```
User: Affiche-moi les détails du ticket PROJ-123

Claude: [Consulte Jira via Atlassian MCP]

Ticket PROJ-123 : Implémenter système de cache

Status: In Progress
Assigné à: Angelo LIMA
Sprint: Sprint 12
Story Points: 5

Description:
Migrer le système de cache de memcached vers Redis...
```

**Chercher dans Confluence :**
```
User: Comment est structurée notre architecture backend selon la doc Confluence ?

Claude: [Recherche dans Confluence]

Selon la documentation "Architecture Backend v2.0" :

1. API Gateway (Kong)
2. Services métier (Node.js + Express)
3. Event Bus (RabbitMQ)
4. Databases (PostgreSQL + Redis)
...
```

**Créer un ticket :**
```
User: Crée un ticket Jira pour documenter cette nouvelle API

Claude: [Crée le ticket via Atlassian MCP]

✅ Ticket créé : PROJ-456
Titre: Documenter la nouvelle API users
Type: Task
Assigné à: Non assigné
Priorité: Medium

Lien: https://your-domain.atlassian.net/browse/PROJ-456
```

---

### Playwright

**Playwright MCP** permet l'automatisation de navigateurs directement depuis Claude Code.

#### Qu'est-ce que Playwright MCP ?

Playwright MCP donne à Claude la capacité de :

- 🌐 **Naviguer sur le web** : Visiter des pages, remplir des formulaires
- 📸 **Capturer des écrans** : Screenshots pour debugging ou documentation
- 🧪 **Tester des interfaces** : Automatiser les tests end-to-end
- 🤖 **Scraping web** : Extraire des données de sites web
- 🔍 **Debugging visuel** : Inspector des problèmes UI

#### Avantages

- ✅ **Tests E2E automatisés** : Générer et exécuter des tests Playwright
- ✅ **Debugging visuel** : Capturer l'état de l'UI à un instant T
- ✅ **Documentation** : Screenshots automatiques pour la doc
- ✅ **Monitoring** : Vérifier que les pages se chargent correctement

#### Installation

```bash
# Installer Playwright MCP (via npx, pas d'installation globale nécessaire)
claude mcp add playwright
```

Le serveur Playwright sera automatiquement lancé via `npx` quand nécessaire.

#### Configuration

Ajoutez Playwright aux permissions :

```json
{
  "permissions": {
    "allow": [
      "mcp__playwright",
      "Bash(npm:*)"
    ]
  }
}
```

#### Utilisation

**Capturer un screenshot :**
```
User: Prends un screenshot de la page d'accueil en production

Claude: [Utilise Playwright MCP]

Je vais capturer un screenshot de la page d'accueil.

[Screenshot capturé : homepage.png]

✅ Screenshot sauvegardé dans ./homepage.png
Résolution : 1920x1080
Temps de chargement : 1.2s
```

**Tester un formulaire :**
```
User: Teste que le formulaire de login fonctionne

Claude: [Utilise Playwright MCP]

Test du formulaire de login :

1. Navigation vers /login ✅
2. Remplissage email ✅
3. Remplissage password ✅
4. Click sur "Se connecter" ✅
5. Redirection vers /dashboard ✅

✅ Test réussi : Le formulaire de login fonctionne correctement
```

**Générer un test E2E :**
```
User: Génère un test Playwright pour le parcours d'achat

Claude: [Génère le test avec Playwright MCP]

```javascript
// tests/e2e/checkout.spec.js
import { test, expect } from '@playwright/test';

test('parcours d'achat complet', async ({ page }) => {
  // Accéder à la boutique
  await page.goto('/shop');

  // Ajouter un produit au panier
  await page.click('[data-testid="add-to-cart-1"]');
  await expect(page.locator('.cart-count')).toHaveText('1');

  // Aller au panier
  await page.click('[data-testid="cart-button"]');
  await expect(page).toHaveURL('/cart');

  // Procéder au checkout
  await page.click('[data-testid="checkout-button"]');

  // Remplir les informations
  await page.fill('[name="email"]', 'test@example.com');
  await page.fill('[name="cardNumber"]', '4242424242424242');
  await page.fill('[name="expiry"]', '12/25');
  await page.fill('[name="cvc"]', '123');

  // Confirmer l'achat
  await page.click('[data-testid="confirm-purchase"]');

  // Vérifier la confirmation
  await expect(page.locator('.success-message')).toBeVisible();
  await expect(page).toHaveURL(/\/order\/[0-9]+/);
});
```

✅ Test généré et sauvegardé
```

---

## Installation

### Installation Globale des MCP

Pour installer tous les serveurs MCP recommandés :

```bash
# Context7 (nécessite une clé API)
export CONTEXT7_API_KEY="votre-clé-api"
claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: $CONTEXT7_API_KEY"

# Atlassian
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse

# Playwright
claude mcp add playwright
```

### Installation Par Projet

Vous pouvez aussi configurer les MCP au niveau du projet avec `.claude/.mcp.json` :

```json
{
  "mcpServers": {
    "context7": {
      "transport": "http",
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "${CONTEXT7_API_KEY}"
      }
    },
    "atlassian": {
      "transport": "sse",
      "url": "https://mcp.atlassian.com/v1/sse"
    },
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp-server"]
    }
  }
}
```

---

## Configuration

### Permissions

Les serveurs MCP doivent être ajoutés aux permissions autorisées :

**`.claude/settings.json` (équipe) :**
```json
{
  "permissions": {
    "allow": [
      "WebSearch",
      "Bash(git commit:*)",
      "Bash(git add:*)",
      "Bash(npm:*)"
    ]
  },
  "enableAllProjectMcpServers": true
}
```

**`.claude/settings.local.json` (personnel) :**
```json
{
  "permissions": {
    "allow": [
      "mcp__context7",
      "mcp__atlassian",
      "mcp__playwright"
    ]
  }
}
```

### Variables d'Environnement

Pour gérer les clés API de façon sécurisée :

```bash
# Dans ~/.bashrc, ~/.zshrc ou ~/.profile
export CONTEXT7_API_KEY="votre-clé-context7"
export ATLASSIAN_TOKEN="votre-token-atlassian"
```

Puis dans `.claude/.mcp.json` :

```json
{
  "mcpServers": {
    "context7": {
      "transport": "http",
      "url": "https://mcp.context7.com/mcp",
      "headers": {
        "CONTEXT7_API_KEY": "${CONTEXT7_API_KEY}"
      }
    }
  }
}
```

---

## Gestion des Serveurs MCP

### Commandes CLI

```bash
# Lister les MCP installés
claude mcp list

# Ajouter un MCP
claude mcp add <name> [options]

# Supprimer un MCP
claude mcp remove <name>

# Mettre à jour un MCP
claude mcp update <name>

# Tester un MCP
claude mcp test <name>
```

### Exemples

**Lister les MCP :**
```bash
$ claude mcp list

Serveurs MCP installés :
- context7 (http://mcp.context7.com/mcp)
- atlassian (sse://mcp.atlassian.com/v1/sse)
- playwright (npx @playwright/mcp-server)
```

**Supprimer un MCP :**
```bash
$ claude mcp remove context7

✅ Serveur MCP 'context7' supprimé
```

**Mettre à jour la configuration d'un MCP :**
```bash
$ claude mcp add --transport http context7 https://mcp.context7.com/mcp --header "CONTEXT7_API_KEY: NEW_API_KEY"

✅ Serveur MCP 'context7' mis à jour
```

---

## Troubleshooting

### Context7 ne répond pas

**Problème** : Claude ne consulte pas Context7.

**Solutions :**

1. Vérifier la clé API :
```bash
# Tester la connexion
curl -H "CONTEXT7_API_KEY: $CONTEXT7_API_KEY" https://mcp.context7.com/mcp/health
```

2. Vérifier les permissions :
```bash
# Vérifier settings.json
cat .claude/settings.json | jq .permissions.allow

# Doit contenir "mcp__context7"
```

3. Vérifier les variables d'environnement :
```bash
echo $CONTEXT7_API_KEY
# Doit afficher votre clé
```

### Atlassian MCP timeout

**Problème** : Timeout lors des requêtes vers Atlassian.

**Solutions :**

1. Vérifier la connectivité réseau :
```bash
# Tester la connexion
curl https://mcp.atlassian.com/v1/health
```

2. Vérifier le token Atlassian :
```bash
# Le token doit avoir les permissions appropriées
# Confluence : read, write
# Jira : read, write
```

3. Augmenter le timeout :
```json
{
  "mcpServers": {
    "atlassian": {
      "transport": "sse",
      "url": "https://mcp.atlassian.com/v1/sse",
      "timeout": 30000
    }
  }
}
```

### Playwright ne se lance pas

**Problème** : Erreurs lors du lancement de Playwright.

**Solutions :**

1. Vérifier l'installation de Playwright :
```bash
# Installer les dépendances Playwright
npx playwright install
```

2. Vérifier les permissions :
```bash
# Playwright nécessite certaines permissions système
# Sur Linux : installer les dépendances
sudo npx playwright install-deps
```

3. Tester manuellement :
```bash
# Tester Playwright directement
npx playwright test --help
```

### MCP non reconnu

**Problème** : Claude ne reconnaît pas un serveur MCP installé.

**Solutions :**

1. Redémarrer Claude Code :
```bash
# Ctrl+C puis relancer
claude
```

2. Vérifier la liste des MCP :
```bash
claude mcp list
```

3. Réinstaller le MCP :
```bash
claude mcp remove <name>
claude mcp add <name> [options]
```

---

## Cas d'Usage

### Use Case 1 : Développement avec Documentation à Jour

**Scénario :** Vous développez avec une nouvelle version de React et avez besoin des dernières best practices.

**Solution avec Context7 :**

```
User: Comment utiliser React Server Components avec Next.js 14 ?

Claude: [Consulte Context7 pour Next.js 14]

Avec Next.js 14, les React Server Components sont le comportement par défaut...

[Documentation à jour via Context7]
```

**Avantage :** Documentation toujours synchronisée avec les dernières versions.

### Use Case 2 : Traçabilité Code-Tâches

**Scénario :** Vous travaillez sur un ticket Jira et voulez lier le code à la tâche.

**Solution avec Atlassian MCP :**

```
User: /epct implémenter le système de cache pour PROJ-789

Claude: [Récupère les détails de PROJ-789 via Atlassian MCP]

D'après le ticket PROJ-789, je dois implémenter un cache Redis avec :
- TTL de 1 heure
- Invalidation sur update
- Support du namespace par tenant

[Implémentation alignée avec le ticket]

✅ Implémentation terminée
💬 Voulez-vous que je mette à jour le statut du ticket à "In Review" ?
```

### Use Case 3 : Tests E2E Automatisés

**Scénario :** Vous avez ajouté une nouvelle page et voulez générer les tests E2E.

**Solution avec Playwright MCP :**

```
User: Génère les tests E2E pour la nouvelle page de profil utilisateur

Claude: [Utilise Playwright MCP pour explorer la page et générer les tests]

✅ Tests générés :

tests/e2e/user-profile.spec.js :
- Navigation vers la page profil
- Édition des informations
- Upload de photo de profil
- Sauvegarde des modifications
- Vérification de la persistance

Voulez-vous que je les exécute maintenant ?
```

### Use Case 4 : Debugging Visuel

**Scénario :** Un bug UI en production, vous voulez voir l'état actuel.

**Solution avec Playwright MCP :**

```
User: Capture l'état actuel de la page de checkout en production

Claude: [Utilise Playwright MCP]

📸 Screenshots capturés :

1. checkout-initial.png (avant remplissage)
2. checkout-filled.png (formulaire rempli)
3. checkout-error.png (message d'erreur visible)

⚠️ J'ai détecté un problème : Le message d'erreur "Card declined" ne s'affiche pas correctement.
Le div.error-message a `display: none` au lieu de `display: block`.

Voulez-vous que je crée un ticket Jira pour ce bug ?
```

---

## Autres Serveurs MCP

### MCP Communautaires

Quelques serveurs MCP populaires de la communauté :

**GitHub MCP**
```bash
claude mcp add github https://mcp.github.com/v1
```
- Accès aux repositories, issues, pull requests
- Création de branches et commits

**Slack MCP**
```bash
claude mcp add slack https://mcp.slack.com/v1
```
- Envoi de messages
- Lecture de channels
- Gestion de threads

**Notion MCP**
```bash
claude mcp add notion https://mcp.notion.so/v1
```
- Lecture et écriture de pages Notion
- Gestion de bases de données Notion

### Créer Votre Propre Serveur MCP

Voir la [documentation officielle MCP](https://modelcontextprotocol.io/) pour créer vos propres serveurs MCP personnalisés.

---

**[← Retour au README](../README.md)** | **[Guide Complet →](GUIDE.md)** | **[Voice Inc. →](VOICE.md)**
