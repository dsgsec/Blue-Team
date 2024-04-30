CheatSheet Forensic
--------------------

# Artefacts Internet

**Outil** : HindSight

### Historique de navigation
- Enregistrenement des sites internet visités classés par date
- Stocké pour chaque user local
- Enregistrement du nombre de visite (fréquence)
- Log l'accès au système de fichier


#### Chemins
```
%USERPROFILE%\Local Settings\History\History.IE5
%USERPROFILE%\AppData\Local\Microsoft\Windows\History\
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat
%USERPROFILE%\ApplicationData\Mozilla\Firefox\Profiles\<randomtext>.default\places.sqlite
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles\<randomtext>.default\places.sqlite
%USERPROFILE%\Local Settings\Application Data\Google\Chrome\UserData\Default\History
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\History
```
**Outil** : DB browser for SQLITE

----
### Cache
- Identification des sites web visités
- Fichiers fournis lors de la navigations

#### Chemins
```
%USERPROFILE%\AppData\Local\Microsoft\Windows\TemporaryInternet Files\Content.IE5
%USERPROFILE%\AppData\Local\Microsoft\Windows\TemporaryInternet Files\Content.IE5
%USERPROFILE%\AppData\Local\Microsoft\Windows\INetCache\IE
%USERPROFILE%\AppData\Local\Packages\microsoft.microsoftedge_<APPID>\AC\MicrosoftEdge\CacheFirefox
%USERPROFILE%\Local Settings\ApplicationData\Mozilla\Firefox\Profiles\<randomtext>.default\Cache
%USERPROFILE%\AppData\Local\Mozilla\Firefox\Profiles\<randomtext>.default\Cache
%USERPROFILE%\Local Settings\Application Data\Google\Chrome\UserData\Default\Cache - data_#/af_######
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\Cache\ - data_#/f_######
```
**Outil** : ChromecacheView (nirsoft)

----
### Session restaurée
- Restauration des onglets ouverts après la fermeture du navigateur
- Historique des sutes vus depuis chaque onglet
- Timestamp des fermetures de sessions

#### Chemins
```
%USERPROFILE%/AppData/Local/Microsoft/Internet Explorer/Recovery
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles\<randomtext>.default\sessionstore.js
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\
```

----
### Cookies
- Énumération des domaines consultés
- Timestamp des cookies
- Nombre de visites
- Pages consultés lors de la session
- Liens sortant
- Méthode d'accès (depuis un mail, direct, depuis un autre site)
- Mots clés utilisés pour trouver le site (Hors SSL)
- Timestamp de la création de cookies

#### Chemins
```
%APPDATA%\Roaming\Macromedia\FlashPlayer\#SharedObjects
​

%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Cookies
​

%USERPROFILE%\AppData\Local\Microsoft\Windows\INetCookies
​

%USERPROFILE%\AppData\Local\Packages\microsoft.microsoftedge_<APPID>\AC\MicrosoftEdge\Cookies
​

%USERPROFILE%\Application Data\Mozilla\Firefox\Profiles\<randomtext>.default\cookies.sqlite
​

%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles\<randomtext>.default\cookies.sqlite
​

%USERPROFILE%\Local Settings\Application Data\Google\Chrome\UserData\Default\Local Storage\
​

%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\Local Storage\
​
```

**Outil** : ChromeCookieView (Nirsoft)

----
### Pièce-Jointe mail
- .ost /.pst
- .mbox
- Appdata\local (contient les PJ ouvertes)

#### Chemins
```
%USERPROFILE%\Local Settings\ApplicationData\Microsoft\Outlook
​

%USERPROFILE%\AppData\Local\Microsoft\Outlook
```
----
### Skype / Teams
- Date et heure des communications
- Fichiers envoyés

#### Chemins
```
C:\Documents and Settings\\Application\Skype\
​

%USERPROFILE%\AppData\Roaming\Skype\

```
----
### Navigateurs
- Fichier ouverts via un navigateur
- Date et heure de consultation
- N'implique pas forcément un téléchargement

#### Chemins
```
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\IEDownloadHistory\index.dat
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat
%USERPROFILE%\AppData\Roaming\Mozilla\Firefox\Profiles\.default\downloads.sqlite
%USERPROFILE%\AppData\Roaming\Mozilla\ Firefox\Profiles\.default\places.sqlite
%USERPROFILE%\AppData\Local\Google\Chrome\User Data\Default\History#
```
**Outil** : DB browser for SQLITE

----
### Téléchargement
```
%USERPROFILE%\Application Data\Mozilla\ Firefox\Profiles\.default\downloads.sqlite
%USERPROFILE%\AppData\Roaming\Mozilla\ Firefox\Profiles\.default\downloads.sqlite
%USERPROFILE%\AppData\Roaming\Microsoft\Windows\IEDownloadHistory\
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat
```
**Outil** : ChromeCookieView (Nirsoft)

----
### Zone identifier
- ZoneID 2 = Trusted
- ZoneID 3 = Internet
- ZoneID 4 = Untrusted

-----------------
# Artefacts Exécution
- 120 derniers programmes récemment exécutés
```
C:\Windows\Prefetch
```

**Outil** : PECmd/WinPrefetchView(Nirsoft)

----
### UserAssist
- Exécution de programme via GUI
```
NTUSER.DAT\Software\Microsoft\Windows\Currentversion\Explorer\UserAssist\{GUID}\Count
```
**Outil** : UserAssistWindows7 - Registry Explorer - Regedit

----
### Timeline Windows10
- Windows 10 enregistre toutes les applications qui ont été utilisées récemment et propose à l’utilisateur une timeline accessible avec les touches suivantes > WIN+TAB.
```
C:\Users\<username>\AppData\Local\ConnectedDevicesPlatform\<random>\ActivitiesCache.db
```
**Outil** : WxTCmd à chaud et à froid

----
### Shimcache
- Windows Application Compatibility Database est utilisée par Windows pour vérifier si une application doit démarrer en mode de compatibilité.
```
SYSTEM\CurrentControlSet\Control\SessionManager\AppCompatibility
SYSTEM\CurrentControlSet\Control\Session Manager\AppCompatCache
```
**Outil** : Registry Explorer

----
### Amcache.hve
- ProgramDataUpdater (une tâche associée au service ‘Application Experience’) utilise la ruche Amcache.hve pour stocker de la donnée lors de la création de process.
```
C:\Windows\AppCompat\Programs\Amcache.hve
```
**Outil** : AmcacheParser

----
### SRUM
- Fait son apparition à partir de Windows 8.1 Enregistre 30 à 60 jours d’historique d’activité de la machine.
```
SOFTWARE\Microsoft\WindowsNT\CurrentVersion\SRUM\Extensions{d10ca2fe-6fcf4f6d-848e-b2e99266fa89}
C:\Windows\System32\SRU\SRUDB.DAT
```
**Outil** : Srum-Dump-Master (Attention, ruche + fichier sru)


----
### RunMRU
- Cet artefact permet de visualiser les éléments exécutés en "exécution de commande"
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RunMRU
```
**Outil** : Registry Explorer


# Artefacts Fichier/Dossiers


### Open/Save MRU
- Pour faire simple, chaque fichier ayant été ouvert ou enregistré (par l’intermédiaire d’une boîte de dialogue Windows) est historisé en Bases de Registres. Les fichiers enregistrée par le biais d’un navigateur web actionnent cette boîte de dialogue
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSaveMRU
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\OpenSavePIDlMRU
```
**Outil** : Registry explorer

----
### Last Visited MRU
- Traque les exécutables utilisés par une application pour ouvrir un document (présent dans OpenSaveMRU). De plus, chaque valeur associe le répertoire ou le fichier ayant été ouvert par cette application.
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\ LastVisitedMRU
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\ComDlg32\ LastVisitedPidlMRU
```
**Outil** : Srum-Dump-Master (Attention, ruche + fichier sru)


----
### ID/EDGE (file://)
Un des comportements méconnus de l’historique IE est qu’il n’enregistre pas que les sites internet consultés mais bel et bien des accès à des fichiers du disque (ou partages réseau). Il va de soi que cet historique apporte énormément d’information en terme de temporalité.
```
%USERPROFILE%\Local Settings\History\ History.IE5
%USERPROFILE%\AppData\Local\Microsoft\Windows\History\History.IE5
%USERPROFILE%\AppData\Local\Microsoft\Windows\WebCache\WebCacheV*.dat%
```

----
### Shell bags
- Les shell bags permettent de déterminer les accès à des répertoires sur la machine locale, le réseau ou des périphériques externes. Il est possible de retrouver des répertoires supprimés ou modifiés et la dernière date à laquelle l’utilisateur y a eu accès.

#### Explorer
```
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\Bags
USRCLASS.DAT\Local Settings\Software\Microsoft\Windows\Shell\BagMRU
```

#### Desktop
```
NTUSER.DAT\Software\Microsoft\Windows\Shell\BagMRU
NTUSER.DAT\Software\Microsoft\Windows\Shell\Bags
```
**Outil** : ShellBagsView (Besoin de tous les fichiers de transaction)

----
### Fichier récents
- Cette clé de registre va suivre les derniers fichiers et répertoires ouverts. Ces fichiers sont accessibles depuis “mes fichiers récents” dans la barre de navigation latérale.
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```
**Outil** : Registry Explorer

----
### Documents offices récents
- Les programmes Office suivent l’activité des fichiers qu’ils ont ouvert et possèdent leur propre catégorie de fichiers récents.

```
NTUSER.DAT\Software\Microsoft\Office\VERSION
NTUSER.DAT\Software\Microsoft\Office\VERSION\UserMRU\LiveID_####\FileMRU
NTUSER.DAT\Software\Microsoft\Office\VERSION\Word\FileMRU -> Word/Excel/Powerpoint..
```
----
### Raccourcis .lnk
- Les raccourcis sont automatiquement créés par l’OS Windows, suite à divers événements. L’ouverture d’un fichier ou l’accès à un répertoire génère des raccourcis.

```
%USERPROFILE%\Recent
​

%USERPROFILE%\AppData\Roaming\Microsoft\Windows\Recent\
​

%USERPROFILE%\AppData\Roaming\Microsoft\Office\Recent\
```

**Outil** : LECmd

----
### Fichier récents
- Cette clé de registre va suivre les derniers fichiers et répertoires ouverts. Ces fichiers sont accessibles depuis “mes fichiers récents” dans la barre de navigation latérale.
```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\RecentDocs
```
**Outil** : Registry Explorer

# Artefacts réseaux


### TimeZone
- La notion de tranche horaire est très importante car tous les timestamps collectés s’appuient sur cette clé de registre (fichiers de logs). Permet d’établir la timezone du système d’exploitation
```
SYSTEM\{CurrentControlSet}\Control\TimeZoneInformation
```
**Outil** : Registry explorer

----
### Configuration IP
```
SYSTEM\{CurrentControlSet}\Services\tcpip\parameters\Interfaces\
```
**Outil** : Registry explorer

-----
### Activité / Historique réseau
- Il est possible d’identifier les réseaux auxquels l’ordinateur s’est connecté. Les réseaux peuvent être filaires ou sans fil. Les informations pouvant être obtenues sont le nom du domaine ainsi que des SSIDs et des adresses MAC (passerelles). Identification des intranets/réseaux. Découverte du nom de domaine et date de la dernière connexion (timestamp d’écriture dans le registre).

```
HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Unmanage
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Signatures\Managed
SOFTWARE\Microsoft\Windows NT\CurrentVersion\NetworkList\Nla\Cache
```
**Outil** : Registry explorer

-----
### Journaux d'événements WLAN
- Permet de déterminer avec quel(s) wlan le client s’est associé et identifier les caractéristiques de ce réseau. Il est également possible de retrouver l’historique des connections et les SSIDs et BSSIDs (MAC address).

```
%SYSTEMROOT%\System32\winevt\Logs\Microsoft-Windows-WLAN-AutoConfigOperational.evtx
%SYSTEMROOT%\System32\winevt\Logs\System.evtx
```

#### Orational.evtx
- 11000 : Association à un réseau sans fil démarrée
- 8001 : Connexion à un réseau sans fil avec succès
- 8002 : Connexion à un réseau sans fil avec echec
- 8003 : Déonnexion d'un réseau sans fil

#### System.evtx
- 6100 : Diagnostic système

**Outil** : EvtxExplorer


# Artefacts utilisateurs


### Dernière authentification
```
SAM\Domains\Account\Users (Non visible avec regedit)
```
**Outil** : Registry explorer


-----
### Événements liés au système
- Le but est d’analyser les services suspects qui démarrent au boot de la machine. Il convient d’inspecter les services démarrés ou stoppés aux alentours de la date de compromission. Énormément utilisent les services pour générer de la persistance. Il est également possible que le malware, de manière volontaire ou non, fasse crasher un service (injection process)

```
%SYSTEMROOT%\System32\winevt\Logs\System.evtx
```

#### System.evtx
- 7034 : Crash d'un service
- 7036 : Démmarage / Arrêt d'un service
- 7040 : Type de démmarage modifié
- 4697 : Service installé sur un système
- 7045 : Service installé sur un système (Win2008R2+)

**Outil** : EvtxExplorer


-----
### Événements liés aux tâches plannifiées

```
OS --> C:\Windows\System32\Tasks ou C:\Windows\Tasks
Registre --> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tree
Registre --> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Schedule\TaskCache\Tasks
Logs --> C:\Windows\System32\winevt\Logs\Microsoft-Windows-TaskScheduler
```

**Outil** : EvtxExplorer


-----
### Journaux d'événements Logon : succès / échecs
- Il est possible de déterminer quel compte s’est connecté à l’OS. Il convient de suivre l’activité des comptes supposés compromis.

```
%SYSTEMROOT%\System32\winevt\Logs\Security.evtx
```

#### Security.evtx
- 4624 : Auth success
- 4625 : Auth failed
- 4634 & 4647 : Logout
- 4648 : Connexion avec : `runas`
- 4672 : Connexion avec un compte à privileges élevés
- 4720 : Compte créé

#### RDP
- 4778 : Session Connected
- 4779 : Session disconnected

#### NTLM
- 4776 : Success

#### Kerberos
- 4768 : Obtention TGT
- 4769 : Demande d'un TGS
- 4771 : Echec de pré-auth

```
L’Event ID 4624 permet d’obtenir des informations spécifiques concernant le type de logon, par rapport à la nature du compte qui se connecte à l’OS. Il sera possible d’extraire la.
```

2. Logon via session interactive
3. Logon réseau
4. Logon via Batch
5. Windows Service Logon
7. Credentials pour débloquer l’écran de veille
8. Logon réseau avec envoi de credentials (en clair)
9. Credentials différents utilisés par l’utilisateur actuel
10. Logon RDP
11. Logon avec credentials en cache


**Outil** : EvtxExplorer

# Artefacts USB

### Identification de la clé
- Les périphériques n’ayant pas un numéro de série unique ont un “&” en second caractère de numéro de série. Les autres numéros de série sont uniques, ce qui veut dire que le constructeur respecte les normes internationales.

```
SYSTEM\CurrentControlSet\Enum\USBSTOR
SYSTEM\CurrentControlSet\Enum\USB
```

Découvrir une lettre associée à un Drive (pluggé à la machine hôte)

Cette technique fonctionne uniquement sur le dernier périphérique associé sur une lettre. Il n’y a pas d’historique ni de liste de clés associées à une lettre.

```
SYSTEM\CurrentControlSet\Enum\USBSTOR
​

SYSTEM\MountedDevices
​

SOFTWARE\Microsoft\Windows Portable Devices\Devices
​
```

Les fichiers .lnk (raccourcis) contiennent le numéro de série du volume ainsi que son nom. La clé de registre RecentDocs contient le nom du volume lorsque la clé est explorée depuis Explorer. Il ne s’agit pas du numéro de série de la clé qui est elle codée en dur dans le firmware.

```
SOFTWARE\Microsoft\WindowsNT\CurrentVersion\EMDMgmt
```

----
### Première utilisation / Dernière utilisation
- Les locations de registre suivantes permettent d’obtenir la liste des périphériques USB branchés au système ainsi que les timestamp, modèle et éditeur

```
SYSTEM\CurrentControlSet\Enum\USBSTOR
SYSTEM\CurrentControlSet\Enum\USB
```

- 0064 = Première installation (Win7-10)
- 0066 = Dernier branchement (Win8-10)
- 0067 = Dernier retrait (Win8-10)

Découvrir une lettre associée à un Drive (pluggé à la machine hôte)

Cette technique fonctionne uniquement sur le dernier périphérique associé sur une lettre. Il n’y a pas d’historique ni de liste de clés associées à une lettre.

```
SYSTEM\CurrentControlSet\Enum\USBSTOR
​

SYSTEM\MountedDevices
​

SOFTWARE\Microsoft\Windows Portable Devices\Devices
​
```

**Outil** : USBDeview

----
### Événement plug and play
- Lorsqu’un driver PnP tente de s’installer sur le système, un événement (ID 20001) est créé et fourni un statut relatif à cet événement. Il est important de noter que cet événement n’est pas uniquement lié à l’USB, mais aussi le firewire, et autres connectiques.


#### System.evtx

**Outil** : EvtxExplorer



----
### Logs supplémentaires
- Il est possible de constater des activités liées au branchement d’un périphérique via USB dans les fichiers suivants (Filtrer sur hardware initiated) :

```
C:\Windows\setupapi.log
C:\Windows\inf\setupapi.dev.log
```

- Il suffit de faire une recherche sur le numéro de série pour découvrir les timestamps associés. Ils sont exprimés dans la timezone locale.

```
\CurrentControlSet\Enum\USBSTOR\Ven_Prod_Version\USBSerial#\Properties\{83da6326...}\####
```

# Artefacts Fichiers supprimés

### Wordwheelquery
- Il s’agit des mots clés saisis par l’utilisateur dans la bar de recherche ou le menu démarrer depuis Windows 7. Les mots clés sont au format unicode et listé de manière chronologique dans la sous-clé MRUList.

```
NTUSER.DAT\Software\Microsoft\Windows\CurrentVersion\Explorer\WordWheelQuery
```

**Outil** : RegistryExplorer

----
### Thumb.db
- Il s’agit d’un fichier caché dans un répertoire. Les miniatures des images et icônes de fichiers sont enregistrées dans le fichier thumbs.db. Ce fichier est créé automatiquement lorsque le partage réseaux homegroup est activé ou lorsqu’un répertoire est accédé via un chemin UNC.

----
### Thumbcache.db
- Les miniatures d’images, de documents et répertoires persistent dans une base de données appelée thumbcache.db. Chaque utilisateur a sa propre base. Les miniatures enregistrées sont récupérables en plusieurs formats : small, medium, large, extra-large. Elles sont enregistrée dans l’une de ces catégories en fonction du type d’affichage de l’utilisateur.

```
%USERPROFILE%\AppData\Local\Microsoft\Windows\Explorer
```

**Outil** : Thumbcache_viewer

----
### Corbeille
- La corbeille contient tout d’abord un répertoire racine contenant des sous-répertoires au noms des SID présents sur la machine. Chaque SID peut être mis en relation avec un utilisateur via le registre ou la commande Get-WmiObject win32_useraccount | Select name, sid

​

Les fichiers commençant par $I files contiennent le chemin d’origine du fichier et la date et heure de suppression

Les fichiers commençant par $R sont les fichiers de sauvegarde des fichiers supprimés

```
C:\$Recycle.bin
```

# Artefacts Active Directory

**Import-module ActiveDirectory** : Permet l’import de l’ensemble des Cmdlet nécessaires à l’investigation numérique et la gestion de l’annuaire Active Directory

- Extract des GPO
- Extractions des comptes à privileges
- Stratégie de sécurtié & Locked Account
