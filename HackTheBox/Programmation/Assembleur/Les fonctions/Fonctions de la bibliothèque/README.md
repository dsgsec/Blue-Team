Fonctions de la bibliothèque
============================

* * * * *

Jusqu'à présent, nous n'avons imprimé que des nombres de Fibonacci inférieurs à `10`. Mais de cette façon, notre programme est statique et imprimera le même résultat à chaque fois. Pour le rendre plus dynamique, nous pouvons demander à l'utilisateur le nombre maximum de Fibonacci qu'il souhaite imprimer, puis l'utiliser avec `cmp`. Avant de commencer, rappelons la convention d'appel des fonctions :

1.  `Save Registers`sur la pile ( `Caller Saved`)
2.  Passer `Function Arguments`(comme les appels système)
3.  Réparer`Stack Alignment`
4.  Obtenir les fonctions' `Return Value`(dans `rax`)

Alors, importons notre fonction et commençons par les étapes de la convention d'appel.

* * * * *

Importation de fonctions libc
-----------------------------

Pour ce faire, nous pouvons utiliser la `scanf`fonction from `libc`pour prendre les entrées de l'utilisateur et les convertir correctement en un entier, que nous utiliserons plus tard avec `cmp`. Tout d'abord, nous devons importer le `scanf`, comme suit :

Code : nasm

```
global  _start
extern  printf, scanf

```

Nous pouvons maintenant commencer à écrire une nouvelle procédure, `getInput`afin de pouvoir l'appeler lorsque nous en avons besoin :

Code : nasm

```
getInput:
    ; call scanf

```

* * * * *

Sauvegarde des registres
------------------------

Comme nous sommes au début de notre programme et n'avons encore utilisé aucun registre, nous n'avons pas à nous soucier de la sauvegarde des registres dans la pile. Nous pouvons donc passer au deuxième point et transmettre les arguments requis à `scanf`.

* * * * *

Arguments de fonction
---------------------

Ensuite, nous devons savoir quels arguments sont acceptés par `scanf`, comme suit :

```
dsgsec@htb[/htb]$ man -s 3 scanf

...SNIP...
int scanf(const char *format, ...);

```

Nous voyons que, de la même manière que `printf`, `scanf`accepte un format d'entrée et le tampon dans lequel nous voulons enregistrer l'entrée utilisateur. Alors, ajoutons d'abord la `inFormat`variable :

Code : nasm

```
section .data
    message db "Please input max Fn", 0x0a
    outFormat db  "%d", 0x0a, 0x00
    inFormat db  "%d", 0x00

```

Nous avons également modifié notre message d'introduction de `Fibonacci Sequence:`à `Please input max Fn`, pour indiquer à l'utilisateur quelle entrée est attendue de sa part.

Ensuite, nous devons définir un espace tampon pour le stockage d'entrée. Comme nous l'avons mentionné dans la `Processor Architecture`section, l'espace tampon non initialisé doit être stocké dans le `.bss`segment mémoire. Ainsi, au début de notre code assembleur, nous devons l'ajouter sous l' `.bss`étiquette, et utiliser `resb 1`pour dire `nasm`de réserver 1 octet d'espace tampon, comme suit :

Code : nasm

```
section .bss
    userInput resb 1

```

Nous pouvons maintenant définir les arguments de notre fonction sous notre `getInput`procédure :

Code : nasm

```
getInput:
    mov rdi, inFormat   ; set 1st parameter (inFormat)
    mov rsi, userInput  ; set 2nd parameter (userInput)

```

* * * * *

Alignement de la pile
---------------------

Ensuite, nous devons nous assurer qu'une limite de 16 octets aligne notre pile. Nous sommes actuellement à l'intérieur de la `getInput`procédure, nous avons donc 1 `call`instruction et aucune `push`instruction, nous avons donc une `8-byte`limite. Nous pouvons donc utiliser `sub`pour corriger `rsp`, comme suit :

Code : nasm

```
getInput:
    sub rsp, 8
    ; call scanf
    add rsp, 8

```

Nous pouvons `push rax`le faire à la place, et cela alignera également correctement la pile. De cette façon, notre pile devrait être parfaitement alignée sur une limite de 16 octets.

* * * * *

Appel de fonction
-----------------

Maintenant, nous définissons les arguments de la fonction et `call scanf`, comme suit :

Code : nasm

```
getInput:
    sub rsp, 8          ; align stack to 16-bytes
    mov rdi, inFormat   ; set 1st parameter (inFormat)
    mov rsi, userInput  ; set 2nd parameter (userInput)
    call scanf          ; scanf(inFormat, userInput)
    add rsp, 8          ; restore stack alignment
    ret

```

Nous ajouterons également `call getInput`at `_start`, afin de passer à cette procédure juste après l'impression du message d'introduction, comme suit :

Code : nasm

```
section .text
_start:
    call printMessage   ; print intro message
    call getInput       ; get max number
    call initFib        ; set initial Fib values
    call loopFib        ; calculate Fib numbers
    call Exit           ; Exit the program

```

Enfin, nous devons utiliser les entrées de l'utilisateur. Pour ce faire, au lieu d'utiliser un static `10`lors de la comparaison dans `cmp rbx, 10`, nous le remplacerons par `cmp rbx, [userInput]`, comme suit :

Code : nasm

```
loopFib:
    ...SNIP...
    cmp rbx,[userInput] ; do rbx - userInput
    js loopFib		    ; jump if result is <0
    ret

```

Remarque : Nous avons utilisé `[userInput]`à la place de `userInput`, car nous voulions comparer avec la valeur finale, et non avec l'adresse du pointeur.

Une fois tout cela fait, notre code final complet devrait ressembler à ceci :

Code : nasm

```
global  _start
extern  printf, scanf

section .data
    message db "Please input max Fn", 0x0a
    outFormat db  "%d", 0x0a, 0x00
    inFormat db  "%d", 0x00

section .bss
    userInput resb 1

section .text
_start:
    call printMessage   ; print intro message
    call getInput       ; get max number
    call initFib        ; set initial Fib values
    call loopFib        ; calculate Fib numbers
    call Exit           ; Exit the program

printMessage:
    ...SNIP...

getInput:
    sub rsp, 8          ; align stack to 16-bytes
    mov rdi, inFormat   ; set 1st parameter (inFormat)
    mov rsi, userInput  ; set 2nd parameter (userInput)
    call scanf          ; scanf(inFormat, userInput)
    add rsp, 8          ; restore stack alignment
    ret

initFib:
    ...SNIP...

printFib:
    ...SNIP...

loopFib:
    ...SNIP...
    cmp rbx,[userInput] ; do rbx - userInput
    js loopFib		    ; jump if result is <0
    ret

Exit:
    ...SNIP...

```

* * * * *

Editeur de liens dynamique
--------------------------

Assemblons notre code, lions-le et essayons d'imprimer des nombres de Fibonacci jusqu'à`100` :

```
dsgsec@htb[/htb]$ nasm -f elf64 fib.s &&  ld fib.o -o fib -lc --dynamic-linker /lib64/ld-linux-x86-64.so.2 && ./fib

Please input max Fn:
100
1
1
2
3
5
8
13
21
34
55
89

```

Nous voyons que notre code a fonctionné comme prévu et a imprimé des nombres de Fibonacci inférieurs au nombre que nous avons spécifié. Avec cela, nous pouvons terminer notre projet de module et créer un programme qui calcule et imprime les nombres de Fibonacci en fonction d'une entrée que nous fournissons, en utilisant uniquement l'assemblage.

En outre, nous devons apprendre à transformer le code Assembly en shellcode machine, que nous pouvons ensuite utiliser directement dans nos charges utiles dans Binary Exploitation.
