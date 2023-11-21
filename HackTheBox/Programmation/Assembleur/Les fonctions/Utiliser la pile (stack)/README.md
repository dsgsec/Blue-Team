Utiliser la pile
================

* * * * *

Nous avons jusqu'à présent appris deux types d'instructions de contrôle : `Loops`et `Branching`. Avant d'en discuter `Functions`, nous devons comprendre comment utiliser la mémoire `Stack`. Dans cette `Computer Architecture`section, nous avons expliqué comment la RAM est segmentée en quatre segments différents, et chaque application se voit attribuer sa mémoire virtuelle et ses segments. Nous avons également discuté du `text`segment dans lequel les instructions d'assemblage de l'application sont chargées pour que le processeur puisse y accéder et du `data`segment qui contient les variables de l'application. Alors maintenant, commençons à discuter du `Stack`.

* * * * *

La pile
-------

La pile est un segment de mémoire alloué au programme pour stocker des données, et elle est généralement utilisée pour stocker des données, puis les récupérer temporairement. Le haut de la pile est désigné par le pointeur Top Stack `rsp`, tandis que le bas est désigné par le pointeur Base Stack `rbp`.

Nous pouvons introduire `push`des données dans la pile, et elles seront en haut de la pile (c'est-à-dire `rsp`), puis nous pouvons `pop`extraire des données de la pile dans un registre ou une adresse mémoire, et elles seront supprimées du haut de la pile.

| Instruction | Description | Exemple |
| --- | --- | --- |
| `push` | Copie le registre/l'adresse spécifié en haut de la pile | `push rax` |
| `pop` | Déplace l'élément en haut de la pile vers le registre/l'adresse spécifié | `pop rax` |

La pile a un design `Last-in First-out`( `LIFO`), ce qui signifie que nous ne pouvons `pop`extraire que le dernier élément `push`de la pile. Par exemple, si nous `push rax`entrons dans la pile, le haut de la pile sera désormais la valeur que `rax`nous venons de pousser. Si nous avons `push`quelque chose au-dessus, nous devrons le `pop`retirer de la pile jusqu'à ce que cette valeur `rax`atteigne le sommet de la pile, nous pouvons alors `pop`revenir à cette valeur `rax`.

Le tableau suivant montre le fonctionnement de la pile :

Nous pouvons cliquer sur `push`pour pousser une valeur de `rax`vers la pile, et vous pouvez cliquer sur `pop`pour faire apparaître la valeur supérieure de la pile dans `rax`.

|  |  |
|  |  |
|  |  |
| 0xabcdef | <-- Haut de la pile ( `$rsp`) |
| 0x1234567890 | <-- Bas de la pile ( `$rbp`) |

`rax: `\
pousser populaire

Réinitialiser la pile

* * * * *

Utilisation avec des fonctions/appels système
---------------------------------------------

Nous allons principalement transférer les données des registres dans la pile avant d'appeler a `function`ou appeler a `syscall`, puis les restaurer après la fonction et l'appel système. En effet, `functions`nous `syscalls`utilisons généralement les registres pour leur traitement, et donc si les valeurs stockées dans les registres sont modifiées après un appel de fonction ou un appel système, nous les perdrons.

Par exemple, si nous voulions appeler un appel système pour imprimer `Hello World`à l'écran et conserver la valeur actuelle stockée dans `rax`, nous le ferions `push rax`dans la pile. Ensuite, nous pouvons exécuter l'appel système et ensuite `pop`revenir à la valeur `rax`. Ainsi, de cette façon, nous pourrions à la fois exécuter l'appel système et conserver la valeur de `rax`.

* * * * *

POUSSER/POP
-----------

Notre code ressemble actuellement à ce qui suit :

Code : nasm

```
global  _start

section .text
_start:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0

```

Supposons que nous souhaitions appeler a `function`ou a `syscall`avant d'entrer dans la boucle. Pour préserver nos registres, nous devrons `push`empiler tous les registres que nous utilisons, puis les réinsérer après le `syscall`.

Pour `push`valoriser dans la pile, nous pouvons utiliser son nom comme opérande, comme dans `push rax`, et la valeur sera `copied`en haut de la pile. Lorsque nous voulons récupérer cette valeur, nous devons d'abord être sûrs qu'elle se trouve en haut de la pile, puis nous pouvons spécifier l'emplacement de stockage comme opérande, comme dans , après quoi la valeur sera à `pop rax`, `moved`et `rax`sera `removed`du haut de la pile. La valeur en dessous sera désormais au sommet de la pile (comme indiqué dans l'accise ci-dessus).

Puisque la pile a une conception LIFO , lorsque nous restaurons nos registres, nous devons les faire dans l'ordre inverse . Par exemple, si nous poussons rax puis rbx , lors de la restauration, nous devons faire apparaître rbx puis faire apparaître rax .

Alors, pour sauvegarder nos registres avant d'entrer dans la boucle, poussons-les vers le registre. Heureusement, nous utilisons uniquement `rax`and `rbx`, et nous n'aurons donc besoin que `push`de ces deux registres dans la pile, puis `pop`d'eux après l'appel système, comme suit :

Code : nasm

```
global  _start

section .text
_start:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
    push rax        ; push registers to stack
    push rbx
    ; call function
    pop rbx         ; restore registers from stack
    pop rax
...SNIP...

```

Notez comment la restauration des registres avec `pop`s'est déroulée dans l'ordre inverse.

Maintenant, assemblons notre code et testons-le avec`gdb` :

  gdb
```
$ ./assembler.sh fib.s -g gef➤  b _start gef➤   r ...COUPER...
gef➤  si gef➤  si gef➤  si ────────────────────────────────── ─────── ──────────────────────────────────────── ──── enregistre ────
$rax : 0x0 $rbx   : 0x1 ───────────────────────────────────── ───────── ──────────────────────────────────────── ─── pile ────
0x00007fffffffe410 │ +0x0000 : 0x0000000000000001 ← $rsp
0x00007fffffffe418 │+0x0008: 0x0000000000000000 0x00007fffffffe420 │+0x0010: 0x0000000000000000 0x00007fffffffe428 │+0x0018: 0x0000000000000000 0x00007fffffffe430 │+0x0020: 0x0000000000000000 0x00007fffffffe438 │+0x0028: 0x0000000000000000 0x00007fffffffe440 │+0x0030: 0x0000000000000000 0x00007fffffffe448 │+0x0038: 0x0000000000000000 ───── ──────────────────────────────────────── ────────── <_start+ 9 > pousser Rax
  0x40100f <_start+10> pousser rbx 0x401010 <_start+11> pop rbx 0x401011 <_start+12> pop rax
──────────────────────────────────────── ────────── ──────────────────────────────────────── ──────────
```
Nous voyons qu'avant d'exécuter `push rax`, nous avons `rax = 0x0`et `rbx = 0x1`. Passons maintenant `push`à `rax`and `rbx`et voyons comment la pile et les registres changent :

  gdb
```
──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x0 $ rbx : 0x1 ── ──────────────────────────────────────── ────────── ───────────────────────────────────── pile ── ── 0x00007fffffffe408 │+0x0000 : 0x0000000000000000

 ← $rsp
0x00007fffffffe410 │+0x0008: 0x0000000000000001 0x00007fffffffe418 │+0x0010: 0x0000000000000000 0x00007fffffffe420 │+0x0018: 0x0000000000000000 0x00007fffffffe428 │+0x0020: 0x0000000000000000 0x00007fffffffe430 │+0x0028: 0x0000000000000000 0x00007fffffffe438 │+0x0030: 0x0000000000000000 0x00007fffffffe440 │+0x0038: 0x0000000000000000 ───── ──────────────────────────────────────── ────────── <loopFib+ 9  > pousser rax → 0x40100f < _start +
   10> pousser rbx
  0x401010 <_start+11> pop rbx 0x401011 <_start+12> pop rax
──────────────────────────────────────── ────────── ──────────────────────────────────────── ────────── ...COUPER...
──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x0 $ rbx : 0x1 ── ──────────────────────────────────────── ────────── ───────────────────────────────────── pile ── ── 0x00007fffffffe400 │+0x0000 : 0x0000000000000001

 ← $rsp
0x00007fffffffe408 │+0x0008: 0x0000000000000000 0x00007fffffffe410 │+0x0010: 0x0000000000000001 0x00007fffffffe418 │+0x0018: 0x0000000000000000 0x00007fffffffe420 │+0x0020: 0x0000000000000000 0x00007fffffffe428 │+0x0028: 0x0000000000000000 0x00007fffffffe430 │+0x0030: 0x0000000000000000 0x00007fffffffe438 │+0x0038: 0x0000000000000000 ───── ──────────────────────────────────────── ────────── ──────────────────────────── code:x86:64 ────
   0x40100e <_start+9> push rax
   0x40100f <_start+10 > pousser rbx
 → 0x401010 <_start+11> pop rbx 0x401011 <_start+12> pop rax
──────────────────────────────────────── ────────── ──────────────────────────────────────── ──────────
```
Nous voyons qu'après avoir `push`édité les deux `rax`et `rbx`, nous avons les valeurs suivantes en haut de notre pile :

```
0x00007fffffffe408│+0x0000: 0x0000000000000001	 ← $rsp
0x00007fffffffe410│+0x0008: 0x0000000000000000

```

Nous voyons qu'en haut de la pile, nous avons la dernière valeur que nous avons poussée, qui est `rbx = 0x1`, et juste en dessous, nous avons la valeur que nous avons poussée avant elle `rax = 0x0`. C'est comme nous nous y attendions et similaire à l'exercice de pile ci-dessus. Nous remarquons également qu'après avoir poussé nos valeurs, elles sont restées dans les registres, `meaning a push is, in fact, a copy to stack`.

Supposons maintenant que nous ayons fini d'exécuter une `print`fonction et que nous souhaitions récupérer nos valeurs, nous continuons donc avec les `pop`instructions :

  gdb
```
──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x0 $ rbx : 0x1 ── ──────────────────────────────────────── ────────── ───────────────────────────────────── pile ── ── 0x00007fffffffe408 │+0x0000 : 0x0000000000000000

 ← $rsp
0x00007fffffffe410 │+0x0008: 0x0000000000000001 0x00007fffffffe418 │+0x0010: 0x0000000000000000 0x00007fffffffe420 │+0x0018: 0x0000000000000000 0x00007fffffffe428 │+0x0020: 0x0000000000000000 0x00007fffffffe430 │+0x0028: 0x0000000000000000 0x00007fffffffe438 │+0x0030: 0x0000000000000000 0x00007fffffffe440 │+0x0038: 0x0000000000000000 ───── ──────────────────────────────────────── ────────── ──────────────────────────── code:x86:64 ────
   0x40100e <_start+9> push rax
   0x40100f <_start+10 > pousser rbx
   0x401010 <_start+11> pop rbx
 → 0x401011 <_start+12> pop rax
───────────────────────── ───── ──────────────────────────────────────── ────────── ──────────────────── ...COUPER...
──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x0 $ rbx : 0x1 ── ──────────────────────────────────────── ────────── ───────────────────────────────────── pile ── ── 0x00007fffffffe410 │+0x0000 : 0x0000000000000001

 ← $rsp
0x00007fffffffe418 │+0x0008: 0x0000000000000000 0x00007fffffffe420 │+0x0010: 0x0000000000000000 0x00007fffffffe428 │+0x0018: 0x0000000000000000 0x00007fffffffe430 │+0x0020: 0x0000000000000000 0x00007fffffffe438 │+0x0028: 0x0000000000000000 0x00007fffffffe440 │+0x0030: 0x0000000000000000 0x00007fffffffe448 │+0x0038: 0x0000000000000000 ───── ──────────────────────────────────────── ────────── ──────────────────────────── code:x86:64 ────
   0x40100f <_start+9> push rax
   0x40100f <_start+10 > pousser rbx
   0x401010 <_start+11> pop rbx
   0x401011 <_start+12> pop rax
 → 0x401011 <loopFib+0> ajouter rax, rbx
───────────────── ─── ──────────────────────────────────────── ────────── ──────────────────────────────
```
Nous voyons qu'après `pop`avoir extrait deux valeurs du haut de la pile, elles ont été supprimées de la pile, et la pile ressemble maintenant exactement à ce qu'elle était au début. Les deux valeurs ont été replacées dans `rbx`et `rax`. Nous n'avons peut-être pas vu de différence puisqu'ils n'ont pas été modifiés dans les registres dans ce cas.

L'utilisation de la pile est très simple. La seule chose que nous devons garder à l'esprit est l'ordre dans lequel nous poussons nos registres et l'état de la pile pour restaurer nos données en toute sécurité et ne pas restaurer une valeur différente lorsqu'une valeur différente se trouve en haut de la pile `pop`.

Nous pouvons supprimer les instructions `push`et `pop`de notre code pour le moment, et nous les utiliserons lorsque nous lancerons des appels de fonction. Avec cela, nous devrions être prêts à utiliser `syscall`et `function`à appeler. Discutons- `syscalls`en ensuite.
