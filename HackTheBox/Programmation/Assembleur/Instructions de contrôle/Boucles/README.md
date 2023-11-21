Boucles
=======

* * * * *

Maintenant que nous avons couvert les instructions de base, nous pouvons commencer à apprendre `Program Control Instructions`. Comme nous le savons déjà, le code Assembly est basé sur une ligne, il recherchera donc toujours la ligne suivante pour obtenir les instructions à traiter. Cependant, comme on peut s'y attendre, la plupart des programmes ne suivent pas un simple ensemble d'étapes séquentielles mais ont généralement une structure plus complexe.

C'est là `Control`qu'interviennent les instructions. De telles instructions nous permettent de modifier le déroulement du programme et de le diriger vers une autre ligne. Il existe de nombreux exemples de la manière dont cela peut être réalisé. Nous avons déjà évoqué `Directives`le fait d'indiquer au programme de diriger l'exécution vers une étiquette spécifique.

D'autres types `Control Instructions`incluent :

| `Loops` | `Branching` | `Function Calls` |
| --- | --- | --- |

* * * * *

Structure de boucle
-------------------

Commençons par discuter `Loops`. Une boucle dans l'assemblage est un ensemble d'instructions qui se répètent plusieurs `rcx`fois. Prenons l'exemple suivant :

Code : nasm

```
exampleLoop:
    instruction 1
    instruction 2
    instruction 3
    instruction 4
    instruction 5
    loop exampleLoop

```

Une fois que le code assembleur atteint `exampleLoop`, il commencera à exécuter les instructions qu'il contient. Nous devons définir le nombre d'itérations que nous voulons que la boucle passe dans le `rcx`registre. Chaque fois que la boucle atteint l' `loop`instruction, elle diminuera `rcx`de `1`(c'est-à-dire `dec rcx`) et reviendra à l'étiquette spécifiée, `exampleLoop`dans ce cas. Ainsi, avant d'entrer dans une boucle, nous devons `mov`enregistrer le nombre d'itérations de boucle que nous voulons `rcx`.

| Instruction | Description | Exemple |
| --- | --- | --- |
| `mov rcx, x` | Définit `rcx`le compteur de boucle ( ) sur`x` | `mov rcx, 3` |
| `loop` | Revient au début de `loop`jusqu'à ce que le compteur atteigne`0` | `loop exampleLoop` |

* * * * *

boucleFib
---------

Pour démontrer cela, revenons à notre `fib.s`code :

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

Puisque tout nombre de Fibonacci actuel est la somme des deux nombres qui le précèdent, nous pouvons automatiser cela avec une boucle. Supposons que le numéro actuel est stocké dans `rax`, donc il est , et que le numéro suivant est stocké dans , donc il est .`Fn``rbx``Fn+1`

En commençant par le dernier numéro as `0`et le numéro actuel as `1`, nous pouvons avoir notre boucle comme suit :

1.  Obtenez le prochain numéro avec`0 + 1 = 1`
2.  Déplacer le numéro actuel vers le dernier numéro ( `1 in place of 0`)
3.  Déplacer le numéro suivant vers le numéro actuel ( `1 in place of 1`)
4.  Boucle

Si nous faisons cela, nous obtiendrons `1`comme dernier numéro et `1`comme numéro actuel. Si nous bouclons à nouveau, nous obtiendrons `1`le dernier numéro et `2`le numéro actuel. Alors, implémentons cela sous forme d'instructions d'assemblage. Puisque nous pouvons supprimer le dernier nombre `0`après l'avoir utilisé dans l'ajout, stockons le résultat à sa place :

-   `add rax, rbx`

Nous devons déplacer le numéro actuel à la place du dernier numéro et déplacer le numéro suivant vers le numéro actuel. Cependant, nous avons le numéro suivant dans `rax`et l'ancien numéro dans `rbx`, ils sont donc échangés. Pouvez-vous penser à des instructions pour les échanger ?

Utilisons l' `xchg`instruction pour échanger les deux nombres :

-   `xchg rax, rbx`

Maintenant, nous pouvons simplement `loop`. Cependant, avant d'entrer dans une boucle, nous devons définir `rcx`le nombre d'itérations souhaité. Commençons par `10`les itérations et ajoutons-le après avoir initialisé le `rax`and `rbx`to `0`and `1`:

Code : nasm

```
_start:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
    mov rcx, 10

```

Nous pouvons maintenant définir notre boucle comme indiqué ci-dessus :

Code : nasm

```
loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    loop loopFib

```

Notre code final est donc :

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
    loop loopFib

```

* * * * *

Boucle boucleFib
----------------

Assemblons notre code et exécutons-le avec `gdb`. Nous ferons une pause à `b loopFib`, afin de pouvoir examiner le code à chaque itération de la boucle. Nous voyons les valeurs de registre suivantes avant la première itération :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b loopFib Point d'arrêt 1 à 0x40100e
gef➤  r ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rax   : 0x0 $rbx   : 0x1 $rcx   : 0xa

On voit qu'on commence par `rax = 0`et `rbx = 1`. Appuyons `c`pour passer à l'itération suivante :

  gdb

──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x1 $ rbx : 0x1 $rcx : 0x9

Nous avons maintenant `1`and `1`, comme prévu, avec `9`les itérations restantes. Continuons `c`encore :

  gdb

──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x1 $ rbx : 0x2 $rcx : 0x8

Nous en sommes maintenant à `1`et `2`. Vérifions les trois prochaines itérations :

  gdb

──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x2 $ rbx : 0x3 $rcx : 0x7 ────────────────────────────────────── ────────── ───────────────────────────────────── registres ── ── $rax : 0x3 $rbx : 0x5 $rcx : 0x6 ───────────────────────────────────── ───────── ─────────────────────────────────────── registres ──── $rax : 0x5 $rbx : 0x8 $rcx : 0x5

Comme nous pouvons le voir, le script calcule avec succès la séquence de Fibonacci sous la forme `0, 1, 1, 2, 3, 5, 8`. Passons à la dernière itération, où `rbx`devrait être`55` :

  gdb

──────────────────────────────────────── ────────── ─────────────────────────────────── rax : 0x22 $ rbx : 0x37 $rcx : 0x1
```

Nous voyons que `rbx`est `0x37`égal à `55`en décimal. Nous pouvons le confirmer avec la `p/d $rax`commande :

```
gef➤  p/d $rbx

$3 = 55

```

Comme nous pouvons le voir, nous avons utilisé avec succès des boucles pour automatiser le calcul de la séquence de Fibonacci. Essayez d'augmenter `rcx`pour voir quels sont les prochains nombres de la séquence de Fibonacci.
