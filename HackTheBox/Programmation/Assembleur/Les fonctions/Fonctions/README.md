Les fonctions
=============

* * * * *

Nous devons maintenant comprendre les différentes instructions de branchement et de contrôle utilisées pour contrôler le flux d'exécution du programme. Nous devons également bien comprendre les procédures et les appels et savoir comment les utiliser pour le branchement. Alors concentrons-nous maintenant sur l'appel des fonctions.

* * * * *

Convention d'appel de fonctions
-------------------------------

Les fonctions sont une forme de `procedures`. Cependant, les fonctions ont tendance à être plus complexes et devraient utiliser pleinement la pile et tous les registres. Nous ne pouvons donc pas simplement appeler une fonction comme nous l'avons fait avec les procédures. Au lieu de cela, les fonctions doivent `Calling Convention`être correctement configurées avant d'être appelées.

Il y a quatre éléments principaux à prendre en compte avant d'appeler une fonction :

1.  `Save Registers`sur la pile ( `Caller Saved`)
2.  Passer `Function Arguments`(comme les appels système)
3.  Réparer`Stack Alignment`
4.  Obtenir les fonctions `Return Value`(dans `rax`)

C'est relativement similaire à l'appel d'un appel système, et la seule différence avec les appels système est que nous devons stocker le numéro de l'appel système dans `rax`, alors que nous pouvons appeler des fonctions directement avec `call function`. De plus, avec syscall, nous n'avons pas à nous soucier des fichiers `Stack Alignment`.

#### Fonctions d'écriture

Tous les points ci-dessus concernent un `caller`point de vue, ce que nous appelons une fonction. Lorsqu'il s'agit d'écrire une fonction, il y a différents points à considérer, qui sont :

1.  Sauvegarde `Callee Saved`des registres ( `rbx`et `rbp`)
2.  Récupérer les arguments des registres
3.  Aligner la pile
4.  Valeur de retour dans`rax`

Comme on peut le constater, ces points sont relativement similaires aux `caller`points. Le `caller`système met en place des éléments, puis le `callee`(c'est-à-dire le récepteur) doit récupérer ces éléments et les utiliser. Ces points sont généralement faits au début et à la fin de la fonction et sont appelés `prologue`et d'une fonction `epilogue`. Ils permettent d'appeler des fonctions sans se soucier de l'état actuel de la pile ou des registres.

`In this module, we will only be calling other functions,`nous devons donc nous concentrer uniquement sur la configuration d'un appel de fonction et n'entrerons pas dans l'écriture de fonctions.

* * * * *

Utilisation de fonctions externes
---------------------------------

Nous voulons imprimer le nombre de Fibonacci actuel à chaque itération de la `loopFib`boucle. Auparavant, nous ne pouvions pas utiliser un `write`appel système puisqu'il n'accepte que `ASCII`des caractères. Nous aurions dû convertir notre nombre de Fibonacci en `ASCII`, ce qui est un peu compliqué.

Heureusement, il existe des fonctions externes que nous pouvons utiliser pour imprimer le numéro actuel sans avoir à le convertir. La `libc`bibliothèque de fonctions utilisée pour `C`les programmes fournit de nombreuses fonctionnalités que nous pouvons utiliser sans tout réécrire à partir de zéro. La `printf`fonction `libc`accepte le format d'impression, nous pouvons donc lui transmettre le nombre de Fibonacci actuel et lui dire de l'imprimer sous forme d'entier, et elle effectuera la conversion automatiquement. Avant de pouvoir utiliser une fonction de `libc`, nous devons d'abord l'importer, puis spécifier la `libc`bibliothèque pour la liaison dynamique lors de la liaison de notre code avec `ld`.

* * * * *

Importation de fonctions libc
-----------------------------

Tout d'abord, pour importer une `libc`fonction externe, nous pouvons utiliser l' `extern`instruction au début de notre code, comme suit :

Code : nasm

```
global  _start
extern  printf

```

Une fois cela fait, nous devrions pouvoir appeler la `printf`fonction. Nous pouvons donc procéder avec ce dont `Functions Calling Convention`nous avons discuté plus tôt.

* * * * *

Sauvegarde des registres
------------------------

Définissons une nouvelle procédure, `printFib`, pour contenir nos instructions d'appel de fonction. La toute première étape consiste à enregistrer dans la pile tous les registres que nous utilisons, qui sont `rax`et `rbx`, comme suit :

Code : nasm

```
printFib:
    push rax        ; push registers to stack
    push rbx
    ; function call
    pop rbx         ; restore registers from stack
    pop rax
    ret

```

Nous pouvons donc passer au deuxième point et transmettre les arguments requis à `printf`.

* * * * *

Arguments de fonction
---------------------

Nous avons déjà expliqué comment transmettre les arguments de la fonction dans la section syscall. Le même processus s'applique aux arguments de fonction.

Tout d'abord, nous devons découvrir quels arguments sont acceptés par la `printf`fonction en utilisant `man -s 3`for `library functions manual`(comme nous pouvons le voir dans `man man`) :

  Fonctions d'écriture

```
dsgsec@htb[/htb]$ man -s 3 printf

...SNIP...
       int printf(const char *format, ...);

```

Comme nous pouvons le voir, la fonction prend un pointeur vers le format d'impression (indiqué par un `*`), puis vers la ou les chaînes à imprimer.

Tout d'abord, nous pouvons créer une variable contenant le format de sortie pour la transmettre comme premier argument. La `printf`page de manuel détaille également différents formats d'impression. Nous voulons imprimer un entier, nous pouvons donc utiliser le `%d`format suivant :

Code : nasm

```
global  _start
extern  printf

section .data
    message db "Fibonacci Sequence:", 0x0a
    outFormat db  "%d", 0x0a, 0x00

```

Remarque : Nous avons terminé le format par un caractère nul `0x00`, car il s'agit du caractère de fin de chaîne dans `printf`, et nous devons terminer n'importe quelle chaîne par ce caractère.

Cela peut être notre premier argument, et `rbx`notre deuxième argument, qui `printf`sera placé comme `%d`. Déplaçons donc les deux arguments vers leurs registres respectifs, comme suit :

Code : nasm

```
printFib:
    push rax            ; push registers to stack
    push rbx
    mov rdi, outFormat  ; set 1st argument (Print Format)
    mov rsi, rbx        ; set 2nd argument (Fib Number)
    pop rbx             ; restore registers from stack
    pop rax
    ret

```

* * * * *

Alignement de la pile
---------------------

Chaque fois que nous voulons créer `call`une fonction, nous devons nous assurer que la `Top Stack Pointer (rsp)`est alignée sur la `16-byte`limite de la `_start`pile de fonctions.

Cela signifie que nous devons pousser au moins 16 octets (ou un multiple de 16 octets) dans la pile avant de passer un appel pour garantir que les fonctions disposent de suffisamment d'espace sur la pile pour s'exécuter correctement. Cette exigence concerne principalement l'efficacité des performances du processeur. Certaines fonctions (comme dans `libc`) sont programmées pour planter si cette limite n'est pas fixée pour garantir l'efficacité des performances. Si nous assemblons notre code et le cassons juste après le second `push`, voici ce que nous verrons :

  gdb
```
──────────────────────────────────────── ────────── ────────────────────────────────── ... ────
0x00007fffffffe3a0 │+0x0000 : 0x0000000000000001 ← $rsp
0x00007fffffffe3a8 │+0x0008 : 0x0000000000000000 0x00007fffffffe3b0 │+0x0010 : 0x00000000004010ad → <loopFib+5> ajouter rax, rbx
0x00007 fffffffe3b8 │+0x0018 : 0x0000000000401044 → <_start+20> appeler 0x4010bd <Sortie>
0x00007fffffffe3c0 │+0x0020 : 0x0000000000000001 ← $r13
───────────────────────────────────── ────────── ──────────────────────────────────── code:x86:64 ─ ───
   0x401090 <initFib+9 > ret
   0x401091 <printFib+0> push rax
   0x401092 <printFib+1> push rbx
 → 0x40100e <printFib+2> movabs rdi, 0x403039
```

Nous voyons que nous avons quatre 8 octets poussés vers la pile, soit une limite totale de 32 octets. Cela est dû à deux choses :

1.  Chaque procédure `call`ajoute une adresse de 8 octets à la pile, qui est ensuite supprimée avec`ret`
2.  Chacun `push`ajoute également 8 octets à la pile

Nous sommes donc à l'intérieur `printFib`et à l'intérieur de `loopFib`, et avons poussé `rax`et `rbx`, pour un total d'une limite de 32 octets. Puisque la frontière est un multiple de 16,`our stack is already aligned, and we don't have to fix anything.`

Si nous étions dans un cas où nous voulions porter la limite à 16, nous pouvons soustraire des octets de `rsp`, comme suit :

Code : nasm

```
    sub rsp, 16
    call function
    add rsp, 16

```

De cette façon, nous ajoutons 16 octets supplémentaires en haut de la pile, puis nous les supprimons après l'appel. Si nous avions poussé 8 octets, nous pouvons amener la limite à 16 en soustrayant 8 de `rsp`.

Cela peut être un peu déroutant, mais la chose essentielle à retenir est que `we should have 16-bytes (or a multiple of 16) on top of the stack before making a call.`nous pouvons compter le nombre d'instructions (non `pop`éditées) `push`et d'instructions (non `ret`exécutées) `call`, et nous obtiendrons combien de 8 octets ont été poussés vers la pile.

* * * * *

Appel de fonction
-----------------

Enfin, nous pouvons émettre `call printf`, et il devrait imprimer le nombre de Fibonacci actuel dans le format que nous avons spécifié, comme suit :

Code : nasm

```
printFib:
    push rax            ; push registers to stack
    push rbx
    mov rdi, outFormat  ; set 1st argument (Print Format)
    mov rsi, rbx        ; set 2nd argument (Fib Number)
    call printf         ; printf(outFormat, rbx)
    pop rbx             ; restore registers from stack
    pop rax
    ret

```

Nous devrions maintenant avoir notre `printFib`procédure prête. Ainsi, nous pouvons l'ajouter au début de `loopFib`, de telle sorte qu'il imprime le nombre de Fibonacci actuel au début de chaque boucle :

Code : nasm

```
loopFib:
    call printFib   ; print current Fib number
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0
    ret

```

Notre `fib.s`code final devrait être le suivant :

Code : nasm

```
global  _start
extern  printf

section .data
    message db "Fibonacci Sequence:", 0x0a
    outFormat db  "%d", 0x0a, 0x00

section .text
_start:
    call printMessage   ; print intro message
    call initFib        ; set initial Fib values
    call loopFib        ; calculate Fib numbers
    call Exit           ; Exit the program

printMessage:
    mov rax, 1           ; rax: syscall number 1
    mov rdi, 1          ; rdi: fd 1 for stdout
    mov rsi, message    ; rsi: pointer to message
    mov rdx, 20          ; rdx: print length of 20 bytes
    syscall             ; call write syscall to the intro message
    ret

initFib:
    xor rax, rax        ; initialize rax to 0
    xor rbx, rbx        ; initialize rbx to 0
    inc rbx             ; increment rbx to 1
    ret

printFib:
    push rax            ; push registers to stack
    push rbx
    mov rdi, outFormat  ; set 1st argument (Print Format)
    mov rsi, rbx        ; set 2nd argument (Fib Number)
    call printf         ; printf(outFormat, rbx)
    pop rbx             ; restore registers from stack
    pop rax
    ret

loopFib:
    call printFib       ; print current Fib number
    add rax, rbx        ; get the next number
    xchg rax, rbx       ; swap values
    cmp rbx, 10		    ; do rbx - 10
    js loopFib		    ; jump if result is <0
    ret

Exit:
    mov rax, 60
    mov rdi, 0
    syscall

```

* * * * *

Editeur de liens dynamique
--------------------------

Nous pouvons maintenant assembler notre code avec `nasm`. Lorsque nous lions notre code avec `ld`, nous devons lui dire d'effectuer une liaison dynamique avec la `libc`bibliothèque. Sinon, il ne saurait pas comment récupérer la `printf`fonction importée. Nous pouvons le faire avec les `-lc --dynamic-linker /lib64/ld-linux-x86-64.so.2`drapeaux, comme suit :

  Fonctions d'écriture

```
dsgsec@htb[/htb]$ nasm -f elf64 fib.s &&  ld fib.o -o fib -lc --dynamic-linker /lib64/ld-linux-x86-64.so.2 && ./fib

1
1
2
3
5
8

```

Comme nous pouvons le voir, `printf`il a été très facile d'imprimer notre numéro de Fibonacci sans se soucier de le convertir au format approprié, comme nous avons dû le faire avec l' `write`appel système. Ensuite, nous devons passer par un autre exemple d'utilisation `libc`de fonctions externes pour comprendre comment appeler correctement des fonctions externes.
