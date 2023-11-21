Branchement conditionnel
========================

* * * * *

Contrairement aux instructions de branchement inconditionnel, `Conditional Branching`les instructions ne sont traitées que lorsqu'une condition spécifique est remplie, basée sur les opérandes Destination et Source. Une instruction de saut conditionnel a plusieurs variétés telles que `Jcc`, où `cc`représente le code de condition. Voici quelques-uns des principaux codes de condition :

| Instruction | Condition | Description |
| --- | --- | --- |
| `jz` | `D = 0` | Destination`equal to Zero` |
| `jnz` | `D != 0` | Destination`Not equal to Zero` |
| `js` | `D < 0` | Destination`is Negative` |
| `jns` | `D >= 0` | Destination `is Not Negative`(c'est-à-dire 0 ou positif) |
| `jg` | `D > S` | `Greater than`Source de destination |
| `jge` | `D >= S` | `Greater than or Equal`Source de destination |
| `jl` | `D < S` | `Less than`Source de destination |
| `jle` | `D <= S` | `Less than or Equal`Source de destination |

Il existe de nombreuses autres conditions similaires que nous pouvons également utiliser. Pour une liste complète des conditions, nous pouvons nous référer au dernier [manuel Intel x86_64](https://www.intel.com/content/dam/www/public/us/en/documents/manuals/64-ia-32-architectures-software-developer-instruction-set-reference-manual-325383.pdf#page=585) , dans la `Jcc-Jump if Condition Is Met`section. Les instructions conditionnelles ne se limitent pas `jmp`uniquement aux instructions, mais sont également utilisées avec d'autres instructions d'assemblage à usage conditionnel, comme les instructions `CMOVcc`et .`SETcc`

Par exemple, si nous voulons exécuter une `mov rax, rbx`instruction, mais seulement si la condition est `= 0`, alors nous pouvons utiliser l' instruction `CMOVcc`ou `conditional mov`, telle que `cmovz rax, rbx`instruction. De même, si nous voulons nous déplacer si la condition est `<`, alors nous pouvons utiliser l' `cmovl rax, rbx`instruction, et ainsi de suite pour d'autres conditions. La même chose s'applique à l' `set`instruction, qui définit l'octet de l'opérande `1`si la condition est remplie ou `0`non. Un exemple de ceci est `setz rax`.

* * * * *

Registre RFLAGS
---------------

Nous avons parlé du respect de certaines conditions, mais nous n'avons pas encore discuté de la manière dont ces conditions sont remplies ni de l'endroit où ils sont stockés. C'est ici que nous utilisons le `RFLAGS`registre, que nous avons brièvement mentionné dans la section Registres.

Le `RFLAGS`registre est composé de 64 bits comme n'importe quel autre registre. Cependant, ce registre ne contient pas de valeurs mais contient à la place des bits d'indicateur. Chaque bit « ou ensemble de bits » tourne vers `1`ou `0`en fonction de la valeur de la dernière instruction.

`Arithmetic instructions set the necessary 'RFLAG' bits depending on their outcome.`Par exemple, si une `dec`instruction aboutit à un `0`, alors le bit `#6`, le drapeau zéro `ZF`, se transforme en `1`. De même, chaque fois que le bit `#6`est `0`, cela signifie que le drapeau zéro est désactivé. De même, si une instruction de division aboutit à un nombre flottant, alors le bit Carry Flag `CF`est activé, ou si une `sub`instruction aboutit à une valeur négative, alors le bit Sign Flag `SF`est activé, et ainsi de suite.

Remarque : Lorsqu'il `ZF`est activé (c'est-à-dire is `1`), il est appelé Zero `ZR`, et lorsqu'il est éteint (c'est-à-dire is `0`), il est appelé Not Zero `NZ`. Ce nom peut correspondre au code de condition utilisé dans les instructions, comme `jnz`celui qui saute avec `NZ`. Mais pour éviter toute confusion, concentrons-nous simplement sur le nom du drapeau.

Il existe de nombreux indicateurs dans un programme assembleur, et chacun d'eux possède son(ses) propre(s) bit(s) dans le `RFLAGS`registre. Le tableau suivant présente les différents drapeaux du `RFLAGS`registre :

| Morceaux) | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | dix | 11 | 12-13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22-63 |
| :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-: |
| Étiquette ( `1`/ `0`) | `CF`( `CY`/ `NC`) | `1` | `PF`( `PE`/ `PO`) | `0` | `AF`( `AC`/ `NA`) | `0` | `ZF`( `ZR`/ `NZ`) | `SF`( `NC`/ `PL`) | `TF` | `IF`( `EL`/ `DI`) | `DF`( `DN`/ `UP`) | `OF`( `OV`/ `NV`) | `IOPL` | `NT` | `0` | `RF` | `VM` | `AC` | `VIF` | `VIP` | `ID` | `0` |
| Description | Porter un drapeau | *Réservé* | Drapeau de parité | *Réservé* | Drapeau de transport auxiliaire | *Réservé* | Drapeau zéro | Signer le drapeau | Drapeau piège | Indicateur d'interruption | Drapeau de direction | Drapeau de débordement | Niveau de privilège E/S | Tâche imbriquée | *Réservé* | Indicateur de reprise | Mode virtuel-x86 | Vérification de l'alignement / Contrôle d'accès | Indicateur d'interruption virtuelle | Interruption virtuelle en attente | Drapeau d'identification | *Réservé* |

Tout comme les autres registres, le registre 64 bits `RFLAGS`possède un sous-registre de 32 bits appelé `EFLAGS`, et un sous-registre de 16 bits appelé `FLAGS`, qui contient les indicateurs les plus significatifs que nous pouvons rencontrer.\
Les drapeaux qui nous intéresseraient le plus sont :

-   Le Carry Flag `CF`: Indique si nous avons un flotteur.
-   Le drapeau de parité `PF`: indique si un nombre est pair ou impair.
-   Le drapeau zéro `ZF`: indique si un nombre est zéro.
-   Le Sign Flag `SF`: Indique si un registre est négatif.

Tous les indicateurs ci-dessus font partie des premiers bits du `FLAGS`sous-registre. Nous n'utiliserons que l' `jnz`instruction pour notre projet de module, qui est appliquée chaque fois que l' `ZF`indicateur est égal à `0`(c'est-à-dire Not Zero `NZ`). Voyons donc comment procéder.

* * * * *

Boucle JNZFib
-------------

L' `loop loopFib`instruction que nous avons utilisée dans la dernière section est en fait une combinaison de deux instructions : `dec rcx`et `jnz loopFib`, mais comme le bouclage est une fonction très courante, l' `loop`instruction a été créée pour réduire la taille du code et être plus efficace, au lieu d'utiliser les deux à chaque fois. Cependant, les instructions de saut conditionnel sont beaucoup plus polyvalentes que `loop`, puisqu'elles nous permettent de sauter n'importe où dans le programme dans n'importe quelle condition dont nous avons besoin.

Bien qu'il soit plus efficace d'utiliser `loop`, pour démontrer l'utilisation de `jnz`, revenons à notre code et essayons d'utiliser l' `jnz`instruction au lieu de`loop` :

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
    dec rcx			; decrement rcx counter
    jnz loopFib		; jump to loopFib until rcx is 0

```

Nous voyons que nous avons remplacé `loop loopFib`par `dec rcx`and `jnz loopFib`, de sorte que chaque fois que la boucle atteint sa fin, le compteur rcx décrémente de 1 et le programme revient à `loopFib`if `ZF`n'est pas défini. Une fois `rcx`atteint `0`, le drapeau zéro `ZF`serait allumé sur `1`, et `jnz`ne sauterait donc plus (puisque c'est `NZ`), et nous quitterions la boucle. Assemblons notre code et exécutons-le avec `gdb`, pour voir ceci en effet :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b loopFib Point d'arrêt 1 à 0x40100e
gef➤  r ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rax   : 0x0 $rbx : 0x1 $rcx : 0xa $eflags : [zéro report de parité ajuster le signe piège INTERRUPTION débordement de direction reprendre l'identification virtualx86] ──────────────────────────── ── ──────────────────────────────────────── ────────── ── enregistre ────
$rax   : 0x1 $rbx   : 0x1 $rcx : 0x9 $eflags: [zéro report PARITÉ ajuster signe piège INTERRUPTION débordement de direction reprendre identification virtualx86] ──────────────────────────── ── ──────────────────────────────────────── ────────── ── registres ────
$rax   : 0x1 $rbx : 0x2 $rcx : 0x8 $eflags: [zéro report de parité ajuster le signe piège INTERRUPTION de débordement de direction reprendre l'identification virtualx86]
```
Nous pouvons voir que nous calculons toujours correctement la séquence de Fibonacci. A chaque itération de la boucle, on diminue `rcx`, et le `zero`drapeau est éteint, tandis que le `parity`drapeau est allumé quand `rcx`est un nombre impair. Les valeurs RFLAGS à ce stade sont définies après l' `dec rcx`instruction, car il s'agit de la dernière instruction arithmétique avant la pause. Ainsi, les États du pavillon sont pour `rcx`.

Remarque : `GEF`nous montre l'état des drapeaux dans le `RFLAGS`registre. Les drapeaux écrits en majuscules grasses sont allumés.

Sortons `c`de la boucle pour voir l'état des registres et des RFLAGS après `rcx`les atteintes`0` :

  gdb
```
gef➤
Continuer.
──────────────────────────────────────── ────────── ─────────────────────────────────── registres ────
$rax : 0x37 $rbx : 0x59 $rcx : 0x0 $eflags: [ ZÉRO report PARITÉ ajuster signe piège INTERRUPTION débordement de direction REPRENDRE l' identification virtualx86]
```
Nous voyons qu'une fois `rcx`atteint `0`, le `zero`drapeau est défini sur on `1`et `jnz`ne revient plus à `loopFib`, donc le programme arrête de s'exécuter.

* * * * *

CMP
---

Il existe d'autres cas où nous pouvons souhaiter utiliser une instruction de saut conditionnel dans notre projet de module. Par exemple, nous pouvons vouloir arrêter l'exécution du programme lorsque le nombre de Fibonacci est supérieur à `10`. Nous pouvons le faire en utilisant l' `js loopFib`instruction, qui reviendrait à `loopFib`tant que la dernière instruction arithmétique aboutissait à un nombre positif.

Dans ce cas, nous n'utiliserons pas l' `jnz`instruction ou le `rcx`registre mais l'utiliserons `js`directement après avoir calculé le nombre de Fibonacci actuel. Mais comment tester si le nombre de Fibonacci actuel (c'est-à-dire `rbx`) est inférieur à `10`? C'est là que nous arrivons à l'instruction Compare `cmp`.

L'instruction Compare `cmp`compare simplement les deux opérandes, en soustrayant le deuxième opérande du premier opérande (c'est-à-dire - ), puis définit les indicateurs nécessaires dans le registre. Par exemple, si nous utilisons , alors l'instruction de comparaison ferait « » et définirait les indicateurs en fonction du résultat.`D1``S2``RFLAGS``cmp rbx, 10``rbx - 10`

| Instruction | Description | Exemple |
| --- | --- | --- |
| `cmp` | Définit `RFLAGS`en soustrayant le deuxième opérande du premier opérande (c'est-à-dire premier - seconde) | `cmp rax, rbx`->`rax - rbx` |

Ainsi, une fois le premier nombre de Fibonacci calculé, il fera ' `1 - 10`', et le résultat serait `-9`, donc il sautera puisque c'est un nombre négatif `<0`. Une fois que nous atteignons le premier nombre de Fibonacci supérieur à `10`, qui est `13`ou `0xd`, cela fera ' `13 - 10`', et le résultat serait ' `3`', auquel cas `js`il ne sauterait plus, car le résultat est un nombre positif `>=0`.

Nous pourrions utiliser `sub`des instructions pour effectuer la même soustraction et définir les drapeaux si nous le souhaitions. Cependant, cela ne serait pas efficace, car nous modifierions la valeur de l'un des registres, tandis que le `cmp`seul comparerait et ne stockerait le résultat nulle part.`The main advantage of 'cmp' is that it does not affect the operands.`

Remarque : Dans une `cmp`instruction, le premier opérande (c'est-à-dire la Destination) doit être un registre, tandis que l'autre peut être un registre, une variable ou une valeur immédiate.

Modifions donc notre code pour utiliser `cmp`and `js`, comme suit :

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

Notez que nous avons supprimé l' `mov rcx, 10`instruction puisque nous ne bouclons plus avec `rcx`. Nous aurions pu l'utiliser à `cmp`la place de `10`, mais en l'utilisant directement, `10`nous utilisons une instruction de moins, ce qui rend notre code plus court et plus efficace.

Maintenant, assemblons notre code et exécutons-le avec `gdb`, pour voir comment cela fonctionne. Nous allons faire une pause à `loopFib`, puis avancer `si`jusqu'à ce que nous atteignions l' `js loopFib`instruction :

  gdb
```
$ ./assembler.sh fib.s -g gef➤   b loopFib Point d'arrêt 1 à 0x401009
gef➤  r ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rax   : 0x1 $rbx   : 0x1 $eflags : [zéro CARRY parité ADJUST  SIGN trap INTERRUPT direction débordement reprise identification virtualx86] ─────────────────────────── ─── ──────────────────────────────────────── ────────── code:x86:64 ────
   0x401009 <loopFib+0> ajouter rax, rbx
   0x40100c <loopFib+3> xchg rbx, rax
   0x40100e <loopFib+5> cmp rbx, 0xa
 → 0x401012 <loopFib+9> js 0x401 009 <boucleFib > PRIS [Raison : S]
```
Nous voyons que lors de la première itération de `loopFib`, une fois atteint `js loopFib`, le `SIGN`drapeau est activé `1`comme prévu, puisque le résultat de `1 - 10`est un nombre négatif. Nous remarquons également `GEF`le message `TAKEN [Reason: S]`, qui nous indique commodément que ce saut conditionnel sera effectué et donne la raison comme `S`, ce qui signifie que le `SIGN`drapeau est activé.

Maintenant, continuons `c`jusqu'à ce `rbx`qu'il soit supérieur à `10`, auquel cas `js`il ne devrait plus sauter. Au lieu d'appuyer manuellement `c`plusieurs fois, profitons de cette occasion pour apprendre à définir des points d'arrêt conditionnels dans `gdb`.

Supprimons d'abord le point d'arrêt actuel avec `del 1`, puis définissons notre point d'arrêt conditionnel. La syntaxe est très similaire à la définition de points d'arrêt réguliers `b loopFib`, mais nous ajoutons une `if`condition après, telle que «`b loopFib if $rbx > 10` ». De plus, au lieu de rompre avec `loopFib`puis d'utiliser `si`pour atteindre `js`, rompons directement `js`avec `*`pour faire référence à son emplacement «`b *loopFib+9 if $rbx > 10` » ou «`b *0x401012 if $rbx > 10` ».\
N'oubliez pas : nous pouvons trouver l'emplacement d'une instruction avec `disas loopFib`.

Nous voyons ce qui suit :
```
gef➤  del 1 gef➤   disas loopFib Dump du code assembleur pour la fonction loopFib : ..COUPER... 0x0000000000401012 <+9> : js 0x401009 gef➤  b *loopFib+9 si $rbx > 10 Point d'arrêt 2 à 0x401012
gef➤  c ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rax : 0x8 $rbx : 0xd $eflags: [zéro report PARITÉ ajuster signe piège INTERRUPTION débordement de direction reprendre identification virtualx86] ──────────────────────────── ── ──────────────────────────────────────── ────────── code:x86:64 ────
    0x401009 <loopFib+0> ajouter rax, rbx
    0x40100c <loopFib+3> xchg rbx, rax
    0x40100e <loopFib+5> cmp rbx, 0xa
 → 0x401012 <loopFib+9> js 0x401 009 <boucleFib > NON pris [Raison : !(S)]
```
Nous voyons maintenant que la dernière instruction arithmétique ' `13 - 10`' a donné un nombre positif, le `sign`drapeau n'est plus activé, donc `GEF`nous indique que ce saut est `NOT TAKEN`, avec la raison `!(S)`, ce qui signifie que le `sign`drapeau n'est pas activé.

Comme nous pouvons le voir, l'utilisation du branchement conditionnel est très puissante et nous permet d'exécuter des instructions plus avancées basées sur une condition que nous spécifions. Nous pouvons utiliser l' `cmp`instruction pour tester diverses conditions. Par exemple, nous pouvons utiliser `jl`à la place de `jns`, ce qui sauterait tant que la destination est inférieure à la source. Ainsi, avec `cmp rbx, 10`, `rbx`commencera à moins de `10`, et une fois `rbx`qu'il sera supérieur à `10`, alors `rbx`(c'est-à-dire la destination) sera supérieur à `10`, auquel cas `jl`il ne sautera pas.

Remarque : Nous pouvons voir des instructions utilisant JMP Equal `je`ou JMP Not Equal `jne`. Ceci est juste un alias de `jz`and `jnz`, puisque si les deux opérandes sont égaux, le résultat de `cmp rax, rax`serait `0`dans tous les cas, ce qui définit le drapeau zéro. La même chose s'applique à `jge`et `jnl`, puisque `>=`c'est la même chose que `!`, et s'applique également à d'autres conditions similaires.

Maintenant que nous avons couvert toutes les instructions de contrôle de base, quelle méthode pensez-vous être la plus efficace ?

1.  Utiliser `mov rcx, 10`et `loop loopFib`=> boucler 10 fois
2.  Utiliser `mov rcx, 10`et `dec rcx`et `jnz loopFib`=> sauter 10 fois
3.  Utiliser `cmp rbx, 10`et `js loopFib`=> sauter pendant que rbx <10

Modifiez votre code pour utiliser la méthode qui vous semble la meilleure.
