Les normes et les cadres de référence fournissent des structures permettant de rationaliser la diffusion et l'utilisation du renseignement sur les menaces dans tous les secteurs d'activité. Ils permettent également d'adopter une terminologie commune, ce qui facilite la collaboration et la communication. Nous présentons ici brièvement quelques normes et cadres de référence essentiels et couramment utilisés.

MITREATT&CK
-----------

Un malicieuxPowerShellLa commande peut sembler évidente, mais votre ticket a besoin d'une étiquette que tout le monde reconnaît.MITRE**Le cadre ATT&CK** de [nom de l'entreprise] fournit cette étiquette. Chaque technique *--- T1059.001PowerShell*, *T1048.003DNStunnel* , etc., agit comme une pierre de Rosette entre les fournisseurs, les coéquipiers et les auditeurs, fournissant des informations sur les comportements adverses.

En tant qu'analyste de niveau 1, vous pouvez utiliser la matrice lors d'une enquête de la manière suivante :

1.  Associez le comportement décrit dans l'alerte à une paire tactique/technique.
2.  Inscrivez l'identifiant dans votre note de triage : « Observation **T1071.001** (via le Web) »C2) contre FINANCE-TRYHATME-00".
3.  Remettez la note au niveau 2 ou à l'équipe de réponse aux incidents ; ils sauront instantanément quelles mesures d'atténuation et quels profils d'acteurs malveillants s'appliquent.

MITRED3FEND
-----------

Si ATT&CK répertorie les méthodes d'attaque des adversaires, **D3FEND** répertorie les réponses des défenseurs. Chaque entrée correspond à des tactiques défensives telles que le renforcement de la sécurité des identifiants ou l'obfuscation des données.

Un exemple de ce type de cas ressemblerait à ceci :

-   Tonprocurationsoulève un **T1048.003DNSAlerte tunnel** .
-   Recherchez D3FEND pour trouver la technique défensive correspondante : D3---NTDNDNS---analyse des requêtes. La page répertorie les commandes pratiques : bloquer les enregistrements TXT volumineux et activer les alertes en cas de requêtes inhabituelles.entropie.
-   Ajoutez la mesure de contrôle la plus appropriée à votre champ « actions suivantes » ; vous venez de fournir une solution d'atténuation, et non seulement un diagnostic.

Cyber ​​Kill Chain
------------------

La chaîne d'attaque cybernétique (Cyber ​​Kill Chain), développée par Lockheed Martin, décompose les actions d'un adversaire en étapes. Cette décomposition aide les analystes et les équipes de défense à identifier les activités spécifiques à chaque phase lors de l'investigation d'une attaque. Les phases définies sont illustrées dans l'image ci-dessous.


| Technique | But | Exemples |
| --- | --- | --- |
| Reconnaissance | Obtenez des informations sur la victime et les tactiques utilisées lors de l'attaque. | Collecte des adresses e-mail,OSINTet les médias sociaux, analyses de réseau |
| Armification | Les logiciels malveillants sont conçus en fonction des besoins et des intentions de l'attaque. | Exploiter une faille de sécurité, un document Office malveillant |
| Livraison | Ce document explique comment le logiciel malveillant serait installé sur le système de la victime. | Courriel, liens Web, clé USB |
| Exploitation | Exploiter les vulnérabilités du système de la victime pour exécuter du code et créer des tâches planifiées afin d'établirpersistance. | EternalBlue, Zero-Logon, etc. |
| Installation | Installer des logiciels malveillants et d'autres outils pour accéder au système de la victime. | Extraction de mots de passe, portes dérobées et chevaux de Troie d'accès à distance |
| Commandement et contrôle | Contrôler à distance le système compromis, déployer des logiciels malveillants supplémentaires, accéder à des ressources précieuses et élever ses privilèges. | Empire, Cobalt Strike, etc. |
| Actions relatives aux objectifs | Réaliser les objectifs visés par l'attaque : gain financier, espionnage industriel et exfiltration de données. | Chiffrement des données, rançongiciels et défiguration publique |

Au fil du temps, la chaîne d'élimination a été étendue à l'aide d'autres cadres, tels que ATT&CK, et une nouvelle chaîne d'élimination unifiée a été formulée.

CVE,CVSSet le NVD
-----------------

UNSOCLa file d'attente contient presque autant de notifications de vulnérabilité que d'alertes de logiciels malveillants.SOCAnalyste de niveau 1, vous devez savoir comment identifier et organiser les notifications de vulnérabilité.

-   **CVE(Vulnérabilités et expositions communes)** --- fournit un numéro de catalogue pour les vulnérabilités découvertes, par exemple, *CVE-2023-4863* .
-   **CVSS(Système de notation des vulnérabilités communes)** --- une échelle de gravité de 0 à 10 avec des modificateurs temporels et environnementaux pour les vulnérabilités.
-   **NVD (National Vulnerability Database)** --- le référentiel canonique qui relieCVEnombres àCVSSscores, exploits et produits concernés.

Partage et traitement des renseignements
----------------------------------------

Nous avons déjà abordé les plateformes et les flux permettant de recueillir des renseignements sur les menaces. Lorsque des organisations publient de nouveaux indicateurs, chaque acteur qui les utilise et les valide contribue à renforcer la défense collective et à proposer des améliorations. Ces informations semblent reposer sur deux normes : **STIX et TAXII.**

-   **STIX** : Nous avons déjà mentionné STIX comme étant structuréJSONschéma de description des informations sur les menaces.
-   **TAXII** (Trusted Automated eXchange of Indicator Information) est un ensemble d'API sécurisées permettant l'échange de renseignements sur les menaces en temps quasi réel, à des fins de détection, de prévention et d'atténuation.\
    Il prend en charge deux modèles de partage : **la collecte** , qui garantit la collecte et l'hébergement des renseignements sur les menaces par un producteur, et **la diffusion** , qui publie ces renseignements auprès des utilisateurs à partir d'un serveur central.

Le partage de renseignements sur les menaces présente des avantages, les flux d'informations quasi en temps réel réduisant le délai entre un incident survenu chez une autre organisation et la mise en œuvre de ses propres procédures préventives. De plus, les contributions de la communauté permettent aux organisations de se faire confiance mutuellement en tant que sources de renseignements précieuses.

Cela dit, tous les indicateurs ne doivent pas être divulgués. Les lois sur la protection de la vie privée, les accords de confidentialité avec les clients ou les informations concurrentielles internes peuvent interdire leur divulgation, et la diffusion précoce d'indicateurs de compromission spécifiques peut alerter les adversaires et leur faire comprendre que leur campagne a été détectée.
