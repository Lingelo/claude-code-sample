# Voice Inc. - Interaction Vocale avec Claude Code

Guide complet pour utiliser Voice Inc. afin d'interagir avec Claude Code via la voix.

---

## Table des matières

- [Qu'est-ce que Voice Inc. ?](#quest-ce-que-voice-inc-)
- [Pourquoi l'interaction vocale ?](#pourquoi-linteraction-vocale-)
- [Installation](#installation)
- [Configuration](#configuration)
- [Workflow Vocal](#workflow-vocal)
- [Bonnes Pratiques](#bonnes-pratiques)
- [Commandes Efficaces](#commandes-efficaces)
- [Troubleshooting](#troubleshooting)
- [Alternatives](#alternatives)

---

## Qu'est-ce que Voice Inc. ?

**Voice Inc.** est une application de dictée vocale qui permet d'interagir avec Claude Code sans utiliser le clavier. Elle transcrit votre voix en texte en temps réel, vous permettant de dicter vos commandes, instructions et questions à Claude.

**Site officiel :** [voiceinc.app](https://voiceinc.app/)

### Caractéristiques principales

- 🎤 **Transcription en temps réel** : Votre voix est immédiatement convertie en texte
- 🌐 **Multilingue** : Support du français, anglais et autres langues
- 🔒 **Privé** : Traitement local ou sécurisé selon le mode
- ⚡ **Rapide** : Latence minimale pour une expérience fluide
- 🎯 **Précis** : Reconnaissance vocale de haute qualité

---

## Pourquoi l'interaction vocale ?

### Avantages principaux

#### 1. 🚀 Productivité accrue

**Gain de temps :**
- La dictée vocale peut être **2-3x plus rapide** que la frappe pour des instructions complexes
- Pas besoin de taper de longs paragraphes d'explications
- Idéal pour décrire des features détaillées

**Exemple :**
```
🎤 "Je veux que tu crées un système d'authentification avec JWT,
    incluant un endpoint de login qui accepte email et password,
    génère un token avec une expiration de 24 heures,
    et stocke le refresh token dans Redis avec un TTL de 7 jours"
```

Au lieu de taper 280 caractères, vous les dictez en 15 secondes.

#### 2. 💪 Ergonomie et santé

**Réduction de la fatigue :**
- ✅ Moins de tension sur les poignets et les mains
- ✅ Prévention du syndrome du canal carpien
- ✅ Alternance entre clavier et voix pour réduire la répétition

**Idéal pour :**
- Longues sessions de développement
- Personnes avec RSI (Repetitive Strain Injury)
- Fatigue liée à la frappe intensive

#### 3. 🎯 Focus et clarté

**Meilleure formulation :**
- Parler force à structurer sa pensée
- Explications plus naturelles et complètes
- Moins de raccourcis ambigus

**Exemple :**
- ❌ Texte : "fix auth bug prod"
- ✅ Vocal : "Il y a un bug en production où l'authentification échoue pour les utilisateurs qui ont des caractères spéciaux dans leur mot de passe. Peux-tu investiguer et corriger ce problème ?"

#### 4. 🔄 Multitâche

**Mains libres :**
- Dicter tout en consultant la documentation papier
- Regarder un écran secondaire pendant la dictée
- Prendre des notes manuscrites en parallèle

---

## Installation

### macOS

**Étape 1 : Télécharger Voice Inc.**
1. Visitez [voiceinc.app](https://voiceinc.app/)
2. Téléchargez la version macOS
3. Ouvrez le fichier `.dmg`
4. Glissez Voice Inc. dans Applications

**Étape 2 : Première configuration**
1. Lancez Voice Inc.
2. Accordez les permissions microphone dans Préférences Système
3. Sélectionnez la langue (français)
4. Testez la reconnaissance vocale

**Étape 3 : Raccourcis clavier**
1. Configurez un raccourci pour activer/désactiver la dictée
2. Recommandation : `Cmd + Shift + V`

### Windows

**Étape 1 : Installation**
1. Téléchargez Voice Inc. pour Windows
2. Exécutez l'installeur
3. Suivez les instructions

**Étape 2 : Configuration microphone**
1. Ouvrez Paramètres Windows > Confidentialité > Microphone
2. Autorisez Voice Inc. à accéder au microphone
3. Testez le microphone dans Voice Inc.

### Linux

**Étape 1 : Installation**
```bash
# Via AppImage (recommandé)
wget https://voiceinc.app/downloads/voice-inc.AppImage
chmod +x voice-inc.AppImage
./voice-inc.AppImage
```

**Étape 2 : Permissions**
```bash
# Vérifier que le microphone est détecté
arecord -l

# Tester l'audio
arecord -d 5 test.wav
aplay test.wav
```

---

## Configuration

### Paramètres recommandés pour Claude Code

#### 1. Langue et modèle

**Configuration :**
- **Langue principale** : Français
- **Langue secondaire** : Anglais (pour les termes techniques)
- **Modèle** : Précision maximale (sacrifier la vitesse si nécessaire)

**Pourquoi ?**
- Le développement mélange souvent français et anglais
- Les termes techniques (API, endpoint, middleware) sont en anglais
- La précision est cruciale pour éviter les erreurs dans le code

#### 2. Ponctuation automatique

**Activer :**
- ✅ Ponctuation automatique
- ✅ Majuscules en début de phrase
- ✅ Détection des questions

**Configuration :**
```
"Ajoute un endpoint API pour récupérer les utilisateurs"
→ "Ajoute un endpoint API pour récupérer les utilisateurs."

"Comment gérer les erreurs 500"
→ "Comment gérer les erreurs 500 ?"
```

#### 3. Commandes vocales personnalisées

**Créer des raccourcis :**
- "nouvelle ligne" → Appui sur Entrée
- "code" → Insert code block
- "commande slash epct" → "/epct "

**Dans Voice Inc. :**
1. Préférences > Commandes vocales
2. Ajouter des raccourcis personnalisés
3. Tester et ajuster

#### 4. Qualité audio

**Paramètres :**
- **Réduction de bruit** : Activée (environnement bruyant)
- **Filtre anti-pop** : Activé
- **Gain micro** : Ajuster selon votre microphone

---

## Workflow Vocal

### Workflow recommandé avec /epct

#### Phase 1 : EXPLORE (Vocal)

```
🎤 "Slash epct. Ajoute un système de notifications push pour
    les utilisateurs. Le système doit supporter les notifications
    web et mobiles, avec personnalisation par utilisateur et
    stockage des préférences en base de données."
```

**Résultat dans Claude Code :**
```
/epct Ajoute un système de notifications push pour les utilisateurs.
Le système doit supporter les notifications web et mobiles, avec
personnalisation par utilisateur et stockage des préférences en
base de données.
```

**Avantage :** Description détaillée donnée naturellement en parlant.

#### Phase 2 : PLAN (Lecture + Validation vocale)

**Lecture silencieuse du plan proposé par Claude**

Puis validation vocale :
```
🎤 "Le plan me convient. Peux-tu juste ajouter une étape pour
    implémenter un système de retry en cas d'échec d'envoi ?"
```

#### Phase 3 : CODE (Observation + Questions vocales)

**Observer l'implémentation**

Si question :
```
🎤 "Pourquoi utilises-tu un worker séparé pour l'envoi des
    notifications au lieu de le faire de façon synchrone ?"
```

#### Phase 4 : TEST (Commandes vocales)

```
🎤 "Lance les tests pour le module de notifications uniquement."
```

### Alternance clavier-voix

**Principe :** Combiner clavier et voix pour efficacité maximale.

**Utiliser la voix pour :**
- ✅ Instructions longues et complexes
- ✅ Descriptions de features
- ✅ Questions et clarifications
- ✅ Explications de bugs

**Utiliser le clavier pour :**
- ✅ Noms de variables précis
- ✅ Chemins de fichiers
- ✅ Code snippets courts
- ✅ Corrections rapides

**Exemple combiné :**
```
🎤 "Crée une fonction qui valide un email. Elle doit s'appeler..."
⌨️  validateEmail
🎤 "et retourner un booléen. Elle doit vérifier que l'email contient
    un arobase et un point."
```

---

## Bonnes Pratiques

### 1. Qualité de la dictée

#### Parler clairement

**✅ À FAIRE :**
- Articuler distinctement
- Rythme régulier (ni trop rapide, ni trop lent)
- Pauses naturelles entre les phrases

**❌ À ÉVITER :**
- Marmonner
- Parler trop vite
- Couper ses phrases brusquement

#### Environnement sonore

**Idéal :**
- 🔇 Environnement calme
- 🎧 Casque avec micro intégré (meilleure qualité)
- 🚪 Porte fermée (si en bureau)

**À éviter :**
- ❌ Bruit de fond intense (café bruyant)
- ❌ Ventilateur près du micro
- ❌ Clavier mécanique bruyant pendant la dictée

### 2. Formulation des commandes

#### Structure claire

**Format recommandé :**
```
[Action] + [Contexte] + [Détails]

🎤 "Crée [ACTION]
    un endpoint API [CONTEXTE]
    qui retourne la liste des utilisateurs actifs avec pagination [DÉTAILS]"
```

#### Épeler les termes techniques

**Quand épeler :**
- Noms de variables avec casse spécifique
- Chemins de fichiers complexes
- Acronymes peu communs

**Exemple :**
```
🎤 "Crée un fichier nommé...
    u majuscule, s, e, r, tiret du bas, s, e, r, v, i, c, e, point j, s
    ...dans le dossier services"

→ User_service.js
```

**Commandes vocales utiles :**
- "majuscule" / "minuscule"
- "tout en majuscules"
- "camel case" / "snake case"
- "tiret du bas" (underscore)
- "tiret du haut" (dash)

### 3. Vérification visuelle

#### Toujours vérifier la transcription

**Avant d'envoyer :**
1. 👀 Lire la transcription à l'écran
2. ✏️ Corriger les erreurs au clavier si nécessaire
3. ✅ Valider et envoyer

**Erreurs courantes à surveiller :**
- Confusion homophones : "ver" → "vers" → "verre"
- Termes techniques mal transcrits : "Redis" → "ready"
- Ponctuation manquante ou incorrecte

#### Utiliser le mode brouillon

**Workflow :**
1. 🎤 Dicter toute la commande
2. ⏸️ Pause dictée
3. 👀 Relire et corriger
4. 📤 Envoyer

**Raccourci Voice Inc. :**
- `Cmd + Shift + V` : Pause/Reprendre dictée
- Permet de corriger sans tout redicter

### 4. Efficacité

#### Préparer mentalement

**Avant de dicter :**
1. 🤔 Structurer la pensée mentalement
2. 📋 Lister les points clés
3. 🎤 Dicter de façon fluide

**Exemple :**
```
Pensée : "Je veux un cache Redis avec TTL"

Dictée structurée :
🎤 "Implémente un système de cache utilisant Redis.
    Les entrées doivent avoir un TTL configurable,
    par défaut une heure.
    Ajoute des méthodes get, set et delete.
    Utilise le pattern singleton pour la connexion Redis."
```

#### Macros vocales

**Créer des phrases-types :**
```
"Lance EPCT" → /epct
"Nouveau fichier" → Crée un nouveau fichier dans
"Tests unitaires" → Écris les tests unitaires pour
"Debug ce bug" → Débugge le problème où
```

---

## Commandes Efficaces

### Commandes de développement

#### Créer des fichiers

```
🎤 "Crée un fichier user point service point j s dans le dossier
    services avec une classe UserService qui contient les méthodes
    get all users et get user by id"
```

#### Implémenter des features

```
🎤 "Slash epct. Ajoute un système d'upload de fichiers qui
    supporte les images et les PDFs, avec validation de taille
    maximale de 5 mégaoctets et stockage sur S3"
```

#### Débugger

```
🎤 "Il y a un bug où l'API retourne une erreur 500 quand
    l'utilisateur essaie de se connecter avec un email contenant
    un plus. Peux-tu investiguer et corriger ce problème?"
```

#### Refactoring

```
🎤 "Refactorise la fonction handle payment pour utiliser
    async await au lieu de callbacks, et extrais la logique
    de validation dans une fonction séparée"
```

### Commandes de navigation

#### Rechercher dans le code

```
🎤 "Cherche tous les fichiers qui utilisent la fonction
    authenticate user"
```

#### Lire un fichier

```
🎤 "Affiche-moi le contenu du fichier auth middleware point j s"
```

### Commandes Git

#### Commit

```
🎤 "Crée un commit avec le message : feat deux points ajout du
    système de cache Redis"
```

#### Review de changements

```
🎤 "Montre-moi les changements que tu as faits dans les fichiers
    modifiés"
```

---

## Troubleshooting

### Problème : Transcription imprécise

**Symptôme :** Voice Inc. transcrit mal vos commandes.

**Solutions :**

1. **Améliorer la qualité audio**
```bash
# Tester le microphone
# macOS
say "test microphone"

# Linux
speaker-test -t wav -c 2
```

2. **Entraîner le modèle**
- Utiliser la fonction d'entraînement de Voice Inc.
- Dicter des phrases d'exemple plusieurs fois
- Corriger les erreurs pour améliorer la reconnaissance

3. **Ajuster le débit**
- Parler plus lentement
- Faire des pauses entre les phrases
- Articuler clairement les consonnes

### Problème : Termes techniques mal reconnus

**Symptôme :** "Redis" transcrit en "ready", "JWT" en "J W T".

**Solutions :**

1. **Ajouter au dictionnaire personnalisé**
```
Voice Inc. > Préférences > Dictionnaire
Ajouter : Redis, JWT, PostgreSQL, Kubernetes, etc.
```

2. **Épeler lors de la dictée**
```
🎤 "Utilise... R E D I S ...en majuscules, pour le cache"
```

3. **Phrases d'entraînement**
```
Dicter plusieurs fois :
"Redis est une base de données en mémoire"
"JWT signifie JSON Web Token"
"Kubernetes orchestre les conteneurs"
```

### Problème : Latence élevée

**Symptôme :** Délai entre votre parole et la transcription.

**Solutions :**

1. **Vérifier la connexion (si cloud)**
```bash
# Tester la latence
ping api.voiceinc.app
```

2. **Passer en mode local**
- Voice Inc. > Préférences > Traitement
- Activer "Traitement local" (si disponible)

3. **Fermer les applications lourdes**
```bash
# Libérer des ressources CPU
# Fermer navigateurs, IDEs non utilisés
```

### Problème : Fatigue vocale

**Symptôme :** Voix fatiguée après de longues sessions.

**Solutions :**

1. **Faire des pauses régulières**
- 5 minutes de pause toutes les 30 minutes de dictée
- Boire de l'eau régulièrement

2. **Alterner avec le clavier**
- Ne pas dicter 100% du temps
- Utiliser le clavier pour les tâches courtes

3. **Volume de voix**
- Ne pas crier ou forcer la voix
- Parler à volume normal (le micro capte bien)

---

## Alternatives

### Autres outils de dictée vocale

#### 1. Whisper (OpenAI)

**Avantages :**
- Open source et gratuit
- Excellente précision
- Support multilingue

**Installation :**
```bash
pip install openai-whisper

# Utilisation
whisper audio.mp3 --model medium --language fr
```

#### 2. Dragon NaturallySpeaking

**Avantages :**
- Très haute précision
- Support professionnel
- Commandes vocales avancées

**Inconvénients :**
- Payant (prix élevé)
- Windows uniquement

#### 3. macOS Dictée intégrée

**Avantages :**
- Gratuit, intégré à macOS
- Fonctionne partout
- Pas d'installation

**Activation :**
```
Préférences Système > Clavier > Dictée
Activer la dictée améliorée
```

**Raccourci :** `Fn Fn` (double appui sur Fn)

---

## Conclusion

L'interaction vocale avec Claude Code via Voice Inc. offre :

- ✅ **Productivité accrue** : Dictée 2-3x plus rapide que la frappe
- ✅ **Ergonomie** : Moins de fatigue, prévention des RSI
- ✅ **Clarté** : Formulation plus naturelle et complète
- ✅ **Flexibilité** : Mains libres pour multitâche

**Conseil final :** Commencez progressivement avec des commandes simples, puis augmentez l'utilisation vocale au fur et à mesure que vous maîtrisez l'outil.

---

**[← Retour au README](../README.md)** | **[Guide Complet →](GUIDE.md)** | **[Documentation MCP →](MCP.md)**
