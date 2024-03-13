Présentation de l'investigation Windows
=======================================

Dans cette section, nous fournirons un aperçu concis des principaux artefacts Windows et procédures médico-légales.

NTFS
----

NTFS (New Technology File System) est un système de fichiers propriétaire développé par Microsoft dans le cadre de sa famille de systèmes d'exploitation Windows NT. Il a été introduit avec la sortie de Windows NT 3.1 en 1993 et ​​est depuis devenu le système de fichiers par défaut et le plus largement utilisé dans les systèmes d'exploitation Windows modernes, notamment Windows XP, Windows 7, Windows 8, Windows 10 et leurs homologues serveur.

NTFS a été conçu pour répondre à plusieurs limitations de son prédécesseur, le système de fichiers FAT (File Allocation Table). Il a introduit de nombreuses fonctionnalités et améliorations qui ont amélioré la fiabilité, les performances, la sécurité et les capacités de stockage du système de fichiers.

Voici quelques-uns des principaux artefacts médico-légaux que les enquêteurs numériques analysent souvent lorsqu'ils travaillent avec des systèmes de fichiers NTFS :

-   `File Metadata`: NTFS stocke des métadonnées complètes pour chaque fichier, y compris l'heure de création, l'heure de modification, l'heure d'accès et les informations sur les attributs (telles que les attributs de fichier en lecture seule, masqués ou système). L'analyse de ces horodatages peut aider à établir des délais et à reconstruire les activités des utilisateurs.
-   `MFT Entries`: La table de fichiers maîtres (MFT) est un composant crucial de NTFS qui stocke les métadonnées de tous les fichiers et répertoires d'un volume. L'examen des entrées MFT fournit des informations sur les noms de fichiers, les tailles, les horodatages et les emplacements de stockage des données. Lorsque les fichiers sont supprimés, leurs entrées MFT sont marquées comme disponibles, mais les données peuvent rester sur le disque jusqu'à ce qu'elles soient écrasées.
-   `File Slack and Unallocated Space`: L'espace non alloué sur un volume NTFS peut contenir des restes de fichiers supprimés ou des fragments de données. La marge de fichier fait référence à la partie inutilisée d'un cluster qui peut contenir des données d'un fichier précédent. Les outils médico-légaux numériques peuvent aider à récupérer et à analyser les données de ces zones.
-   `File Signatures`: Les en-têtes et signatures de fichiers peuvent être utiles pour identifier les types de fichiers même lorsque les extensions de fichiers ont été modifiées ou masquées. Ces informations sont essentielles pour reconstruire les types de fichiers présents sur un système.
-   `USN Journal`: Le journal USN (Update Sequence Number) est un journal tenu par NTFS pour enregistrer les modifications apportées aux fichiers et aux répertoires. Les enquêteurs médico-légaux peuvent analyser le journal USN pour suivre les modifications, suppressions et renommages de fichiers.
-   `LNK Files`: Les fichiers de raccourci Windows (fichiers LNK) contiennent des informations sur le fichier ou le programme cible, ainsi que des horodatages et des métadonnées. Ces fichiers peuvent fournir des informations sur les fichiers récemment consultés ou les programmes exécutés.
-   `Prefetch Files`: Les fichiers Prefetch sont générés par Windows pour améliorer les performances de démarrage des applications. Ces fichiers peuvent indiquer quels programmes ont été exécutés sur le système et quand ils ont été exécutés pour la dernière fois.
-   `Registry Hives`: Bien qu'elles ne soient pas directement liées au système de fichiers, les ruches du registre Windows contiennent des informations importantes sur la configuration et le système. Des activités malveillantes ou des modifications non autorisées peuvent laisser des traces dans le registre, que les enquêteurs légistes analysent pour comprendre les modifications du système.
-   `Shellbags`: Les Shellbags sont des entrées de registre qui stockent les paramètres d'affichage des dossiers, tels que les positions des fenêtres et les préférences de tri. L'analyse des shellbags peut révéler les modèles de navigation des utilisateurs et potentiellement identifier les dossiers consultés.
-   `Thumbnail Cache`: Les caches de vignettes stockent des aperçus miniatures des images et des documents. Ces caches peuvent révéler les fichiers récemment consultés, même si les fichiers d'origine ont été supprimés.
-   `Recycle Bin`: La Corbeille contient des fichiers qui ont été supprimés du système de fichiers. L'analyse de la corbeille peut aider à récupérer les fichiers supprimés et fournir des informations sur les actions des utilisateurs.
-   `Alternate Data Streams (ADS)`: Les ADS sont des flux de données supplémentaires associés aux fichiers. Des acteurs malveillants peuvent utiliser l'ADS pour cacher des données, et les enquêteurs légistes doivent examiner ces flux pour garantir une analyse complète.
-   `Volume Shadow Copies`: NTFS prend en charge les clichés instantanés de volume, qui sont des instantanés du système de fichiers à différents moments. Ces copies peuvent être utiles pour la récupération de données et l'analyse des modifications apportées au fil du temps.
-   `Security Descriptors and ACLs`: Les listes de contrôle d'accès (ACL) et les descripteurs de sécurité déterminent les autorisations des fichiers et des dossiers. L'analyse de ces artefacts permet de comprendre les droits d'accès des utilisateurs et les failles de sécurité potentielles.

Journaux d'événements Windows
-----------------------------

`Windows Event Logs`font partie intégrante du système d'exploitation Windows, stockant les journaux de différents composants du système, y compris le système lui-même, les applications qui y sont exécutées, les fournisseurs ETW, les services et autres.

La journalisation des événements Windows offre des fonctionnalités de journalisation complètes pour les erreurs d'application, les événements de sécurité et les informations de diagnostic. En tant que professionnels de la cybersécurité, nous exploitons largement ces journaux pour l'analyse et la détection des intrusions.

Les tactiques adverses, depuis la compromission initiale à l'aide de logiciels malveillants ou d'autres exploits, jusqu'à l'accès aux informations d'identification, l'élévation des privilèges et les mouvements latéraux à l'aide des outils internes du système d'exploitation Windows, sont souvent capturées via les journaux d'événements Windows.

En consultant les journaux d'événements Windows disponibles, les enquêteurs peuvent avoir une bonne idée de ce qui est enregistré et même rechercher des entrées de journal spécifiques. Pour accéder directement aux journaux pour une analyse hors ligne, les enquêteurs doivent accéder au chemin de fichier par défaut pour le stockage des journaux à l'adresse `C:\Windows\System32\winevt\logs`.

L'analyse des journaux d'événements Windows a été abordée dans les modules intitulés `Windows Event Logs & Finding Evil`et `YARA & Sigma for SOC Analysts`.

Artefacts d'exécution
---------------------

`Windows execution artifacts`faire référence aux traces et aux preuves laissées sur un système d'exploitation Windows lors de l'exécution de programmes et de processus. Ces artefacts fournissent des informations précieuses sur l'exécution d'applications, de scripts et d'autres composants logiciels, qui peuvent s'avérer cruciales dans les enquêtes d'investigation numérique, la réponse aux incidents et l'analyse de la cybersécurité. En examinant les artefacts d'exécution, les enquêteurs peuvent reconstituer des chronologies, identifier des activités malveillantes et établir des modèles de comportement. Voici quelques types courants d'artefacts d'exécution Windows :

-   `Prefetch Files`: Windows gère un dossier prefetch qui contient des métadonnées sur l'exécution de diverses applications. Les fichiers de prélecture enregistrent des informations telles que les chemins d'accès aux fichiers, le nombre d'exécutions et les horodatages de l'exécution des applications. L'analyse des fichiers de prélecture peut révéler un historique des programmes exécutés et l'ordre dans lequel ils ont été exécutés.
-   `Shimcache`: Shimcache est un mécanisme Windows qui enregistre des informations sur l'exécution du programme pour faciliter l'optimisation de la compatibilité et des performances. Il enregistre des détails tels que les chemins de fichiers, les horodatages d'exécution et les indicateurs indiquant si un programme a été exécuté. Shimcache peut aider les enquêteurs à identifier les programmes récemment exécutés et leurs fichiers associés.
-   `Amcache`: Amcache est une base de données introduite dans Windows 8 qui stocke des informations sur les applications et exécutables installés. Il comprend des détails tels que les chemins d'accès aux fichiers, leurs tailles, les signatures numériques et les horodatages de la dernière exécution des applications. L'analyse d'Amcache peut fournir des informations sur l'historique d'exécution du programme et identifier les logiciels potentiellement suspects ou non autorisés.
-   `UserAssist`: UserAssist est une clé de registre qui conserve des informations sur les programmes exécutés par les utilisateurs. Il enregistre des détails tels que les noms d'applications, le nombre d'exécutions et les horodatages. L'analyse des artefacts UserAssist peut révéler un historique des applications exécutées et de l'activité des utilisateurs.
-   `RunMRU Lists`: Les listes RunMRU (les plus récemment utilisées) dans le registre Windows stockent des informations sur les programmes récemment exécutés à partir de divers emplacements, tels que les clés `Run`et `RunOnce`. Ces listes peuvent indiquer quels programmes ont été exécutés, quand ils ont été exécutés et potentiellement révéler l'activité des utilisateurs.
-   `Jump Lists`: les listes de raccourcis stockent des informations sur les fichiers, dossiers et tâches récemment consultés associés à des applications spécifiques. Ils peuvent fournir des informations sur les activités des utilisateurs et les fichiers récemment utilisés.
-   `Shortcut (LNK) Files`: les fichiers de raccourci peuvent contenir des informations sur l'exécutable cible, les chemins d'accès aux fichiers, les horodatages et les interactions de l'utilisateur. L'analyse des fichiers LNK peut révéler des détails sur les programmes exécutés et le contexte dans lequel ils ont été exécutés.
-   `Recent Items`: Le dossier Éléments récents contient une liste des fichiers récemment ouverts. Il peut fournir des informations sur les documents récemment consultés et l'activité des utilisateurs.
-   `Windows Event Logs`: Divers journaux d'événements Windows, tels que les journaux de sécurité, d'applications et système, enregistrent les événements liés à l'exécution du programme, y compris la création et l'arrêt de processus, les plantages d'applications, etc.

| Artefact | Clé d'emplacement/de registre | Données stockées |
| --- | --- | --- |
| Prélecture des fichiers | C:\Windows\Prefetch | Métadonnées sur les applications exécutées (chemins d'accès aux fichiers, horodatages, nombre d'exécutions) |
| Shimcache | Registre : HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache | Détails d'exécution du programme (chemins de fichiers, horodatages, indicateurs) |
| Amcache | C:\Windows\AppCompat\Programs\Amcache.hve (ruche de registre binaire) | Détails de l'application (chemins d'accès aux fichiers, tailles, signatures numériques, horodatages) |
| Assistance utilisateur | Registre : HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\UserAssist | Détails du programme exécuté (noms d'application, nombre d'exécutions, horodatages) |
| Listes RunMRU | Registre : HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU | Programmes récemment exécutés et leurs lignes de commande |
| Listes de raccourcis | Dossiers spécifiques à l'utilisateur (par exemple, %AppData%\Microsoft\Windows\Recent) | Fichiers, dossiers et tâches récemment consultés associés aux applications |
| Fichiers de raccourci (LNK) | Divers emplacements (par exemple, bureau, menu Démarrer) | Exécutable cible, chemins de fichiers, horodatages, interactions utilisateur |
| Articles récents | Dossiers spécifiques à l'utilisateur (par exemple, %AppData%\Microsoft\Windows\Recent) | Fichiers récemment consultés |
| Journaux d'événements Windows | C:\Windows\System32\winevt\Logs | Divers journaux d'événements contenant la création, l'arrêt et d'autres événements de processus |

Artefacts de persistance Windows
--------------------------------

La persistance de Windows fait référence aux techniques et mécanismes utilisés par les attaquants pour garantir leur présence et leur contrôle non autorisés sur un système compromis, leur permettant ainsi de conserver l'accès et le contrôle même après l'intrusion initiale. Ces méthodes de persistance exploitent divers composants du système, tels que les clés de registre, les processus de démarrage, les tâches planifiées et les services, permettant aux acteurs malveillants de résister aux redémarrages et aux mesures de sécurité tout en continuant à atteindre leurs objectifs sans être détectés.

Enregistrement

Windows `Registry`agit comme une base de données cruciale, stockant les paramètres système critiques pour le système d'exploitation Windows. Cela englobe les configurations des appareils, de la sécurité, des services et même le stockage des configurations de sécurité des comptes d'utilisateurs dans le gestionnaire de comptes de sécurité ( `SAM`). Compte tenu de son importance, il n'est pas surprenant que les adversaires ciblent souvent le registre Windows pour établir la persistance. Par conséquent, il est essentiel d'inspecter régulièrement les clés d'exécution automatique du registre.

Exemple de `Autorun`clés utilisées pour la persistance :

-   `Run/RunOnce Keys`

    -   HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Run
    -   HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\RunOnce
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\RunOnce
    -   HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Policies\
-   `Keys used by WinLogon Process`

    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon\Shell
-   `Startup Keys`

    -   Dossiers HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\User Shell
    -   Dossiers HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Shell
    -   Dossiers HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Shell
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\Utilisateur

Tâches

Windows fournit une fonctionnalité permettant aux programmes de planifier des tâches spécifiques. Ces tâches résident dans `C:\Windows\System32\Tasks`, chacune étant enregistrée sous forme de fichier XML. Ce fichier détaille le créateur, le timing ou le déclencheur de la tâche et le chemin d'accès à la commande ou au programme à exécuter. Pour examiner les tâches planifiées, nous devons naviguer `C:\Windows\System32\Tasks`et examiner le contenu des fichiers XML.

Prestations de service

`Services`dans Windows sont essentiels pour maintenir les processus sur un système, permettant aux composants logiciels de fonctionner en arrière-plan sans intervention de l'utilisateur. Les acteurs malveillants altèrent ou créent souvent des services malveillants pour assurer leur persistance et conserver les accès non autorisés. L'emplacement du registre à surveiller est : `HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services`.

Analyse médico-légale du navigateur Web
---------------------------------------

En plongeant dans l'investigation des navigateurs Web, il s'agit d'une discipline centrée sur l'analyse des restes laissés par les navigateurs Web. Ces vestiges peuvent mettre en lumière les actions des utilisateurs, les engagements en ligne et les comportements potentiellement dangereux. Certains des artefacts médico-légaux essentiels du navigateur incluent :

-   `Browsing History`: enregistrements des sites Web visités, y compris les URL, les titres, les horodatages et la fréquence des visites.
-   `Cookies`: petits fichiers de données stockés par les sites Web sur l'appareil d'un utilisateur, contenant des informations telles que les détails de la session, les préférences et les jetons d'authentification.
-   `Cache`: copies mises en cache des pages Web, des images et d'autres contenus visités par l'utilisateur. Peut révéler les sites Web consultés même si l'historique est effacé.
-   `Bookmarks/Favorites`: liens enregistrés vers des sites Web ou des pages d'intérêt fréquemment visités.
-   `Download History`: enregistrements des fichiers téléchargés, y compris les URL sources, les noms de fichiers et les horodatages.
-   `Autofill Data`: informations saisies automatiquement dans les formulaires, telles que les noms, adresses et mots de passe.
-   `Search History` : requêtes saisies dans les moteurs de recherche, accompagnées des termes de recherche et des horodatages.
-   `Session Data`: informations sur les sessions de navigation actives, les onglets et les fenêtres.
-   `Typed URLs`: URL saisies directement dans la barre d'adresse.
-   `Form Data`: informations saisies dans les formulaires Web, telles que les informations de connexion et les requêtes de recherche.
-   `Passwords`: Mots de passe enregistrés ou remplis automatiquement pour les sites Web.
-   `Web Storage`: Données de stockage local utilisées par les sites Web à diverses fins.
-   `Favicons`: Petites icônes associées aux sites Internet, qui peuvent révéler les sites visités.
-   `Tab Recovery Data`: Informations sur les onglets et sessions ouverts qui peuvent être restaurés après un crash du navigateur.
-   `Extensions and Add-ons`: Extensions de navigateur installées et leurs configurations.

SRU
---

Passant à `SRUM`(System Resource Usage Monitor), il s'agit d'une fonctionnalité introduite dans Windows 8 et les versions ultérieures. SRUM suit méticuleusement l'utilisation des ressources et les modèles d'utilisation des applications. Les données sont hébergées dans un fichier de base de données nommé `sru.db`trouvé dans le `C:\Windows\System32\sru`répertoire. Cette base de données au format SQLite permet un stockage de données structuré et une récupération efficace des données. Les enregistrements de SRUM, organisés par intervalles de temps, peuvent aider à reconstruire l'utilisation des applications et des ressources sur des durées spécifiques.

Les principales facettes de la médecine légale SRUM comprennent :

-   `Application Profiling`: SRUM peut fournir une vue complète des applications et des processus qui ont été exécutés sur un système Windows. Il enregistre des détails tels que les noms d'exécutables, les chemins d'accès aux fichiers, les horodatages et les mesures d'utilisation des ressources. Ces informations sont cruciales pour comprendre l'environnement logiciel d'un système, identifier les applications potentiellement malveillantes ou non autorisées et reconstruire les activités des utilisateurs.
-   `Resource Consumption`: SRUM capture des données sur le temps CPU, l'utilisation du réseau et la consommation de mémoire pour chaque application et processus. Ces données sont inestimables pour enquêter sur les activités gourmandes en ressources, identifier les modèles inhabituels de consommation de ressources et détecter les problèmes de performances potentiels causés par des applications spécifiques.
-   `Timeline Reconstruction`: En analysant les données SRUM, les experts en criminalistique numérique peuvent créer des calendriers d'exécution des applications et des processus, de l'utilisation des ressources et des activités du système. Cette reconstruction chronologique joue un rôle déterminant dans la compréhension de la séquence des événements, l'identification des comportements suspects et l'établissement d'une image claire des interactions et des actions des utilisateurs.
-   `User and System Context`: Les données SRUM incluent des identifiants d'utilisateur, ce qui permet d'attribuer des activités à des utilisateurs spécifiques. Cela peut faciliter l'analyse du comportement des utilisateurs et déterminer si certaines actions ont été effectuées par des utilisateurs légitimes ou par des acteurs malveillants potentiels.
-   `Malware Analysis and Detection`: Les données SRUM peuvent être utilisées pour identifier les applications inhabituelles ou non autorisées pouvant indiquer des logiciels malveillants ou des activités malveillantes. Des pics soudains d'utilisation des ressources, des modèles d'application anormaux ou des installations de logiciels non autorisées peuvent tous être détectés grâce à l'analyse SRUM.
-   `Incident Response`: Lors de la réponse aux incidents, SRUM peut fournir des informations rapides sur les activités récentes des applications et des processus, permettant aux analystes d'identifier rapidement les menaces potentielles et d'y répondre efficacement.
