# Urlscan.io – Notes de révision

## Définition

**Urlscan.io** est un service gratuit permettant d'analyser un site web de manière automatisée.

L'outil ouvre l'URL dans un navigateur contrôlé ("sandbox"), visite le site comme le ferait un utilisateur et enregistre toutes les interactions réalisées pendant le chargement de la page.

Il est principalement utilisé en **OSINT**, **analyse de malware**, **investigation de phishing** et **analyse de sécurité web**.

---

# Objectifs principaux

Lors d'un scan, Urlscan.io permet de :

- Identifier les domaines contactés.
- Identifier les adresses IP utilisées.
- Observer les requêtes HTTP effectuées.
- Voir les fichiers téléchargés (JS, CSS, images...).
- Détecter les redirections.
- Identifier les technologies utilisées.
- Capturer une image (screenshot) du site.
- Collecter des métadonnées utiles pour une enquête.

En résumé :

> **Urlscan montre tout ce qu'un navigateur voit lorsqu'il visite un site.**

---

# Fonctionnement

1. L'utilisateur soumet une URL.
2. Urlscan ouvre automatiquement cette URL.
3. Le navigateur charge entièrement la page.
4. Toutes les communications réseau sont enregistrées.
5. Un rapport détaillé est généré.

Le rapport contient :

- Capture d'écran
- Requêtes réseau
- Domaines contactés
- Adresses IP
- Certificats SSL
- Cookies
- Scripts exécutés
- Technologies détectées

---

# Types de données collectées

## Domaines

Liste de tous les domaines contactés.

Exemple :

- example.com
- googleapis.com
- cloudflare.com

Cela permet de voir si un site communique avec des services tiers.

---

## Adresses IP

Chaque domaine résolu possède une ou plusieurs adresses IP.

Ces informations permettent de :

- localiser un serveur
- rechercher sa réputation
- retrouver l'hébergeur

---

## Requêtes HTTP

Toutes les requêtes effectuées sont enregistrées.

Exemples :

- HTML
- CSS
- JavaScript
- Images
- API
- Polices d'écriture

---

## Capture d'écran

Urlscan prend automatiquement une capture du site.

Très utile pour :

- vérifier l'apparence d'un site
- identifier un faux site de phishing
- conserver une preuve

---

## Technologies détectées

Urlscan identifie automatiquement les technologies utilisées.

Exemples :

- WordPress
- React
- Angular
- jQuery
- Bootstrap
- Cloudflare

---

## Cookies

Le rapport affiche :

- les cookies créés
- leur nom
- leur valeur
- leur domaine
- leur durée de vie

Ces informations permettent parfois d'identifier le framework utilisé.

---

# Les deux vues principales

## 1. Latest Scans

Affiche les analyses récemment effectuées.

On peut y voir :

- URL analysée
- Date
- Capture d'écran
- Résultat

---

## 2. Live Scans

Affiche les analyses en cours d'exécution en temps réel.

Utile pour observer :

- les nouveaux scans
- les campagnes de phishing en cours
- les analyses réalisées par d'autres utilisateurs

---

# Les sections importantes du rapport

## 1. Summary (Résumé)

La section la plus importante.

Elle contient :

- URL analysée
- Adresse IP
- Domaine
- WHOIS
- Hébergeur
- Screenshot
- Historique du domaine
- Pays d'hébergement

À consulter en premier.

---

## 2. HTTP

Affiche toutes les connexions HTTP.

On y retrouve :

- requêtes GET
- requêtes POST
- codes HTTP
- types de fichiers
- taille des fichiers

Exemple :

| Code | Signification |
|-------|---------------|
| 200 | Succès |
| 301 | Redirection permanente |
| 302 | Redirection temporaire |
| 403 | Accès interdit |
| 404 | Ressource introuvable |
| 500 | Erreur serveur |

---

## 3. Redirects

Liste toutes les redirections.

Deux types :

### Redirection HTTP

Exemple :

```
site.com
    ↓
www.site.com
```

ou

```
http://
    ↓
https://
```

---

### Redirection côté client

Effectuée via :

- JavaScript
- Meta Refresh

Très utilisée par :

- les sites malveillants
- certaines campagnes de phishing

---

## 4. Links

Liste tous les liens sortants.

Permet d'identifier :

- partenaires
- CDN
- API
- réseaux sociaux
- services externes

---

## 5. Behaviour

Analyse le comportement du site.

On y retrouve :

- cookies
- variables JavaScript
- stockage local (Local Storage)
- Session Storage

Cette partie aide souvent à reconnaître :

- React
- Angular
- Vue.js
- WordPress
- autres frameworks

---

## 6. Indicators

Liste tous les indicateurs observés (IOCs - Indicators of Compromise).

Exemples :

- domaines
- adresses IP
- URLs
- certificats
- hachages (MD5, SHA1, SHA256)

⚠️ **Important :**

La présence d'un indicateur **ne signifie pas qu'il est malveillant**.

Il indique simplement qu'il a été observé lors de la visite du site.

---

# Pourquoi utiliser Urlscan ?

## En cybersécurité

- analyser un site suspect
- vérifier un lien avant de cliquer
- enquêter sur un phishing
- analyser un malware
- observer les connexions réseau

---

## En OSINT

Permet de récupérer rapidement :

- infrastructure réseau
- hébergeur
- domaines associés
- CDN utilisés
- fournisseurs tiers

---

## En Blue Team

- détecter des communications inhabituelles
- vérifier des IOC
- analyser un domaine compromis

---

## En Red Team

- comprendre la surface d'attaque
- identifier les technologies utilisées
- cartographier les ressources externes

---

# Limites

Les résultats peuvent varier selon :

- le jour de l'analyse
- les modifications du site
- les changements d'infrastructure
- les CDN utilisés
- les campagnes temporaires

Deux scans du même site peuvent donc produire des résultats différents.

---

# Exemple d'analyse

Supposons l'analyse de :

```
https://example.com
```

Urlscan peut révéler :

```
example.com
│
├── Hébergé chez Cloudflare
├── IP : 104.x.x.x
├── HTTPS activé
├── Technologies :
│      ├── React
│      ├── Bootstrap
│      └── Google Analytics
├── 24 requêtes HTTP
├── 5 domaines externes
├── 3 cookies
└── Screenshot du site
```

---

# Cas pratique (TryHackMe)

Lors d'un exercice, une capture d'écran des résultats Urlscan est fournie.

Le but est généralement de retrouver :

- l'adresse IP du serveur
- l'hébergeur
- le pays
- les domaines contactés
- les redirections
- les technologies utilisées
- les cookies présents
- les liens externes
- les indicateurs (IOCs)

Toutes les réponses se trouvent dans les différentes sections du rapport.

---

# Bonnes pratiques d'analyse

1. Commencer par **Summary**.
2. Vérifier les **redirections**.
3. Examiner les **requêtes HTTP**.
4. Identifier les **technologies** utilisées.
5. Observer les **cookies**.
6. Vérifier les **liens sortants**.
7. Examiner les **Indicators**.
8. Corréler les informations avec d'autres outils OSINT (VirusTotal, Shodan, WHOIS, etc.).

---

# À retenir (cheat sheet)

| Élément | Utilité |
|---------|----------|
| **Summary** | Vue d'ensemble (IP, domaine, screenshot, WHOIS) |
| **HTTP** | Toutes les requêtes réseau |
| **Redirects** | Redirections HTTP et JavaScript |
| **Links** | Liens sortants détectés |
| **Behaviour** | Cookies, variables JS, stockage local |
| **Indicators** | Domaines, IP, URLs et hachages observés |
| **Latest Scans** | Dernières analyses réalisées |
| **Live Scans** | Analyses en cours en temps réel |

---

# Mémo ultra-rapide

> **Urlscan = "Le navigateur enregistre tout."**

Il permet de voir :

- 🌐 Les domaines contactés
- 📡 Les adresses IP
- 📁 Les ressources téléchargées
- 🔄 Les redirections
- 🍪 Les cookies
- ⚙️ Les technologies utilisées
- 📸 Une capture d'écran
- 🔍 Les indicateurs (IOC)

**Ordre d'analyse recommandé :**

```
Summary
    ↓
HTTP
    ↓
Redirects
    ↓
Links
    ↓
Behaviour
    ↓
Indicators
```
