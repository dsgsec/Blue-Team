CheatSheet Forensic
--------------------

# Analyse mémoire vive


### Volatility
- Framework OpenSource permettant l'analyse de dump mémoire vive
- Il a besoin de quel type de système provient le dump afin de pouvoir quel symbole, structure de données et algorythme utiliser

#### Commandes
```powershell
volatility.exe -f dump.mem imageinfo
```
**Outil** : volatility

```
Disclaimer : Si le dump provient d'un os exotique ou de Linux, il faut créer son propre profile, mais on peut lister le sprofile existant avec --info
```

**Plusieurs profiles sont disponibles ici** :
```
https://github.com/volatilityfoundation/profiles
```
​
**Créer son profile volatility linux**
```
https://github.com/volatilityfoundation/volatility/wiki/Linux
```




---
### Syntaxe et module

- Le profile identifier il faut l'ajouter aux commandes avec `--profile=$profile` (**mais depuis volatility3, il n’est plus nécessaire de déclarer le profile, volatility se charge de l’identifier tout seul**)
- Les modules viendront compléter la commande  (`-h` permet de lister les modules installer)

#### Commandes pour lister les modules
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 -h
```
#### Commandes pour lister les options d'un modules
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 apihooks -h
```

**Outil** : volatility

---
# Module : pslist

- Permet de lister les processus actifs

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 pslist
```

|Offset|Name|PID|PPID|Start|
|------|----|---|----|-----|
Valeur hexadécimale représentant l’emplacement dans la mémoire du processus en cours d’exécution| Nom du processus en cours d’exécution |Numéro d’identification du processus|Numéro d’identification du processus parent|Heure de démarrage du processus|


**Outil** : volatility

---
# Module : pstree

- Permet de lister les processus actifs avec l'arboresence parents/enfants

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 pstree
```

Name|PID|PPID|Start|
|----|---|----|-----|
|Nom du processus en cours d’exécution |Numéro d’identification du processus|Numéro d’identification du processus parent|Heure de démarrage du processus|


**Outil** : volatility

---
# Module : cmdline

- Permet de lister les processus actifs avec commande associée

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 cmdline
```

**Outil** : volatility

---
# Module : connections, connscan, sockets, sockscan, netscan

- Permet de lister les connexions actives

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 netscan
```

|Offset|Proto|LocalAddr|ForeignAddr|State|PID|Owner|
|------|-----|---------|-----------|-----|---|-----|
|Emplacement dans la mémoire |Protocole réseau utilisé par le processus|Adresse source de la connexion réseau|Adresse destination de la connexion réseau| État de la connexion réseau : établie, fermée ou écoute|ID du processus|Compte associé au processus|


**Outil** : volatility

---
# Module : procdump

- Permet d'extraire un processus sous la forme d'un executable (.exe)

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 procdump -p <pid> -D <DossierDump>
```

**Outil** : volatility

---
# Module : filescan / dumpfiles

- Filescan permet de lister les fichiers chargés / capturé par le dump
- dumpfiles permet de les extraire

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 filescan 
```

#### Commande
```powershell
volatility.exe -f dump.mem --profile=Win2016x64 dumpfiles -D <DossierDump> -Q <Offset>
```

**Outil** : volatility

---
# Artefacts Windows

|mftparser|amcache|hivelist|printkey|shimcache|shellbags|userassist|dumpregistry|timeliner|
|------|-----|---------|-----------|-----|---|-----|-----|-----|
|Liste la table MFT |Liste les infos de amcache|liste les ruches de registres chargées|Liste une clé de registre| Liste les informations de l'artefacts shimcache|Liste les informations de l'artefacts shellbags|Liste les informations de l'artefacts userassist|Dump un registre entier|Créer une timeline|

**Outil** : volatility
