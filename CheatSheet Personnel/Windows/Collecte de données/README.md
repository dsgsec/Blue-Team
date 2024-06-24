CheatSheet Forensic
--------------------

# Collecte de données 

## Collecte live

### Firewall
```
netsh advfirewall firewall show rule name=all,
netsh firewall show allowedprogram
netsh firewall show config
```

### Données système
```powershell
date /T, time /T #Heure et date du système

wmic useraccount get name, sid #Affiche les users

netstat -aon #Connexion en cours

ipconfig /all #Liste les interfaces réseaux

arp -a #Affiche la table ARP

ipconfig /displaydns #Affiche le cache DNS

nbtstat -c, nbtstat -S #Affiche les Connections NetBios

net use #Affiche les partages réseau

reg save HKLM\SYSTEM C:\system_bak.reg #Dump la ruche SYSTEM

systeminfo #Version de l'OS, heure de boot

Autoruns #Programmes lancés au démarage

drivequery #Drivers

get-service #Powershell - liste les services

get-process #Powershell - liste les processus en cours


```


## Analyse offline : Imaging

### Disclaimer
```markdown
Process Conseillé : Extraction physique du support, et le brancher sur un poste d'analyse

1. Identification du disque à cloner
2. Clonage du support en 3 EXEMPLAIRES
3. Vérification que les copies sont identiques à l'original (hash)

Il en conseillé de convertir les dumps DD en .raw
```

### Check des hash
```powershell
get-date -Format "ddd MM/dd/yyyy HH:mm K" ; Get-FileHash dump.img -Algorithm SHA1
```

```bash
date +%d/%m/%Y-%H:%M ; sha1sum dump.img
```

### ⚠️ Fichier essentiels à récupérer
#### Ruches système et Ruches utilisateurs
-  C:\Windows\System32\config || C:\Users\ 
    - NTUSER.DAT
    - SYSTEM
    - SOFTWARE
    - SAM
    - Amcache.hve

### ⚠️ IMPORTANT DE BIEN NOTER LA TIMEZONE
```
Si la timezone diffère de celle des postes d'analyse cela peut induire en erreur, donc important de noter celle du poste infecté afin d'aligner les outils dessus.
```

#### Prefetchs
- C:\Windows\Prefetch

#### Journaux d'événements
- C:\Windows\System32\winevt\Logs

#### $MFT
##### Windows
- Via FTK Imager

##### Linux
```bash
mmls dump.raw
```

- identifier la partition système : `Basic datapartition`
- Noter le offest de "start"

```bash
icat -o 'offest de "start"' dump.raw 0 > MFT
```

### Des-hériter le fichier $MFT de ses attributs
#### msdos
```batch
attrib -S -H $MFT
```
#### Bash
```bash
attrib -S -H $MFT
```

### Créer une timeline depuis $MFT
```powershell
MFTCmd.exe -f $MFT --csv .\Timeline.csv
```

## Outils
- **FTK Imager** : Extraire les fichier de MFT et monter les images de dump
- **Sleuth kit** : outil en cli qui permet d'investiguer sur les images diques
Autopsy : Interface graphique de Sleuth kit
- **TimelineExplorer.exe** : Permet d'avoir une visualisation interactive et dynamique du CSV Timeline
- **PLASO** : Créer une timeline global d'un FileSystem, intégrant les entrées MFT, les artefacts d'utilisation du système, journaux, etc...
- **RegRipper** : étude modulaire d'un artefacts en fonction des registres dont ils dépendent
- **Registry Explorer** : Permet de lire et ouvrivre les registres depuis les dumps de ces derniers
- **Chainsaws** : Outils d'analyse 'firt-reponse' pour identifier les menaces journalisés à travers les logs windows
