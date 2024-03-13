Techniques et outils d'acquisition de preuves
=============================================

`Evidence acquisition`est une phase critique de la criminalistique numérique, impliquant la collecte d'artefacts numériques et de données provenant de diverses sources afin de préserver les preuves potentielles à des fins d'analyse. Ce processus nécessite des outils et des techniques spécialisés pour garantir l'intégrité, l'authenticité et l'admissibilité des preuves collectées. Voici un aperçu des techniques d'acquisition de preuves couramment utilisées en criminalistique numérique :

-   Imagerie médico-légale
-   Extraction de preuves basées sur l'hôte et tri rapide
-   Extraction des preuves du réseau

Imagerie médico-légale
----------------------

L'imagerie médico-légale est un processus fondamental en criminalistique numérique qui consiste à créer une copie exacte, bit par bit, des supports de stockage numériques, tels que les disques durs, les disques SSD, les clés USB et les cartes mémoire. Ce processus est crucial pour préserver l'état d'origine des données, garantir leur intégrité et maintenir l'admissibilité des preuves dans les procédures judiciaires. L'imagerie médico-légale joue un rôle essentiel dans les enquêtes en permettant aux analystes d'examiner les preuves sans altérer ni compromettre les données originales.

Vous trouverez ci-dessous quelques outils et solutions d'imagerie médico-légale :

-   [FTK Imager](https://www.exterro.com/ftk-imager) : Développé par AccessData (maintenant acquis par Exterro), FTK Imager est l'un des outils d'imagerie disque les plus utilisés dans le domaine de la cybersécurité. Il nous permet de créer des copies (ou images) parfaites de disques informatiques à des fins d'analyse, en préservant l'intégrité des preuves. Il nous permet également de visualiser et d'analyser le contenu des périphériques de stockage de données sans altérer les données.
-   [AFF4 Imager](https://github.com/Velocidex/c-aff4) : Un outil gratuit et open source conçu pour créer et dupliquer des images de disque médico-légales. Il est convivial et compatible avec de nombreux systèmes de fichiers. L'un des avantages de l'AFF4 Imager est sa capacité à extraire des fichiers en fonction de leur temps de création, à segmenter les volumes et à réduire le temps nécessaire à l'imagerie grâce à la compression.
-   `DD and DCFLDD`: Les deux sont des utilitaires de ligne de commande disponibles sur les systèmes Unix (y compris Linux et MacOS). DD est un outil polyvalent inclus par défaut dans la plupart des systèmes basés sur Unix, tandis que DCFLDD est une version améliorée de DD avec des fonctionnalités spécifiquement utiles pour la médecine légale, telles que le hachage.
-   `Virtualization Tools`: Compte tenu de l'utilisation répandue de la virtualisation dans les systèmes modernes, les intervenants en cas d'incident devront souvent collecter des preuves à partir d'environnements virtuels. En fonction de la solution de virtualisation spécifique, des preuves peuvent être rassemblées en arrêtant temporairement le système et en transférant le répertoire qui l'héberge. Une autre méthode consiste à utiliser la capacité d'instantané présente dans de nombreux outils logiciels de virtualisation.

* * * * *

Exemple 1 : Imagerie médico-légale avec FTK Imager

Voyons maintenant une démonstration d'utilisation `FTK Imager`pour créer une image disque. N'oubliez pas que vous aurez besoin d'un support de stockage auxiliaire, comme un disque dur externe ou une clé USB, pour enregistrer l'image disque résultante.

-   Sélectionnez `File`-> `Create Disk Image`. ![](https://academy.hackthebox.com/storage/modules/237/image11.png)
-   Ensuite, sélectionnez la source multimédia. Généralement, c'est soit `Physical Drive`ou `Logical Drive`. ![](https://academy.hackthebox.com/storage/modules/237/image1.png)
-   Choisissez le lecteur à partir duquel vous souhaitez créer une image. ![](https://academy.hackthebox.com/storage/modules/237/image3.png)
-   Spécifiez la destination de l'image. ![](https://academy.hackthebox.com/storage/modules/237/image7.png)
-   Sélectionnez le type d'image souhaité. ![](https://academy.hackthebox.com/storage/modules/237/image4.png)
-   Saisissez les détails des preuves. ![](https://academy.hackthebox.com/storage/modules/237/image10.png)
-   Choisissez le dossier de destination et le nom de fichier de l'image. À cette étape, vous pouvez également ajuster les paramètres de fragmentation et de compression de l'image. ![](https://academy.hackthebox.com/storage/modules/237/image8.png)
-   Une fois tous les paramètres confirmés, cliquez sur `Start`. ![](https://academy.hackthebox.com/storage/modules/237/image2.png)
-   Vous observerez la progression de l'imagerie. ![](https://academy.hackthebox.com/storage/modules/237/image12.png)
-   Si vous avez choisi de vérifier l'image, vous verrez également la progression de la vérification. ![](https://academy.hackthebox.com/storage/modules/237/image6.png)
-   Une fois l'image vérifiée, vous recevrez un résumé de l'imagerie. Maintenant, vous êtes prêt à analyser ce dump. ![](https://academy.hackthebox.com/storage/modules/237/image9.png)

* * * * *

Exemple 2 : Montage d'une image disque avec Arsenal Image Mounter

Voyons maintenant une autre démonstration de l'utilisation [d'Arsenal Image Mounter](https://arsenalrecon.com/downloads) pour monter une image disque que nous avons précédemment créée (pas celle mentionnée ci-dessus) à partir d'une machine virtuelle (VM) compromise fonctionnant sur VMWare. Le disque dur virtuel de la VM a été stocké sous le nom `HTBVM01-000003.vmdk`.

Après avoir installé Arsenal Image Mounter, assurons-nous de le lancer avec `administrative rights`. Dans la fenêtre principale d'Arsenal Image Mounter, cliquons sur le `Mount disk image`bouton. À partir de là, nous naviguerons jusqu'à l'emplacement de notre `.VMDK`fichier et le sélectionnerons.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_mount.png)

Arsenal Image Mounter lancera alors son analyse du fichier VMDK. Nous aurons également le choix de décider si nous voulons monter le disque en tant que `read-only`ou `read-write`, en fonction de nos besoins spécifiques.

*Choisir de monter une image disque en lecture seule est une étape fondamentale de l'investigation numérique et de la réponse aux incidents. Cette approche est essentielle pour préserver l'état original des preuves, garantissant que leur authenticité et leur intégrité restent intactes.*

Une fois montée, l'image apparaîtra sous la forme d'un lecteur auquel sera attribuée la lettre `D:\`.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_drive.png)

* * * * *

Extraction de preuves basées sur l'hôte et tri rapide
-----------------------------------------------------

#### Preuve basée sur l'hôte

Les systèmes d'exploitation modernes, Microsoft Windows en étant un excellent exemple, génèrent une pléthore d'artefacts de preuves. Celles-ci peuvent provenir de l'exécution d'applications, de modifications de fichiers, ou encore de la création de comptes utilisateurs. Chacune de ces actions laisse une trace, fournissant des informations inestimables aux analystes de réponse aux incidents.

Les preuves sur un système hôte varient dans leur nature. Le terme `volatility`fait référence à la persistance des données sur un système hôte après des événements tels que des déconnexions ou des coupures de courant. Un type crucial de preuves volatiles est la mémoire active du système. Lors des enquêtes, notamment celles concernant les infections par des logiciels malveillants, cette mémoire système active devient indispensable. Les logiciels malveillants laissent souvent des traces dans la mémoire système, et la perte de ces preuves peut entraver l'enquête d'un analyste. Pour capturer la mémoire, des outils tels que [FTK Imager](https://www.exterro.com/ftk-imager) sont couramment utilisés.

Certaines autres solutions d'acquisition de mémoire sont :

-   [WinPmem](https://github.com/Velocidex/WinPmem) : WinPmem est depuis longtemps le pilote d'acquisition de mémoire open source par défaut pour Windows. Il vivait dans le projet Rekall, mais a récemment été séparé dans son propre référentiel.
-   [DumpIt](https://www.magnetforensics.com/resources/magnet-dumpit-for-windows/) : Un utilitaire simpliste qui génère un vidage de la mémoire physique des machines Windows et Linux. Sous Windows, il concatène la mémoire physique système 32 bits et 64 bits en un seul fichier de sortie, ce qui le rend extrêmement facile à utiliser.
-   [MemDump](http://www.nirsoft.net/utils/nircmd.html) : MemDump est un utilitaire de ligne de commande gratuit et simple qui nous permet de capturer le contenu de la RAM d'un système. C'est très utile dans les enquêtes médico-légales ou lors de l'analyse d'un système à la recherche d'activités malveillantes. Sa simplicité et sa facilité d'utilisation en font un choix populaire pour l'acquisition de mémoire.
-   [Belkasoft RAM Capturer](https://belkasoft.com/ram-capturer) : Il s'agit d'un autre outil puissant que nous pouvons utiliser pour l'acquisition de mémoire, fourni gratuitement par Belkasoft. Il peut capturer la RAM d'un ordinateur Windows fonctionnant sous Windows, même s'il existe une protection anti-débogage ou anti-dumping active. Cela en fait un outil très efficace pour extraire autant de données que possible lors d'une enquête médico-légale en direct.
-   [Magnet RAM Capture](https://www.magnetforensics.com/resources/magnet-ram-capture/) : Développé par Magnet Forensics, cet outil offre un moyen gratuit et simple de capturer la mémoire volatile d'un système.
-   [LiME (Linux Memory Extractor)](https://github.com/504ensicsLabs/LiME) : LiME est un Loadable Kernel Module (LKM) qui permet l'acquisition de mémoire volatile. LiME est unique dans le sens où il est conçu pour être transparent pour le système cible, évitant ainsi de nombreuses mesures anti-criminelles courantes.

* * * * *

Exemple 1 : acquisition de mémoire avec WinPmem

Voyons maintenant une démonstration d'utilisation `WinPmem`pour l'acquisition de mémoire.

Pour générer un vidage mémoire, exécutez simplement la commande ci-dessous avec les privilèges d'administrateur.

  Techniques et outils d'acquisition de preuves

```
C:\Users\X\Downloads> winpmem_mini_x64_rc2.exe memdump.raw

```

![](https://academy.hackthebox.com/storage/modules/237/image13.png)

* * * * *

Exemple 2 : acquisition de mémoire VM

Voici les étapes pour acquérir de la mémoire à partir d'une machine virtuelle (VM).

1.  Ouvrez les options de la VM en cours d'exécution
2.  Suspendre la VM en cours d'exécution
3.  Localisez le `.vmem`fichier dans le répertoire de la VM.

![](https://academy.hackthebox.com/storage/modules/237/suspend-vm.png) ![](https://academy.hackthebox.com/storage/modules/237/suspend-vmem.png)

* * * * *

D'un autre côté, les données non volatiles restent sur le disque dur, persistant généralement même lors des arrêts. Cette catégorie comprend des artefacts tels que :

-   Enregistrement
-   Journal des événements Windows
-   Artefacts liés au système (par exemple, Prefetch, Amcache)
-   Artefacts spécifiques à l'application (par exemple, journaux IIS, historique du navigateur)

#### Triage rapide

Cette approche met l'accent sur la collecte de données provenant de systèmes potentiellement compromis. L'objectif est de centraliser les données de grande valeur, en rationalisant leur indexation et leur analyse. En centralisant ces données, les analystes peuvent déployer plus efficacement des outils et des techniques, en se concentrant sur les systèmes ayant la plus grande valeur probante. Cette approche ciblée permet d'approfondir la criminalistique numérique, offrant ainsi une image plus claire des actions de l'adversaire.

L'une des meilleures solutions rapides d'analyse et d'extraction d'artefacts, sinon la meilleure, est [KAPE (Kroll Artifact Parser and Extractor)](https://www.kroll.com/en/services/cyber-risk/incident-response-litigation-support/kroll-artifact-parser-extractor-kape) . Voyons comment nous pouvons utiliser `KAPE`pour récupérer des données médico-légales précieuses à partir de l'image que nous avons précédemment montée à l'aide d'Arsenal Image Mounter ( `D:\`).

`KAPE`est un outil puissant dans le domaine de la criminalistique numérique et de la réponse aux incidents. Conçu pour aider les experts légistes et les enquêteurs, KAPE facilite la collecte et l'analyse de preuves numériques à partir de systèmes Windows. Développé et maintenu par `Kroll`(anciennement connu sous le nom de `Magnet Forensics`), KAPE est célèbre pour ses fonctionnalités de collection complètes, son adaptabilité et son interface intuitive. Le diagramme ci-dessous illustre le flux opérationnel de KAPE.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape.png)

Référence de l'image : <https://ericzimmerman.github.io/KapeDocs/#!index.md>

KAPE fonctionne sur la base des principes de `Targets`et `Modules`. Ces éléments guident l'outil dans le traitement des données et l'extraction d'artefacts médico-légaux. Lorsque nous transmettons une source à KAPE, celui-ci duplique des fichiers spécifiques liés à la médecine légale dans un répertoire de sortie désigné, tout en conservant les métadonnées de chaque fichier.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_working.png)

Après le téléchargement, décompressez le fichier et lancez KAPE. Dans le répertoire KAPE, nous remarquerons deux fichiers exécutables : `gkape.exe`et `kape.exe`. KAPE propose aux utilisateurs deux modes : CLI ( `kape.exe`) et GUI ( `gkape.exe`).

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_modes.png)

Optons pour la version GUI pour explorer plus visuellement les options disponibles.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_window.png)

Le nœud du processus réside dans la sélection des configurations cibles appropriées.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_target.png)

Dans la terminologie de KAPE, `Targets`faire référence aux artefacts spécifiques que nous souhaitons extraire d'une image ou d'un système. Ceux-ci sont ensuite dupliqués dans le répertoire de sortie.

Les fichiers cibles de KAPE ont une `.tkape`extension et résident dans le `<path to kape>\KAPE\Targets`répertoire. Par exemple, la cible `RegistryHivesSystem.tkape`dans la capture d'écran ci-dessous spécifie les emplacements et les masques de fichiers associés aux ruches de registre liées au système. Dans cette configuration cible, `RegistryHivesSystem.tkape`contient des informations pour collecter les fichiers avec un masque de fichier `SAM.LOG*`à partir du `C:\Windows\System32\config`répertoire.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_tkape.png)

KAPE propose également des `Compound Targets`, qui sont essentiellement des fusions de plusieurs cibles. Cette fonctionnalité accélère le processus de collecte en rassemblant plusieurs fichiers définis sur différentes cibles en une seule exécution. Le fichier `Compound`du répertoire `KapeTriage`fournit un aperçu du contenu de cette cible composée.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_compound.png)

Précisons notre source (dans notre scénario, c'est `D:\`) et désignons un emplacement pour stocker les données récoltées. Nous pouvons également déterminer un dossier de sortie pour héberger les données traitées par KAPE.

Après avoir configuré nos options, appuyons sur le `Execute`bouton pour lancer la collecte de données.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_gui.png)

Lors de l'exécution, KAPE commencera la collecte, en stockant les résultats dans la destination prédéterminée.

  Techniques et outils d'acquisition de preuves

```
KAPE version 1.3.0.2, Author: Eric Zimmerman, Contact: https://www.kroll.com/kape (kape@kroll.com)

KAPE directory: C:\htb\dfir_module\data\kape\KAPE
Command line:   --tsource D: --tdest C:\htb\dfir_module\data\investigation\image --target !SANS_Triage --gui

System info: Machine name: REDACTED, 64-bit: True, User: REDACTED OS: Windows10 (10.0.22621)

Using Target operations
Found 18 targets. Expanding targets to file list...
Target ApplicationEvents with Id 2da16dbf-ea47-448e-a00f-fc442c3109ba already processed. Skipping!
Target ApplicationEvents with Id 2da16dbf-ea47-448e-a00f-fc442c3109ba already processed. Skipping!
Target ApplicationEvents with Id 2da16dbf-ea47-448e-a00f-fc442c3109ba already processed. Skipping!
Target ApplicationEvents with Id 2da16dbf-ea47-448e-a00f-fc442c3109ba already processed. Skipping!
Target ApplicationEvents with Id 2da16dbf-ea47-448e-a00f-fc442c3109ba already processed. Skipping!
Found 639 files in 4.032 seconds. Beginning copy...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDefenderApiLogger.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDefenderAuditLogger.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagLog.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagtrack-Listener.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-Application.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventlog-Security.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-System.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTSgrmEtwSession.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTUBPM.etl due to UnauthorizedAccessException...
  Deferring D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTWFP-IPsec Diagnostics.etl due to UnauthorizedAccessException...
  Deferring D:\$MFT due to UnauthorizedAccessException...
  Deferring D:\$LogFile due to UnauthorizedAccessException...
  Deferring D:\$Extend\$UsnJrnl:$J due to NotSupportedException...
  Deferring D:\$Extend\$UsnJrnl:$Max due to NotSupportedException...
  Deferring D:\$Secure:$SDS due to NotSupportedException...
  Deferring D:\$Boot due to UnauthorizedAccessException...
  Deferring D:\$Extend\$RmMetadata\$TxfLog\$Tops:$T due to NotSupportedException...
Deferred file count: 17. Copying locked files...
  Copied deferred file D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDefenderApiLogger.etl to C:\htb\dfir_module\data\investigation\image\D\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDefenderApiLogger.etl. Hashing source file...
  Copied deferred file D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagLog.etl to C:\htb\dfir_module\data\investigation\image\D\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagLog.etl. Hashing source file...
  Copied deferred file D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagtrack-Listener.etl to C:\htb\dfir_module\data\investigation\image\D\Windows\System32\LogFiles\WMI\RtBackup\EtwRTDiagtrack-Listener.etl. Hashing source file...
  Copied deferred file D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-Application.etl to C:\htb\dfir_module\data\investigation\image\D\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-Application.etl. Hashing source file...
  Copied deferred file D:\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-System.etl to C:\htb\dfir_module\data\investigation\image\D\Windows\System32\LogFiles\WMI\RtBackup\EtwRTEventLog-System.etl. Hashing source file...
  Copied deferred file D:\$MFT to C:\htb\dfir_module\data\investigation\image\D\$MFT. Hashing source file...
  Copied deferred file D:\$LogFile to C:\htb\dfir_module\data\investigation\image\D\$LogFile. Hashing source file...
  Copied deferred file D:\$Extend\$UsnJrnl:$J to C:\htb\dfir_module\data\investigation\image\D\$Extend\$J. Hashing source file...
  Copied deferred file D:\$Extend\$UsnJrnl:$Max to C:\htb\dfir_module\data\investigation\image\D\$Extend\$Max. Hashing source file...
  Copied deferred file D:\$Secure:$SDS to C:\htb\dfir_module\data\investigation\image\D\$Secure_$SDS. Hashing source file...
  Copied deferred file D:\$Boot to C:\htb\dfir_module\data\investigation\image\D\$Boot. Hashing source file...

```

Le répertoire de sortie de KAPE héberge les fruits de la collecte et du traitement des artefacts. Le contenu exact de ce répertoire peut différer en fonction des artefacts sélectionnés et des configurations définies. Dans notre démonstration, nous avons opté pour la `!SANS_Triage`configuration cible de collecte. Naviguons vers le répertoire de sortie de KAPE pour inspecter les données récoltées.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_output.png)

D'après les résultats affichés, il est évident que le `$MFT`fichier a été collecté, ainsi que les répertoires `Users`et .`Windows`

Il convient de noter que KAPE a également récupéré les fichiers `Windows event logs`, qui sont nichés dans les sous-dossiers du répertoire Windows.

![](https://academy.hackthebox.com/storage/modules/237/win_dfir_kape_winevt.png)

* * * * *

Et si nous voulions effectuer une collecte d'artefacts à distance et en masse ? C'est là que les solutions EDR et [Velociraptor](https://github.com/Velocidex/velociraptor) entrent en jeu.

Les plates-formes Endpoint Detection and Response (EDR) offrent un avantage significatif aux analystes de réponse aux incidents. Ils permettent l'acquisition et l'analyse à distance de preuves numériques. Par exemple, les plateformes EDR peuvent afficher les binaires récemment exécutés ou les fichiers nouvellement ajoutés. Au lieu de passer au crible des systèmes individuels, les analystes peuvent rechercher de tels indicateurs sur l'ensemble du réseau. Un autre avantage est la capacité de rassembler des preuves, qu'il s'agisse de dossiers spécifiques ou de packages médico-légaux complets. Cette fonctionnalité accélère la collecte de preuves et facilite la recherche et la collecte à grande échelle.

[Velociraptor](https://github.com/Velocidex/velociraptor) est un outil puissant pour collecter des informations basées sur l'hôte à l'aide de requêtes Velociraptor Query Language (VQL). Au-delà de cela, Velociraptor peut s'exécuter `Hunts`pour amasser divers artefacts. Un artefact fréquemment utilisé est le `Windows.KapeFiles.Targets`. Bien que KAPE (Kroll Artifact Parser and Extractor) lui-même ne soit pas open source, sa logique de collecte de fichiers, codée en YAML, est accessible via le [projet KapeFiles](https://github.com/EricZimmerman/KapeFiles) . Cette approche est un élément essentiel du triage rapide.

Pour utiliser Velociraptor pour les artefacts KapeFiles :

-   Lancez une nouvelle chasse. ![](https://academy.hackthebox.com/storage/modules/237/vel0.png)

    ![](https://academy.hackthebox.com/storage/modules/237/image33.png)

-   Choisissez `Windows.KapeFiles.Targets`comme artefacts à collectionner. ![](https://academy.hackthebox.com/storage/modules/237/image64.png)

-   Spécifiez la collection à utiliser. ![](https://academy.hackthebox.com/storage/modules/237/vel1.png)

    ![](https://academy.hackthebox.com/storage/modules/237/image19.png)

-   Cliquez sur `Launch`pour lancer la chasse. ![](https://academy.hackthebox.com/storage/modules/237/image27.png)

-   Une fois terminé, téléchargez les résultats. ![](https://academy.hackthebox.com/storage/modules/237/image90.png)

L'extraction de l'archive révélera les fichiers liés aux artefacts collectés et à tous les fichiers collectés.

![](https://academy.hackthebox.com/storage/modules/237/image12_.png)

![](https://academy.hackthebox.com/storage/modules/237/image37.png)

Pour la collecte de vidages de mémoire à distance à l'aide de Velociraptor :

-   Commencez une nouvelle chasse, mais cette fois, sélectionnez l' `Windows.Memory.Acquisition`artefact. ![](https://academy.hackthebox.com/storage/modules/237/image92.png)
-   Une fois la chasse terminée, téléchargez l'archive résultante. À l'intérieur, vous trouverez un fichier nommé `PhysicalMemory.raw`, contenant l'image mémoire. ![](https://academy.hackthebox.com/storage/modules/237/image45.png)

Extraction des preuves du réseau
--------------------------------

Tout au long de notre exploration des modules du `SOC Analyst`parcours, nous avons approfondi le domaine des preuves de réseau, un aspect fondamental pour tout analyste SOC.

-   Tout d'abord, nos modules `Intro to Network Traffic Analysis`et couverts . Considérez la capture du trafic comme un instantané de toutes les conversations numériques qui se déroulent sur notre réseau. Des outils nous permettent de capturer et de disséquer ces paquets, nous donnant une vue granulaire des données en transit.`Intermediate Network Traffic Analysis``traffic capture analysis``Wireshark``tcpdump`

-   Ensuite, nos modules `Working with IDS/IPS`et `Detecting Windows Attacks with Splunk`ont couvert l'utilisation des données dérivées de l'IDS/IPS. `Intrusion Detection Systems (IDS)`sont nos sentinelles vigilantes, surveillant en permanence le trafic réseau à la recherche de signes d'activité malveillante. Lorsqu'ils remarquent quelque chose qui ne va pas, ils nous alertent. D'un autre côté, `Intrusion Prevention Systems (IPS)`allez plus loin. Non seulement ils détectent, mais ils prennent également des actions prédéfinies pour bloquer ou empêcher ces activités malveillantes.

-   `Traffic flow`les données, souvent provenant d'outils comme `NetFlow`ou `sFlow`, nous offrent une vue plus large du comportement de notre réseau. Même s'il ne nous donne pas les détails essentiels de chaque paquet, il offre un aperçu de haut niveau des modèles de trafic.

-   Enfin, notre fidèle `firewalls`. Il ne s'agit pas seulement de barrières qui bloquent ou autorisent la circulation selon des règles prédéfinies. Les pare-feu modernes sont des bêtes intelligentes. Ils peuvent identifier les applications, les utilisateurs et même détecter et bloquer les menaces. En analysant les journaux du pare-feu, nous pouvons découvrir les tentatives d'exploitation des vulnérabilités, les tentatives d'accès non autorisées et d'autres activités malveillantes.
