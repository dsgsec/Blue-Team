# Abuse.ch – Notes de révision

## Définition

**Abuse.ch** est un projet de recherche suisse hébergé par la **Haute École spécialisée bernoise (Bern University of Applied Sciences)**.

Son objectif est de **collecter, analyser et partager des renseignements sur les menaces (Threat Intelligence)** afin d'aider les analystes en cybersécurité à détecter, comprendre et bloquer les activités malveillantes.

Abuse.ch met gratuitement à disposition plusieurs plateformes spécialisées permettant de rechercher :

- des malwares,
- des serveurs de commande et contrôle (C2),
- des certificats SSL malveillants,
- des URL malveillantes,
- des indicateurs de compromission (IOC).

---

# Objectifs principaux

Abuse.ch permet de :

- partager des renseignements sur les menaces (Threat Intelligence)
- identifier rapidement des IOC
- rechercher des malwares connus
- détecter des infrastructures malveillantes
- enrichir une enquête de cybersécurité
- alimenter des outils de détection (SIEM, IDS, Firewall...)

---

# Les plateformes d'Abuse.ch

Abuse.ch regroupe **5 plateformes principales** :

1. MalwareBazaar
2. Feodo Tracker
3. SSL Blacklist
4. URLhaus
5. ThreatFox

Chaque plateforme répond à un besoin précis.

---

# 1. MalwareBazaar

## Définition

**MalwareBazaar** est une base de données collaborative contenant des **échantillons de logiciels malveillants**.

Les chercheurs et analystes peuvent :

- déposer un malware
- rechercher un malware
- télécharger un échantillon
- enrichir les informations disponibles

Il s'agit d'une immense bibliothèque de malwares.

---

## Fonctionnalités

### Upload

Permet de déposer un nouvel échantillon.

Les chercheurs peuvent envoyer :

- un exécutable
- un document Office malveillant
- un script
- une archive
- etc.

---

### Téléchargement

Les analystes peuvent récupérer des échantillons afin de :

- réaliser une analyse statique
- réaliser une analyse dynamique
- créer des signatures de détection
- entraîner des outils de sécurité

---

### Recherche

Les recherches peuvent être effectuées selon :

- SHA256
- MD5
- SHA1
- nom du malware
- famille
- tags
- signature YARA
- signature ClamAV
- fournisseur antivirus

---

## Informations disponibles

Pour chaque malware :

- Nom
- Famille
- Taille
- Date d'ajout
- Hash SHA256
- Hash MD5
- Type de fichier
- Signature
- Tags
- URL d'origine (si connue)

---

## Cas d'utilisation

Exemple :

Vous trouvez un fichier suspect.

Vous calculez son SHA256 :

```
9b3f...
```

Vous recherchez ce hash dans MalwareBazaar.

Résultat :

- AgentTesla
- RedLine
- Emotet
- QakBot

Vous savez immédiatement à quelle famille appartient le malware.

---

# 2. Feodo Tracker

## Définition

**Feodo Tracker** est une base de données des serveurs **Command & Control (C2)** utilisés par plusieurs botnets.

Un serveur C2 est un serveur contrôlant les machines infectées.

---

## Botnets surveillés

Feodo Tracker suit principalement :

- Emotet
- Dridex
- TrickBot
- QakBot
- BazarLoader
- BazarBackdoor
- Heodo (Feodo)

---

## Informations disponibles

Pour chaque serveur :

- Adresse IP
- Port
- Date de détection
- État (actif/inactif)
- Malware associé
- ASN
- Pays
- Hébergeur

---

## Utilité

Si une machine communique avec une IP présente dans Feodo Tracker :

➡️ il est très probable qu'elle soit compromise.

Les données servent à :

- bloquer les IP
- créer des règles IDS
- enrichir un SIEM
- détecter des infections

---

## Exemple

Logs firewall :

```
192.168.1.25
     ↓
185.xxx.xxx.xxx
```

Recherche dans Feodo Tracker :

```
185.xxx.xxx.xxx
```

Résultat :

```
TrickBot C2
```

La machine doit être investiguée.

---

# 3. SSL Blacklist

## Définition

Cette plateforme recense les **certificats SSL/TLS utilisés par des infrastructures malveillantes**.

Les botnets utilisent souvent HTTPS pour masquer leurs communications.

SSL Blacklist permet d'identifier ces certificats.

---

## Informations disponibles

- certificat SSL
- empreinte SHA1
- empreinte SHA256
- date
- malware associé
- serveur C2

---

## Les empreintes JA3 et JA3S

### JA3

Empreinte d'un **client TLS**.

Elle permet d'identifier :

- un navigateur
- un malware
- un script

---

### JA3S

Empreinte du **serveur TLS**.

Elle permet d'identifier le serveur distant.

---

## Pourquoi c'est utile ?

Même si une IP change :

- le certificat SSL
- ou l'empreinte JA3

peuvent rester identiques.

On peut ainsi détecter des infrastructures malveillantes malgré les changements d'adresse IP.

---

## Utilisations

- Threat Hunting
- IDS
- Suricata
- Zeek
- Firewall
- SIEM

---

# 4. URLhaus

## Définition

**URLhaus** est une base de données d'URL utilisées pour distribuer des malwares.

Elle référence :

- URL malveillantes
- domaines
- fichiers infectés
- campagnes

---

## Recherche

On peut rechercher :

- URL
- domaine
- hash
- adresse IP
- type de fichier

---

## Informations disponibles

Pour chaque URL :

- statut
- malware distribué
- pays
- hébergeur
- date de découverte
- type de fichier
- hash
- URL de téléchargement

---

## Flux disponibles

URLhaus fournit des listes filtrées selon :

- pays
- ASN
- TLD (.com, .ru, .xyz...)
- malware

Ces flux peuvent être utilisés pour alimenter automatiquement :

- Firewall
- Proxy
- IDS
- SIEM

---

## Exemple

URL trouvée dans un e-mail :

```
hxxp://evil-site.com/update.exe
```

Recherche dans URLhaus.

Résultat :

```
RedLine Stealer
```

Le lien est confirmé comme malveillant.

---

# 5. ThreatFox

## Définition

**ThreatFox** est une plateforme dédiée au partage des **IOC (Indicators of Compromise)**.

Les analystes peuvent :

- rechercher
- partager
- exporter
- réutiliser des IOC

---

## Types d'IOC

ThreatFox contient notamment :

- adresses IP
- domaines
- URL
- hashes
- certificats
- fichiers
- signatures

---

## Formats d'export

Les IOC peuvent être exportés dans plusieurs formats :

- JSON
- CSV
- MISP
- Suricata Rules
- DNS RPZ
- Hosts File

---

## Pourquoi est-ce utile ?

Les exports permettent d'intégrer directement les IOC dans :

- SIEM
- IDS
- Firewall
- EDR
- outils de Threat Hunting

---

# Comment utiliser Abuse.ch lors d'une investigation ?

Supposons que vous analysez une alerte.

Vous disposez :

- d'une IP
- d'une URL
- d'un hash
- d'un certificat

Selon le type d'information, utilisez la plateforme adaptée :

| Information | Plateforme |
|-------------|------------|
| Hash d'un malware | MalwareBazaar |
| Serveur C2 | Feodo Tracker |
| Certificat SSL | SSL Blacklist |
| URL suspecte | URLhaus |
| IOC (IP, domaine, hash...) | ThreatFox |

---

# Cas pratique

Vous recevez un e-mail contenant :

```
http://bad-example.com/invoice.exe
```

Étapes :

1. Vérifier l'URL dans **URLhaus**.
2. Télécharger le hash du fichier.
3. Rechercher le hash dans **MalwareBazaar**.
4. Identifier les IOC associés dans **ThreatFox**.
5. Vérifier si les IP sont présentes dans **Feodo Tracker**.
6. Contrôler le certificat SSL via **SSL Blacklist**.

Cette approche permet d'obtenir rapidement une vision complète de la menace.

---

# Complément : les IOC (Indicators of Compromise)

Un **IOC** est un élément permettant de détecter une activité potentiellement malveillante.

Exemples :

- Adresse IP
- Domaine
- URL
- Hash MD5
- Hash SHA1
- Hash SHA256
- Certificat SSL
- Empreinte JA3
- Nom de fichier
- Clé de registre
- Processus

⚠️ Un IOC **n'est pas une preuve** qu'une machine est compromise. Il indique seulement qu'un élément observé est **associé** à une menace connue.

---

# Limites

- Les bases de données ne contiennent que les menaces déjà connues.
- Les nouveaux malwares (Zero-Day) peuvent ne pas être référencés.
- Certaines infrastructures malveillantes changent très rapidement (IP, domaines, certificats).
- Les IOC doivent toujours être recoupés avec d'autres sources avant de conclure.

---

# Bonnes pratiques

Lors d'une enquête :

1. Identifier le type d'indicateur (URL, IP, hash...).
2. Choisir la plateforme Abuse.ch adaptée.
3. Vérifier les informations disponibles.
4. Corréler avec d'autres outils (VirusTotal, Urlscan, Shodan, WHOIS...).
5. Exporter les IOC utiles vers les outils de sécurité si nécessaire.

---

# À retenir (cheat sheet)

| Plateforme | Rôle principal |
|------------|----------------|
| **MalwareBazaar** | Base de données d'échantillons de malwares |
| **Feodo Tracker** | Suivi des serveurs C2 de botnets |
| **SSL Blacklist** | Certificats SSL/TLS et empreintes JA3 malveillants |
| **URLhaus** | Base de données d'URL distribuant des malwares |
| **ThreatFox** | Partage et export d'IOC |

---

# Schéma récapitulatif

```text
                Abuse.ch
                    │
 ┌──────────────────┼──────────────────┐
 │                  │                  │
 │                  │                  │
MalwareBazaar   Feodo Tracker     SSL Blacklist
     │               │                  │
 Échantillons     Serveurs C2      Certificats SSL
 de malwares       de botnets       + JA3/JA3S
     │
     ├──────────────┐
     │              │
 URLhaus       ThreatFox
     │              │
 URL de         IOC
 distribution   (IP, URL,
 de malwares    domaines, hashes...)
```

---

# Mémo ultra-rapide

> **Abuse.ch = une suite d'outils de Threat Intelligence pour rechercher des malwares, des infrastructures malveillantes et des IOC.**

**Choisir la bonne plateforme :**

- 🦠 **MalwareBazaar** → Échantillons de malwares
- 🤖 **Feodo Tracker** → Serveurs C2 de botnets
- 🔒 **SSL Blacklist** → Certificats SSL/TLS et empreintes JA3
- 🌐 **URLhaus** → URL distribuant des malwares
- 🎯 **ThreatFox** → IOC (IP, URL, domaines, hashes, certificats...)

### Astuce mnémotechnique

**M-F-S-U-T**

- **M** = **MalwareBazaar** → Malware
- **F** = **Feodo Tracker** → Feodo / Botnets / C2
- **S** = **SSL Blacklist** → SSL & JA3
- **U** = **URLhaus** → URL malveillantes
- **T** = **ThreatFox** → Threat Intelligence & IOC
