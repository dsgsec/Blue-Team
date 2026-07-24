Threat Intelligence (CTI) --- Notes pour triage L1
================================================

Pourquoi c'est utile
--------------------

Aide un analyste à prioriser rapidement parmi un flot d'alertes en répondant à 3 questions :

1.  **Qui/quoi** est derrière l'indicateur ?
2.  **Comportement passé** de cette entité ?
3.  **Comment réagir** maintenant (procédure interne) ?

→ Objectif : transformer le triage "au feeling" en décision rapide et justifiée.

Pyramide Données → Information → Intelligence
---------------------------------------------

| Niveau | Définition | Exemple | Action L1 |
| --- | --- | --- | --- |
| **Données** | Observable brut, non actionnable | `45.155.205.3:443` | Capturer l'artefact |
| **Information** | Donnée + annotation factuelle | IP chez Hetzner, vue depuis 2023-07-14 | Enregistrer les attributs |
| **Intelligence** | Info analysée qui répond au "so what" | IP = C2 actif de BumbleBee | **Bloquer / escalader** |

→ Rôle du L1 : enrichir une donnée jusqu'à l'intelligence, ou prouver qu'elle n'y arrivera jamais.

Vocabulaire clé à retenir
-------------------------

-   **IOC** (Indicator of Compromise) = preuve qu'une compromission a eu lieu (ex: IP C2 dans les logs)
-   **IOA** (Indicator of Attack) = action malveillante *en cours* (ex: lancement d'un service inconnu)
-   **TTP** (Tactics, Techniques, Procedures) = méthodologie de l'adversaire, mappée sur **MITRE ATT&CK**

Types d'indicateurs → où enrichir
---------------------------------

| Indicateur | Exemple | Ressources d'enrichissement | IOA/TTP associé |
| --- | --- | --- | --- |
| IPv4/IPv6 | `45.155.205.3` | WHOIS, VirusTotal Relations, Shodan | T1110.003 (Password Guessing) |
| Domaine/FQDN | `malicious-updates[.]net` | WHOIS age, DNS passif (SecurityTrails), urlscan.io | Surtension DNS sur domaine récent |
| URL | `hxxp://.../login` | URLhaus, urlscan.io, Any.Run (réseau off) | POST vers /gateway.php |
| Hash fichier | `e99a18c428cb38d5...` | VirusTotal, Hybrid-Analysis, MalShare | T1055 Process Injection |
| Email | `billing@evil-corp.com` | MXToolbox (headers), HaveIBeenPwned | Échec SPF + domaine récent |
| Artefact local | `HKCU\...\Run\updater.exe` | Règles Sigma, prévalence, KB vendeur | T1060.001 Registry Run Keys |

**💡 Astuce pratique** : préparer un dossier de signets/lanceur avec recherches pré-remplies par indicateur → gain de temps cumulatif énorme sur un mois.

Feed vs Platform
----------------

-   **Feed** : flux d'indicateurs programmé (CSV, STIX/TAXII). ⚠️ Trop de feeds non curés = bruit + perte de confiance.
-   **Platform** (TIP) : dépôt structuré, stocke/enrichit/relie les indicateurs, gère les droits de partage (ex: MISP, OpenCTI).
-   Bonne pratique : tester un feed → valider l'alignement avec le modèle de menace → promouvoir sur la plateforme seulement si actionnable. La plateforme = source unique de vérité.

4 sources de cyber-intel
------------------------

1.  **Télémétrie interne** (logs, détections, soumissions mailbox) --- pertinence immédiate max
2.  **Services commerciaux** (feeds premium, sandbox payants) --- haute fidélité, mais limites de licence/partage
3.  **OSINT** (AbuseIPDB, URLhaus, blogs, recherche académique) --- à croiser/valider avant usage
4.  **Communautés/ISAC** (ex: FS-ISAC) --- listes sectorielles avec contexte riche

4 classifications de Threat Intelligence
----------------------------------------

| Type | Portée | Exemple |
| --- | --- | --- |
| **Stratégique** | Haut niveau, tendances, décisions business | Rapport annuel sur montée des ransomwares |
| **Tactique** | Analyse des TTP adverses | Note sur abus de T1059.005 (VBA) dans malspam |
| **Opérationnelle** | Détails de campagne, motifs/intentions | Identification des actifs ciblés (personnes/process/tech) |
| **Technique** | Indicateurs atomiques | IOC, hashes liés à une attaque |

**Rôle du L1** : agrège les IOC techniques, observe/documente les IOA tactiques, remonte des patterns pour les rapports opérationnels.

* * * * *

*Points à retenir absolument : la pyramide Données/Info/Intelligence, la différence IOC vs IOA vs TTP, et le tableau indicateur → ressource d'enrichissement.*
