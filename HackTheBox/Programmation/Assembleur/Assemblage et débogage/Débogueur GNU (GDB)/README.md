Débogueur GNU (GDB)
===================

* * * * *

Le débogage est une compétence importante à acquérir pour les développeurs et les pentesters. Le débogage est un terme utilisé pour rechercher et supprimer les problèmes (c'est-à-dire les bogues) de notre code, d'où le nom de débogage. Lorsque nous développons un programme, nous rencontrons très fréquemment des bugs dans notre code. Il n'est pas efficace de continuer à modifier notre code jusqu'à ce qu'il fasse ce que nous attendons de lui. Au lieu de cela, nous effectuons le débogage en définissant des points d'arrêt et en voyant comment notre programme agit sur chacun d'eux et comment nos entrées changent entre eux, ce qui devrait nous donner une idée claire de la cause du `bug`.

Les programmes écrits dans des langages de haut niveau peuvent définir des points d'arrêt sur des lignes spécifiques et exécuter le programme via un débogueur pour surveiller leur comportement. Avec Assembly, nous traitons du code machine représenté sous forme d'instructions d'assemblage, nos points d'arrêt sont donc définis dans l'emplacement mémoire dans lequel notre code machine est chargé, comme nous le verrons.

Pour déboguer nos binaires, nous utiliserons un débogueur bien connu pour les programmes Linux appelé [GNU Debugger](https://www.gnu.org/software/gdb/) ( `GDB`). Il existe d'autres débogueurs similaires pour Linux, comme [Radare](https://www.radare.org/r/) et [Hopper](https://www.hopperapp.com/) , et pour Windows, comme [Immunity Debugger](https://www.immunityinc.com/products/debugger/) et [WinGDB](http://wingdb.com/) . Il existe également de puissants débogueurs disponibles pour de nombreuses plates-formes, comme [IDA Pro](https://www.hex-rays.com/products/ida/) et [EDB](https://github.com/eteran/edb-debugger) . Dans ce module, nous utiliserons GDB. C'est le plus fiable pour les binaires Linux puisqu'il est construit et maintenu directement par GNU, ce qui lui confère une excellente intégration avec le système Linux et ses composants.

* * * * *

Installation
------------

GDB est installé dans de nombreuses distributions Linux, et il est également installé par défaut dans Parrot OS et PwnBox. S'il n'est pas installé sur votre VM, vous pouvez `apt`l'installer avec les commandes suivantes :

```
dsgsec@htb[/htb]$ sudo apt-get update
dsgsec@htb[/htb]$ sudo apt-get install gdb

```

L'une des fonctionnalités intéressantes de `GDB`est sa prise en charge des plugins tiers. Un excellent plugin bien entretenu et doté d'une bonne documentation est [GEF](https://github.com/hugsy/gef) . GEF est un plugin GDB gratuit et open source conçu précisément pour l'ingénierie inverse et l'exploitation binaire. Ce fait en fait un excellent outil pour apprendre.

Pour ajouter GEF à GDB, nous pouvons utiliser les commandes suivantes :

```
dsgsec@htb[/htb]$ wget -O ~/.gdbinit-gef.py -q https://gef.blah.cat/py
dsgsec@htb[/htb]$ echo source ~/.gdbinit-gef.py >> ~/.gdbinit

```

* * * * *

Commencer
---------

Maintenant que les deux outils sont installés, nous pouvons exécuter gdb pour déboguer notre `HelloWorld`binaire à l'aide des commandes suivantes, et GEF sera chargé automatiquement :

```
dsgsec@htb[/htb]$ gdb -q ./helloWorld
...SNIP...
gef➤

```

Comme nous pouvons le voir `gef➤`, GEF est chargé lorsque GDB est exécuté. Si jamais vous rencontrez des problèmes avec `GEF`, vous pouvez consulter la [documentation du FEM](https://hugsy.github.io/gef/) et vous trouverez probablement une solution.

À l'avenir, nous assemblerons et lierons fréquemment notre code assembleur, puis l'exécuterons avec `gdb`. Pour le faire rapidement, nous pouvons utiliser le `assembler.sh`script que nous avons écrit dans la section précédente avec le `-g`flag. Il assemblera et liera le code, puis l'exécutera avec `gdb`, comme suit :

```
dsgsec@htb[/htb]$ ./assembler.sh helloWorld.s -g
...SNIP...
gef➤

```

* * * * *

Info
----

Une fois `GDB`démarré, nous pouvons utiliser la `info`commande pour afficher des informations générales sur le programme, comme ses fonctions ou ses variables.

Astuce : Si nous voulons comprendre comment une commande s'exécute dans `GDB`, nous pouvons utiliser la `help CMD`commande pour obtenir sa documentation. Par exemple, nous pouvons essayer d'exécuter`help info`

#### Les fonctions

Pour commencer, nous utiliserons la `info`commande pour vérifier lesquels `functions`sont définis dans le binaire :

  Les fonctions

```
gef➤  info functions

All defined functions:

Non-debugging symbols:
0x0000000000401000  _start

```

Comme nous pouvons le constater, nous avons trouvé notre `_start`fonction principale.

#### Variables

Nous pouvons également utiliser la `info variables`commande pour afficher toutes les variables disponibles dans le programme :

  Variables

```
gef➤  info variables

All defined variables:

Non-debugging symbols:
0x0000000000402000  message
0x0000000000402012  __bss_start
0x0000000000402012  _edata
0x0000000000402018  _end

```

Comme nous pouvons le voir, nous trouvons le `message`, ainsi que quelques autres variables par défaut qui définissent les segments de mémoire. Nous pouvons faire beaucoup de choses avec les fonctions, mais nous nous concentrerons sur deux points principaux : le désassemblage et les points d'arrêt.

* * * * *

Démonter
--------

Pour afficher les instructions dans une fonction spécifique, nous pouvons utiliser la commande `disassemble`ou `disas`avec le nom de la fonction, comme suit :

  Variables

```
gef➤  disas _start

Dump of assembler code for function _start:
   0x0000000000401000 <+0>:	mov    eax,0x1
   0x0000000000401005 <+5>:	mov    edi,0x1
   0x000000000040100a <+10>:	movabs rsi,0x402000
   0x0000000000401014 <+20>:	mov    edx,0x12
   0x0000000000401019 <+25>:	syscall
   0x000000000040101b <+27>:	mov    eax,0x3c
   0x0000000000401020 <+32>:	mov    edi,0x0
   0x0000000000401025 <+37>:	syscall
End of assembler dump.

```

Comme nous pouvons le voir, le résultat que nous avons obtenu ressemble beaucoup à notre code d'assemblage et au résultat de désassemblage que nous avons obtenu `objdump`dans la section précédente. Nous devons nous concentrer sur l'essentiel de ce démontage : les adresses mémoire de chaque instruction et opérandes (c'est-à-dire les arguments).

`Having the memory address is critical for examining the variables/operands and setting breakpoints for a certain instruction.`

Vous remarquerez peut-être lors du débogage que certaines adresses mémoire sont sous la forme de `0x00000000004xxxxx`, plutôt que leur adresse brute en mémoire `0xffffffffaa8a25ff`. Cela est dû `$rip-relative addressing`aux exécutables indépendants de la position `PIE`, dans lesquels les adresses mémoire sont utilisées par rapport à leur distance par rapport au pointeur d'instruction `$rip`dans la propre RAM virtuelle du programme, plutôt que d'utiliser des adresses mémoire brutes. Cette fonctionnalité peut être désactivée pour réduire le risque d'exploitation binaire.
