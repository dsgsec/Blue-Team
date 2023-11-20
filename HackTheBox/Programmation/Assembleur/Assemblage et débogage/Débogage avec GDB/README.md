Débogage avec GDB
=================

* * * * *

Maintenant que nous avons les informations générales sur notre programme, nous allons commencer à l'exécuter et à le déboguer. Le débogage comprend principalement quatre étapes :

| Étape | Description |
| --- | --- |
| `Break` | Définition de points d'arrêt à différents points d'intérêt |
| `Examine` | Exécuter le programme et examiner l'état du programme à ces points |
| `Step` | Parcourir le programme pour examiner comment il agit avec chaque instruction et avec les entrées de l'utilisateur |
| `Modify` | Modifier les valeurs dans des registres ou des adresses spécifiques à des points d'arrêt spécifiques, pour étudier comment cela affecterait l'exécution |

Nous passerons en revue ces points dans cette section pour apprendre les bases du débogage d'un programme avec GDB.

* * * * *

Casser
------

La première étape du débogage consiste `breakpoints`à arrêter l'exécution à un emplacement spécifique ou lorsqu'une condition particulière est remplie. Cela nous aide à examiner l'état du programme et la valeur des registres à ce stade. `Breakpoints`nous permettent également d'arrêter l'exécution du programme à ce stade afin que nous puissions entrer dans chaque instruction et examiner comment elle modifie le programme et les valeurs.

Nous pouvons définir un point d'arrêt à une adresse spécifique ou pour une fonction particulière. Pour définir un point d'arrêt, nous pouvons utiliser la commande `break`ou `b`avec l'adresse ou le nom de la fonction à laquelle nous voulons arrêter. Par exemple, pour suivre toutes les instructions exécutées par notre programme, arrêtons-nous sur la `_start`fonction, comme suit :

```
gef➤  b _start

Breakpoint 1 at 0x401000

```

Maintenant, pour démarrer notre programme, nous pouvons utiliser la commande `run`ou :`r`


gdb
```
gef➤   b _start Point d'arrêt 1 à 0x401000
gef➤r   _ Programme de démarrage : ./helloWorld
 Point d'arrêt 1, 0x0000000000401000 dans _start () [ Légende:Registre modifié | Codes | Tas | Pile | Chaîne ] ─────────────────────────────────────── ───────── ───────────────────────────────────── registres ── ──
$rax   : 0x0 $rbx   : 0x0 $rcx   : 0x0 $rdx   : 0x0 $rsp   : 0x00007fffffffe310 → 0x0000000000000001 $rbp   : 0x0 $   rsi :
 0x0 $rdi   : 0x0 $rip   : 0x0000000000401000 → <_start+0> mov eax, 0x1 ...COUPER...
──────────────────────────────────────── ────────── ────────────────────────────────── ... ────
0x00007fffffffe310 │+0x0000 : 0x0000000000000001 ← $rsp
0x00007fffffffe318 │+0x0008 : 0x00007fffffffe5a0 → "./helloWorld" ...COUPER...
──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
   0x400ffa ajoute BYTE PTR [rax], al
   0x400ffc ajoute BYTE PTR [rax], al
   0x400ffe ajoute BYTE PTR [rax], al
 → 0x401000 <_start+0> mov eax, 0x1 0x401005 <_start+5> mov edi, 0x1 0x40100a <_start+10> movabs rsi, 0x402000 0x401014 <_start+20> mouvement edx, 0x12 0x401019 <_start+25> appel système 0x40101b <_start+27> mouvement eax, 0x3c
──────────────────────────────────────── ────────── ───────────────────────────────────── fils ── ── [#0 ] Identifiant 1, Nom : "helloWorld",arrêté  0x401000 dans_start (), raison :POINT D'ARRÊT
────────────────────────────────────── ─────────── ──────────────────────────────────────── trace ──── [#0 ] 0x401000 → _start () ────────────────────────────────── ──────── ──────────────────────────────────────── ────────── ────────
```

Si nous voulons définir un point d'arrêt à une certaine adresse, comme `_start+10`, nous pouvons soit `b *_start+10`soit`b *0x40100a` :

```
gef➤  b *0x40100a
Breakpoint 1 at 0x40100a

```

Le `*`dit `GDB`d'interrompre l'instruction stockée dans `0x40100a`.

Remarque : Une fois le programme exécuté, si nous définissons un autre point d'arrêt, comme `b *0x401005`, afin de continuer jusqu'à ce point d'arrêt, nous devons utiliser la commande `continue`ou . `c`Si nous utilisons `run`ou `r`à nouveau, le programme sera exécuté depuis le début. Cela peut être utile pour sauter des boucles, comme nous le verrons plus loin dans le module.

Si nous voulons voir quels points d'arrêt nous avons à tout moment de l'exécution, nous pouvons utiliser la `info breakpoint`commande. Nous pouvons également `disable`, `enable`ou `delete`n'importe quel point d'arrêt. De plus, GDB prend également en charge la définition de pauses conditionnelles qui arrêtent l'exécution lorsqu'une condition spécifique est remplie.

* * * * *

Examiner
--------

La prochaine étape du débogage concerne `examining`les valeurs dans les registres et les adresses. Comme nous pouvons le voir dans la sortie précédente du terminal, `GEF`nous avons automatiquement fourni de nombreuses informations utiles lorsque nous avons atteint notre point d'arrêt. C'est l'un des avantages du `GEF`plugin, car il automatise de nombreuses étapes que nous effectuons habituellement à chaque point d'arrêt, comme l'examen des registres, de la pile et des instructions d'assemblage actuelles.

Pour examiner manuellement l'une des adresses ou des registres ou en examiner un autre, nous pouvons utiliser la `x`commande au format `x/FMT ADDRESS`, comme `help x`nous le dirait. Il `ADDRESS`s'agit de l'adresse ou du registre que nous souhaitons examiner, tandis `FMT`que le format d'examen. Le format d'examen `FMT`peut comporter trois parties :

| Argument | Description | Exemple |
| --- | --- | --- |
| `Count` | Le nombre de fois que nous voulons répéter l'examen | `2`, `3`,`10` |
| `Format` | Le format dans lequel nous voulons que le résultat soit représenté | `x(hex)`, `s(string)`,`i(instruction)` |
| `Size` | La taille de la mémoire que nous voulons examiner | `b(byte)`, `h(halfword)`, `w(word)`,`g(giant, 8 bytes)` |

#### Instructions

Par exemple, si nous voulons examiner les quatre instructions suivantes en ligne, nous devrons examiner le `$rip`registre (qui contient l'adresse de l'instruction suivante) et utiliser `4`pour le `count`, `i`pour le `format`et `g`pour le `size`(pour 8 octets ou 64 bits). Ainsi, la commande d'examen finale serait `x/4ig $rip`la suivante :

  Instructions

```
gef➤  x/4ig $rip

=> 0x401000 <_start>:	mov    eax,0x1
   0x401005 <_start+5>:	mov    edi,0x1
   0x40100a <_start+10>:	movabs rsi,0x402000
   0x401014 <_start+20>:	mov    edx,0x12

```

Nous voyons que nous obtenons les quatre instructions suivantes comme prévu. Cela peut nous aider, au fur et à mesure que nous parcourons un programme, à examiner certains domaines et les instructions qu'ils peuvent contenir.

#### Cordes

Nous pouvons également examiner une variable stockée à une adresse mémoire spécifique. Nous savons que notre `message`variable est stockée dans la `.data`section sur l'adresse `0x402000`de notre démontage précédent. Nous voyons également la commande à venir `movabs rsi, 0x402000`, nous souhaiterons donc peut-être examiner ce qui est déplacé `0x402000`.

Dans ce cas, nous ne mettrons rien pour le `Count`, car nous ne voulons qu'une seule adresse (1 est la valeur par défaut), et nous l'utiliserons `s`comme format pour l'obtenir sous forme de chaîne plutôt qu'en hexadécimal :

  Cordes

```
gef➤  x/s 0x402000

0x402000:	"Hello HTB Academy!"

```

Comme nous pouvons le voir, nous pouvons voir la chaîne à cette adresse représentée sous forme de texte plutôt que de caractères hexadécimaux.

Remarque : si nous ne spécifions pas le `Size`ou `Format`, la valeur par défaut sera la dernière que nous avons utilisée.

#### Adresses

Le format d'examen le plus courant est le format hexadécimal `x`. Nous devons souvent examiner des adresses et des registres contenant des données hexadécimales, telles que des adresses mémoire, des instructions ou des données binaires. Examinons la même instruction précédente, mais sous `hex`forme, pour voir à quoi elle ressemble :

  Adresses

```
gef➤  x/wx 0x401000

0x401000 <_start>:	0x000001b8

```

Nous voyons à la place de `mov eax,0x1`, nous obtenons `0x000001b8`, qui est la représentation hexadécimale du `mov eax,0x1`code machine au format petit-boutiste.

-   Ceci se lit comme suit : `b8 01 00 00`.

Essayez de répéter les commandes que nous avons utilisées pour examiner les chaînes en utilisant `x`pour les examiner en hexadécimal. Nous devrions voir le même texte mais au format hexadécimal. Nous pouvons également utiliser `GEF`des fonctionnalités pour examiner certaines adresses. Par exemple, à tout moment, nous pouvons utiliser la `registers`commande pour imprimer la valeur actuelle de tous les registres :

  gdb
  
```
gef➤  enregistre $rax   : 0x0 $rbx   : 0x0 $rcx   : 0x0 $rdx    : 0x0 $rsp   : 0x00007fffffffe310 → 0x0000000000000001 $rbp   : 0x0 $    rsi : 0x0 $rdi   : 0x0 $rip   : 0x000000000040 1000   → <_start+0> déplacement eax, 0x1 ...COUPER...
```
* * * * *

Étape
-----

La troisième étape du débogage consiste `stepping`à parcourir le programme, une instruction ou une ligne de code à la fois. Comme nous pouvons le constater, nous en sommes actuellement à la toute première instruction de notre `helloWorld`programme :

  gdb
```
──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
   0x400ffe ajoute BYTE PTR [rax], al
 → 0x401000 <_start+0> mov eax, 0x1 0x401005 <_start+5> mov edi, 0x1
```

```
Remarque : l'instruction affichée avec le `->`symbole correspond à l'endroit où nous en sommes et elle n'a pas encore été traitée.
```

Pour vous déplacer dans le programme, nous pouvons utiliser trois commandes différentes : `stepi`et `step`.

#### Instructions étape par étape

La commande `stepi`ou `si`parcourra les instructions d'assemblage une par une, ce qui constitue le plus petit niveau d'étapes possible lors du débogage. Utilisons la `s`commande pour voir comment passer à l'instruction suivante :

  gdb
```
──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
g ef➤   si 0x0000000000401005 dans _start ()
 0x400fff ajouter BYTE PTR [rax+0x1], bh
 → 0x401005 <_start+5> mov edi, 0x1 0x40100a <_start+10> movabs rsi, 0x402000 0x401014 <_start+20> mouvement edx, 0x12 0x401019 <_start+25> appel système
──────────────────────────────────────── ────────── ───────────────────────────────────── fils ── ── [#0 ] Identifiant 1, Nom : "helloWorld",arrêté  0x401005 dans_start (), raison :UNE SEULE ÉTAPE
```

Comme nous pouvons le voir, nous avons fait exactement un pas et nous nous sommes arrêtés à nouveau à l' `mov edi, 0x1`instruction.

#### Nombre de pas

De la même manière pour examiner, nous pouvons répéter la `si`commande en ajoutant un nombre après. Par exemple, si nous voulons avancer de 3 pas pour atteindre l' `syscall`instruction, nous pouvons le faire comme suit :

  gdb
```
gef➤   si 3 0x0000000000401019 dans _start ()
──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
   0x401004 <_start+4> ajouter BYTE PTR [rdi+0x1], bh
   0x40100a <_start+10> movabs rsi, 0x402000
   0x401014 <_start+20> mov edx, 0x12
 → 0x401019 <_start+25> syscall 0x40101b <_start+27> mouvement eax, 0x3c 0x401020 <_start+32> mov edi, 0x0 0x401025 <_start+37> appel système
──────────────────────────────────────── ────────── ───────────────────────────────────── fils ── ── [#0 ] Identifiant 1, Nom : "helloWorld",arrêté  0x401019 dans_start (), raison :UNE SEULE ÉTAPE
```

Comme nous pouvons le constater, nous nous sommes arrêtés à l' `syscall`instruction comme prévu.

Astuce : Vous pouvez appuyer sur le `return`/ `enter`vide pour répéter la dernière commande. Essayez de le frapper à ce stade, et vous devriez faire 3 autres pas et interrompre l'autre `syscall`instruction.

#### Étape

La commande `step`ou `s`, en revanche, continuera jusqu'à ce que la ligne de code suivante soit atteinte ou jusqu'à ce qu'elle quitte la fonction en cours. Si nous exécutons un code assembleur, il se brisera lorsque nous quitterons la fonction actuelle `_start`.

S'il y a un appel à une autre fonction dans cette fonction, il s'interrompra au début de cette fonction. Sinon, cela se brisera après la sortie de cette fonction après la fin du programme. Essayons d'utiliser `s`, et voyons ce qui se passe :

  Étape

```
gef➤  step

Single stepping until exit from function _start,
which has no line number information.
Hello HTB Academy!
[Inferior 1 (process 14732) exited normally]

```

Nous voyons que l'exécution s'est poursuivie jusqu'à ce que nous arrivions à la sortie de la `_start`fonction, nous avons donc atteint la fin du programme et `exited normally`sans aucune erreur. Nous voyons également que `GDB`la sortie du programme est `Hello HTB Academy!`également imprimée.

Remarque : Il existe également la commande `next`or `n`, qui continuera également jusqu'à la ligne suivante, mais ignorera toutes les fonctions appelées dans la même ligne de code, au lieu de les interrompre comme `step`. Il y a aussi le `nexti`or `ni`, qui est similaire à `si`, mais ignore les appels de fonctions, comme nous le verrons plus tard dans le module.

* * * * *

Modifier
--------

La dernière étape du débogage concerne `modifying`les valeurs dans les registres et les adresses à un certain point d'exécution. Cela nous aide à voir comment cela affecterait l'exécution du programme.

#### Adresses

Pour modifier les valeurs dans GDB, nous pouvons utiliser la `set`commande. Cependant, nous utiliserons la `patch`commande `GEF`pour rendre cette étape beaucoup plus facile. Entrons `help patch`dans GDB pour obtenir son menu d'aide :

  Adresses

```
gef➤  help patch

Write specified values to the specified address.
Syntax: patch (qword|dword|word|byte) LOCATION VALUES
patch string LOCATION "double-escaped string"
...SNIP...

```

Comme nous pouvons le voir, nous devons fournir la `type/size`nouvelle valeur, la valeur `location`à stocker et la valeur que `value`nous souhaitons utiliser. Essayons donc de remplacer la chaîne stockée dans la `.data`section (à l'adresse `0x402000`comme nous l'avons vu précédemment) par la chaîne `Patched!\n`.

Nous allons d'abord arrêter `syscall`à `0x401019`, puis faire le patch, comme suit :

  Adresses

```
gef➤  break *0x401019

Breakpoint 1 at 0x401019
gef➤  r
gef➤  patch string 0x402000 "Patched!\\x0a"
gef➤  c

Continuing.
Patched!
 Academy!

```

Nous voyons que nous avons réussi à modifier la chaîne et à `Patched!\n Academy!`remplacer l'ancienne chaîne. Remarquez comment nous avons utilisé `\x0a`pour ajouter une nouvelle ligne après notre chaîne.

#### Registres

Notons également que nous n'avons pas remplacé la chaîne entière. En effet, nous n'avons modifié les caractères que jusqu'à la longueur de notre chaîne et avons laissé le reste de l'ancienne chaîne. Enfin, la `printf`fonction a spécifié une longueur de `0x12`octets à imprimer.

Pour résoudre ce problème, modifions la valeur stockée dans `$rdx`la longueur de notre chaîne, qui est `0x9`. Nous ne corrigerons qu'une taille d'un octet. Nous entrerons dans les détails du `syscall`fonctionnement plus tard dans le module. Démontrons en utilisant `set`to modifier `$rdx`, comme suit :

  Registres

```
gef➤  break *0x401019

Breakpoint 1 at 0x401019
gef➤  r
gef➤  patch string 0x402000 "Patched!\\x0a"
gef➤  set $rdx=0x9
gef➤  c

Continuing.
Patched!

```

Nous voyons que nous avons réussi à modifier la chaîne imprimée finale et que le programme produit quelque chose de notre choix. La possibilité de modifier les valeurs des registres et des adresses nous aidera beaucoup lors du débogage et de l'exploitation binaire, car elle nous permettra de tester diverses valeurs et conditions sans avoir à changer le code et à recompiler le binaire à chaque fois.

* * * * *

Conclusion
----------

`The ability to set breakpoints to stop the execution, step through a program and each of its instructions, examine various data and addresses at each point, and modify values when needed, enables us to do proper debugging and reverse engineering`.

Que nous souhaitions voir exactement pourquoi notre programme échoue ou comprendre comment un programme fonctionne et ce qu'il fait à chaque étape, GDB devient très pratique.

Pour les tests d'intrusion, ce processus nous permet de comprendre comment un programme gère les entrées à un moment donné et exactement pourquoi il échoue. Cela nous permet de développer des exploits qui profitent de tels échecs, comme nous l'apprendrons dans les modules d'exploitation binaire
