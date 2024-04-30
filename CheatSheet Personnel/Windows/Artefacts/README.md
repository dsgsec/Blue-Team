CheatSheet Forensic
--------------------

# Artefacts

## Artefacts Internet

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


## Artefacts Exécution
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


## Artefacts Fichier/Dossiers


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
