Branchement inconditionnel
==========================

* * * * *

Le deuxième type `Control Instructions`est `Branching Instructions`, qui sont des instructions générales qui nous permettent d'accéder `jump`à n'importe quel point du programme si une condition spécifique est remplie. Parlons d'abord de l'instruction de branchement la plus élémentaire : `jmp`, qui sautera toujours vers un emplacement sans condition.

* * * * *

Boucle JMPFib
-------------

L' `jmp`instruction fait passer le programme à l'étiquette ou à l'emplacement spécifié dans son opérande afin que l'exécution du programme s'y poursuive. Une fois que l'exécution d'un programme est dirigée vers un autre emplacement, il continue à traiter les instructions à partir de ce point. Si nous voulions passer temporairement à un point puis revenir au point d'appel d'origine, nous utiliserions des fonctions, dont nous parlerons dans la section suivante.

L' `jmp`instruction de base est inconditionnelle, ce qui signifie qu'elle sautera toujours à l'emplacement spécifié, quelles que soient les conditions. Cela contraste avec `Conditional Branching`les instructions qui ne sautent que si une condition spécifique est remplie, dont nous parlerons ensuite.

| Instruction | Description | Exemple |
| --- | --- | --- |
| `jmp` | Passe à l'étiquette, à l'adresse ou à l'emplacement spécifié | `jmp loop` |

Essayons de l'utiliser `jmp`dans notre `fib.s`programme et voyons comment cela modifierait le flux d'exécution. Au lieu de revenir en arrière `loopFib`, revenons `jmp`-y : notre code final est donc :

Code : nasm

```
global  _start

section .text
_start:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
    mov rcx, 10
loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    jmp loopFib

```

Maintenant, assemblons notre code et exécutons-le avec `gdb`. Nous allons encore une fois `b loopFib`et voir comment cela change :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b loopFib Point d'arrêt 1 à 0x40100e
gef➤  r ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rbx : 0x1 $rcx   : 0xa $rcx   : 0xa ─────────────────────────────────── ─────── ──────────────────────────────────────── ─── enregistre ────
$rax   : 0x1 $rbx   : 0x1 $rcx   : 0xa ───────────────────────────── ────── ──────────────────────────────────────── ─────── enregistre ── ──
$rax   : 0x1 $rbx   : 0x2 $rcx   : 0xa ─────────────────────────────── ──────── ──────────────────────────────────────── ─── enregistre ────
$rax : 0x2 $rbx   : 0x3 $rcx   : 0xa ────────────────────────────────── ──────── ──────────────────────────────────────── ─── enregistre ────
$rax : 0x3 $rbx   : 0x5 $rcx   : 0xa ─────────────────────────────── ──────── ──────────────────────────────────────── ─── enregistre ────
$rax : 0x5 $rbx   : 0x8 $rcx   : 0xa
```
Nous appuyons `c`plusieurs fois pour laisser le programme revenir plusieurs fois à `loopFib`. Comme nous pouvons le voir, le programme exécute toujours la même fonction et calcule toujours correctement la séquence de Fibonacci. Cependant, `the main difference from the loop is that 'rcx' is not decrementing.`cela est dû au fait qu'une `jmp`instruction ne se considère pas `rcx`comme un compteur (comme c'est `loop`le cas) et ne le décrémentera donc pas automatiquement.

Supprimons notre point d'arrêt avec `del 1`, et appuyez sur `c`pour voir dans quel but le programme va s'exécuter :

  gdb
```
gef➤   pause info Num Type Disp Enb Adresse Quoi 1 point d'arrêt garder y 0x000000000040100e <loopFib> point d'arrêt déjà atteint 6 fois
gef➤  del 1 gef➤   c Continuer.
 Le programme a reçu le signal SIGINT, Interruption. 0x000000000040100e dans loopFib()
──────────────────────────────────────── ────────── ─────────────────────────────────── registres ────
$rax   : 0x2e02a93188557fa9 $rbx   : 0x903b4b15ce8cedf0 $rcx   : 0xa
```
Nous avons remarqué que le programme a continué à fonctionner jusqu'à ce que nous appuyions `ctrl+c`après quelques secondes pour le tuer, moment auquel le nombre de Fibonacci a atteint `0x903b4b15ce8cedf0`(ce qui est un nombre énorme). Cela est dû à l' `jmp`instruction inconditionnelle, qui revient `loopFib`sans cesse en arrière puisqu'une condition spécifique ne la restreint pas. Ceci est similaire à une `(while true)`boucle.

C'est pourquoi le branchement inconditionnel est généralement utilisé dans les cas où il faut toujours sauter, et il ne convient pas aux boucles, car il bouclera indéfiniment. Pour arrêter de sauter lorsqu'une condition spécifique est remplie, nous l'utiliserons `Conditional Branching`pour nos prochaines étapes.
