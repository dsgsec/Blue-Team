# La CTI

Un analyste junior se connecte pour leur quart de travail et trouve deux cents nouvelles alertes qui attendent -- tout, des scans de réseau bénin à un Balise qui se sent hors de propos. La file d'attente de billets ne se rythme pas poliment; elle exige des décisions rapides et confiantes. C'est là Il gagne son don.

Le renseignement sur les menaces fournit le contexte qui aide un analyste à décider laquelle de ces deux cents alertes représente un véritable danger. Avec un contexte fiable, le dépense de l'énergie sur les bons enjeux et dort mieux quand les tableaux de bord sont enfin calmes.

Concrètement, cherche à répondre à trois questions essentielles:

1.  **Qui, ou quoi, est à l'autre bout de cet indicateur d'alerte?**
2.  **Quel était leur comportement dans le passé ?**
3.  **Comment mon organisation répond-elle et que dois-je faire à ce sujet en ce moment?**

Lorsque ces questions sont traitées tôt -- de préférence en quelques minutes -- un analyste de L1 gagne un temps précieux en aval pour la réponse à l'incident () équipe. n'est donc pas le domaine exclusif des spécialistes de l'information; c'est un outil de première ligne qui élève Triage des conjectures à l'action calculée.

Des données brutes à l'intelligence utilisable
----------------------------------------------

La littérature sur la sécurité de l'information distingue les **données, l'information** et **l'intelligence**, mais les trois termes se brouillent souvent dans la conversation quotidienne. Les rendre explicites clarifie l'objectif d'un analyste.

| Layer | Définition | Exemple d'alerte-file d'attente | L1 action |
| --- | --- | --- | --- |
| **Données** | Un non-traitable observable | `45.155.205.3 :443` | Capturez l'artefact. |
| **Information** | Données plus annotation factuelle | *IP enregistrée à Hetzner, d'abord vue 2023-07-14* | Enregistrer les attributs. |
| **Intelligence** | Informations analysées qui *répondent à so-what* | *IP appartient à l'actuel BumbleBee C2; bloc immédiatement* | Escalade ou suppression. |

Un analyste de niveau 1 est responsable de rendre les artefacts utilisables et de les enrichir jusqu'à ce qu'ils se qualifient comme une intelligence, ou de démontrer qu'ils ne le feront jamais. Cette poussée est édictée **par** l'enrichissement: des recherches rapides et méthodiques de sources publiques, commerciales et internes qui mettent en lumière l'origine, le comportement et la pertinence.

Lors de l'ascension des données à l'intelligence, trois autres étiquettes deviennent primordiales pour les analystes à connaître.

-   **Indicateur de compromis (CIO**): Preuve d'une violation, telle qu'une adresse C2 dans les journaux.
-   **Indicateur d'attaque (IOA**): Une action malveillante, telle que le lancement d'un service inconnu, est en cours.
-   **Tactics, Techniques, and Procedures (TTP)**: An adversary's detailed methodologies expressed in MITRE ATT&CK IDs and descriptions.

Indicator Types Essential to First-Line Triage
----------------------------------------------

Every artefact demands a tailored enrichment path. Memorising tools is less important than recognising what kind of indicator the alert supplies and knowing where to look. Below, we have a table showing the types of indicators we need to be aware of, with examples:

| Indicator | Example | First Resources | Associated IOA or TTP Examples |
| --- | --- | --- | --- |
| **IPv4 / IPv6** | `45.155.205.3` | - WHOIS (ASN, allocation date) - VirusTotal Relations- Shodan banner scan | IOA: Repeated SSH failures : `T1110.003`Password Guessing |
| **Domaine / FQDN** | `malicious-updates[.]net` | - WHOIS age - RiskIQ or SecurityTrails passive-DNS - urlscan.io | IOA: surge of DNS queries to a 24-hour-old domain |
| **URL** | `hxxp://malicious-updates[.]net/login` | - URLhaus reputation - urlscan.io behaviour graph - Any.Run dynamic run (network off) | IOA: Browser POST to /gateway.php with payload |
| **Dossier hachage** | `e99a18c428cb38d5...` | - VirusTotal static & dynamic - Hybrid-Analysis - MalShare corpus | TTP: T1055 Process Injection into regsvr32.exe |
| **Adresse e-mail** | `billing@evil-corp.com` | - Analyse d'en-tête MXToolbox - Ai-je été pwned | IOA: échec SPF plus enregistrement récent de domaine |
| **Artefact local** | `HKCU\Software\Run\updater.exe` | - Règles Sigma - requête de prévalence - Base de connaissances des fournisseurs | : T1060.001 Clés d'exécution du registre |

> **Conseil actionnable.** Maintenir un dossier de signet de navigateur ou un panneau de lanceur qui ouvre vos recherches préférées avec l'indicateur surligné pré-rempli. Les trente secondes économisées par alerte en heures sur un mois.

Flux, plateformes et pourquoi la distinction compte
---------------------------------------------------

Most SOCs do not build or have intelligence in-house. The insights must be integrated and ingested from reliable sources.

**Feed**: A scheduled stream of indicators, usually delivered in various formats such as CSV, , STIX, or through TAXII. Over-ingesting feeds without curation drowns analysts in false positives and erodes trust in the programme.

**Platform**Plate-forme: Un dépôt structuré qui stocke les indicateurs, suit l'enrichissement, cartographie les relations et applique les autorisations de partage. **[](https://tryhackme.com/room/misp)**et **[OpenCTI](https://tryhackme.com/room/opencti)** sont des exemples open source.

Son La pratique introduit **progressivement** les flux, confirme qu'ils s'alignent sur le modèle de menace de l'organisation et ne les promeut sur la plate-forme qu'après avoir mesuré l'actionnabilité. La plate-forme devient alors la source unique de vérité -- un analyste l'interroge d'abord, assurant que les biographies d'indicateurs évoluent plutôt que de fourcher.

Sources de cyber-espionnage
---------------------------

Le renseignement n'est aussi digne de confiance que sa source, car il suscite la crédibilité et dirige la révision juridique si un plus tard déclenche une perturbation d'entreprise. En tant qu'analyste, vous devez savoir et noter d'où provient chaque indicateur.

Il y a quatre grandes sources que vous rencontrerez dans votre pratique:

-   **Télémétrie interne:** des logs, détections, -les soumissions de boîte aux lettres offrent la plus grande pertinence immédiate.
-   **Services commerciaux:** flux premium fournisseurs, bacs à sable payants**,** et analyses de source fermée. Ceux-ci fournissent une grande fidélité, mais peuvent avoir des limites d'exportation et de partage basées sur la licence.
-   **Intelligence open source (:** AbuseIPDB, URLhaus, blogs publics avec CIOs, et recherche académique. Avant d'appliquer, les informations provenant de ces sources devront être confirmées de manière croisée.
-   **Communautés et** ISAC: Listes sectorielles marquées d'étiquettes et de contexte riche (p. ex., FS-ISAC)

Classifications de renseignement sur les menaces
------------------------------------------------

![Un analyste qui examine les quatre classes de renseignement sur les menaces.](https://cdn-images.tryhackme.com/user-uploads/5fc2847e1bbebc03aa89fbf2/room-content/cd207b841ee45fd4e62eaf4266cc06ae.png)

L'intelligence de la menace vise à comprendre la relation entre votre environnement opérationnel et votre adversaire. Dans cet esprit, nous pouvons décomposer les informations de menace en classifications suivantes:

-   **Intel** stratégique: Des renseignements de haut niveau qui examinent le paysage des menaces de l'organisation et cartographient les domaines de risque en fonction des tendances, des modèles et des menaces émergentes qui peuvent avoir un impact sur les décisions des entreprises. Un exemple est un rapport annuel sur les tendances des ransomwares prédisant un passage à l'extorsion d'effacement des données dans les soins de santé.

-   **Intel** tactique: Évaluations des comportements des adversaires par l'analyse de tactiques, de techniques et de procédures (TTP). Cela peut prendre la forme de notes consultatives, telles que le détail du nouvel abus T1059.005 (Visual Basic) dans le malspam.

-   **Information** opérationnelle: Détails spécifiques à la campagne sur les motifs et l'intention d'effectuer une attaque. Ceci est utile pour comprendre les actifs essentiels disponibles dans l'organisation (personnes, processus et technologies) qui peuvent être ciblés.

-   **Intel technique** : Indicateurs atomiques et artefacts tels que et des hachages liés à une attaque.

Les analystes de L1 aggraveront de nombreux CIO techniques, observeront et documenteront les IOA tactiques et identifieront les modèles qui alimentent les rapports opérationnels.
