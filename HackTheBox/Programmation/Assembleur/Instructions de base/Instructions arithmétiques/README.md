Instructions arithmétiques
==========================

* * * * *

Le deuxième type d'instructions de base est les instructions arithmétiques. Avec les instructions arithmétiques, nous pouvons effectuer divers calculs mathématiques sur les données stockées dans les registres et les adresses mémoire. Ces instructions sont généralement traitées par l'ALU dans la CPU, entre autres instructions. Nous diviserons les instructions arithmétiques en deux types : les instructions qui ne prennent qu'un seul opérande ( `Unary`), les instructions qui prennent deux opérandes ( `Binary`).

* * * * *

Instructions unaires
--------------------

Voici les principales instructions arithmétiques unaires (nous supposerons que cela `rax`commence comme `1`pour chaque instruction) :

| Instruction | Description | Exemple |
| --- | --- | --- |
| `inc` | Incrémenter de 1 | `inc rax`-> `rax++`ou `rax += 1`->`rax = 2` |
| `dec` | Décrémenter de 1 | `dec rax`-> `rax--`ou `rax -= 1`->`rax = 0` |

Pratiquons ces instructions en revenant à notre `fib.s`code. Jusqu'à présent, nous avons initialisé `rax`et `rbx`avec nos valeurs initiales `0`et `1`avec l' `mov`instruction. Au lieu de déplacer la valeur immédiate de `1`to `bl`, passons `0`-y, puis utilisons `inc`pour la créer`1` :

Code : nasm

```
global  _start
section .text
_start:
    mov al, 0
    mov bl, 0
    inc bl

```

N'oubliez pas que nous avons utilisé `al`à la place de `rax`pour plus d'efficacité. Maintenant, assemblons notre code et exécutons-le avec`gdb` :

  gdb
```
$ ./assembler.sh fib.s -g ...COUPER...

──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
 → 0x401005 <_start+5> mouvement al, 0x0
────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rbx : 0x0  ...COUPER...

──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
 → 0x40100a <_start+10> inc. bl
──────────────────────────────────────── ───────── ──────────────────────────────────── enregistre ─── ─
$rbx : 0x1
```

Comme nous pouvons le voir, `rbx`commencé avec la valeur `0`, et avec `inc rbx`, il a été incrémenté jusqu'à `1`. L' `dec`instruction est similaire à `inc`, mais décrémente au `1`lieu d'incrémenter.

`This knowledge will become very handy later on.`

* * * * *

Instructions binaires
---------------------

Ensuite, nous avons les instructions arithmétiques binaires, et les principales sont : Nous supposerons que les deux `rax`commenceront `rbx`comme `1`pour chaque instruction.

| Instruction | Description | Exemple |
| --- | --- | --- |
| `add` | Ajouter les deux opérandes | `add rax, rbx`-> `rax = 1 + 1`->`2` |
| `sub` | Soustraire la source de la destination ( *c.-à-d.`rax = rax - rbx`* ) | `sub rax, rbx`-> `rax = 1 - 1`->`0` |
| `imul` | Multipliez les deux opérandes | `imul rax, rbx`-> `rax = 1 * 1`->`1` |

`Note that in all of the above instructions, the result is always stored in the destination operand, while the source operand is not affected.`

Commençons par discuter des `add`instructions. L'addition de deux nombres est l'étape essentielle du calcul d'une séquence de Fibonacci, puisque le nombre de Fibonacci actuel ( ) est la somme des deux qui le précèdent ( ).`Fn``Fn = Fn-1 + Fn-2`

Ajoutons donc `add rax, rbx`à la fin de notre `fib.s`code :

Code : nasm

```
global  _start

section .text
_start:
   mov al, 0
   mov bl, 0
   inc bl
   add rax, rbx

```

Maintenant, assemblons notre code et exécutons-le avec`gdb` :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b _start Point d'arrêt 1 à 0x401000
gef➤r   _ ...COUPER...

──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
   0x401004 <_start+4> inc bl
 → 0x401006 <_start+6> ajouter rax, rbx
──────────────────────────────── ─────── ──────────────────────────────────────── ────── enregistre ─── ─
$rax   : 0x1 $rbx : 0x1
```
Comme nous pouvons le voir, une fois l'instruction traitée, `rax`est égal à `0x1 + 0x0`, ce qui correspond à `0x1`. En utilisant le même principe, si nous avions d'autres nombres de Fibonacci dans `rax`et `rbx`, nous obtiendrions le nouveau Fibonacci en utilisant add.

Les deux `sub`et `imul`sont similaires à `add`, comme le montrent les exemples du tableau précédent. Essayez d'ajouter `sub`et `imult`au code ci-dessus, assemblez-le, puis exécutez-le `gdb`pour voir comment ils fonctionnent.

* * * * *

Instructions au niveau du bit
-----------------------------

Passons maintenant aux instructions au niveau du bit, qui sont des instructions qui fonctionnent au niveau du bit (nous supposerons cela `rax = 1`et `rbx = 2`pour chaque instruction) :

| Instruction | Description | Exemple |
| --- | --- | --- |
| `not` | Bitwise NOT (inverser tous les bits, 0->1 et 1->0) | `not rax`-> `NOT 00000001`->`11111110` |
| `and` | ET au niveau du bit (si les deux bits sont 1 -> 1, si les bits sont différents -> 0) | `and rax, rbx`-> `00000001 AND 00000010`->`00000000` |
| `or` | OU au niveau du bit (si l'un des bits est 1 -> 1, si les deux sont 0 -> 0) | `or rax, rbx`-> `00000001 OR 00000010`->`00000011` |
| `xor` | XOR au niveau du bit (si les bits sont identiques -> 0, si les bits sont différents -> 1) | `xor rax, rbx`-> `00000001 XOR 00000010`->`00000011` |

Ces instructions peuvent paraître déroutantes au premier abord, mais elles deviennent simples une fois que nous les comprenons. Chacune de ces instructions réalise l'instruction spécifiée sur chaque bit de la valeur. Par exemple, `not`ira à chaque bit et l'inversera, en tournant `0`les " en `1`" et en tournant `1`les " `0`en ". Essayez d'ajouter `not rax`à la fin de notre code précédent, assemblez-le, puis exécutez-le `gdb`pour voir comment il fonctionne.

De même, les deux instructions `and`/ `or`fonctionnent sur chaque bit et exécutent la porte `AND`/ `OR`sur chacun, comme le montrent les exemples ci-dessus. Chacune de ces instructions a ses cas d'utilisation dans Assembly.

Cependant, l'instruction que nous utiliserons le plus est `xor`. L' `xor`instruction a divers cas d'utilisation, mais comme elle met à zéro les bits similaires, nous pouvons l'utiliser pour transformer n'importe quelle valeur en 0 en `xor`ingérant une valeur avec elle-même. Nous devons mettre, `using xor on any register with itself will turn it into 0`.

Par exemple, si nous voulons transformer le `rax`registre en `0`, la manière la plus efficace de le faire est `xor rax, rax`, ce qui fera `rax = 0`. C'est simplement parce que tous les éléments de `rax`sont similaires et `xor`qu'ils se transformeront donc tous en `0`. En revenant à notre `fib.s`code précédent, au lieu de passer `0`aux deux `rax`et `rbx`, nous pouvons utiliser `xor`sur chacun d'eux, comme suit :

Code : nasm

```
global  _start

section .text
_start:
    xor rax, rax
    xor rbx, rbx
    inc rbx
    add rax, rbx

```

Ce code devrait effectuer exactement les mêmes opérations, mais désormais de manière plus efficace. Assemblons notre code et exécutons-le avec`gdb` :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b _start Point d'arrêt 1 à 0x401000
gef➤r   _ ...COUPER...

──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
 → 0x401001 <_start+1> xor eax, eax 0x401003 <_start+3> xor ebx, ebx
──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x0 $ rbx : 0x0
 ...COUPER...

──────────────────────────────────────── ────────── ───────────────────────────────── code:x86:64 ────
 → 0x40100c ajouter BYTE PTR [rax] , al
───────────────────────────────── ... ───────── ───────────────────────────────────── registres ── ──
$rax : 0x1 $rbx : 0x1
```

Comme nous pouvons le voir, `xor`l'interaction de nos registres avec eux-mêmes a transformé chacun d'eux en `0`'s, et le reste du code a effectué les mêmes opérations que précédemment, nous nous sommes donc retrouvés avec les mêmes valeurs finales pour les deux `rax`et `rbx`.
