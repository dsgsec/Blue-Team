Appels système
==============

* * * * *

Même si nous parlons directement au processeur via les instructions machine dans Assembly, nous n'avons pas besoin d'invoquer chaque type de commande en utilisant uniquement les instructions machine de base. Les programmes utilisent régulièrement de nombreux types d'opérations. Le système d'exploitation peut nous aider grâce aux appels système pour ne pas avoir à exécuter ces opérations manuellement à chaque fois.

Par exemple, supposons que nous devions écrire quelque chose à l'écran, sans appels système. Dans ce cas, nous devrons parler à la mémoire vidéo et aux E/S vidéo, résoudre tout encodage requis, envoyer notre entrée pour impression et attendre la confirmation qu'elle a été imprimée. Comme prévu, si nous devions faire tout cela pour imprimer un seul caractère, cela rendrait les codes d'assemblage beaucoup plus longs.

* * * * *

Appel système Linux
-------------------

A `syscall`est comme une fonction disponible mondialement écrite en `C`, fournie par le noyau du système d'exploitation. Un appel système prend les arguments requis dans les registres et exécute la fonction avec les arguments fournis. Par exemple, si nous voulons écrire quelque chose à l'écran, nous pouvons utiliser l' `write`appel système, fournir la chaîne à imprimer et d'autres arguments requis, puis appeler l'appel système pour émettre l'impression.

Il existe de nombreux appels système disponibles fournis par le noyau Linux, et nous pouvons en trouver une liste ainsi que les noms de `syscall number`chacun en lisant le `unistd_64.h`fichier système :

```
dsgsec@htb[/htb]$ cat /usr/include/x86_64-linux-gnu/asm/unistd_64.h
#ifndef _ASM_X86_UNISTD_64_H
#define _ASM_X86_UNISTD_64_H 1

#define __NR_read 0
#define __NR_write 1
#define __NR_open 2
#define __NR_close 3
#define __NR_stat 4
#define __NR_fstat 5

```

Le fichier ci-dessus définit le numéro d'appel système pour chaque appel système pour faire référence à cet appel système en utilisant ce numéro.

Remarque : Avec `32-bit`les processeurs x86, les numéros d'appel système sont dans le `unistd_32.h`fichier.

Pratiquons l'utilisation des appels système avec l' `write`appel système qui s'imprime à l'écran. Nous n'imprimerons pas encore les nombres de Fibonacci, mais commencerons plutôt par imprimer un message d'introduction, , `Fibonacci Sequence`au début de notre programme.

* * * * *

Arguments de la fonction Syscall
--------------------------------

Pour utiliser l' `write`appel système, nous devons d'abord savoir quels arguments il accepte. Pour trouver les arguments acceptés par un appel système, nous pouvons utiliser la `man -s 2`commande avec le nom de l'appel système dans la liste ci-dessus :

```
dsgsec@htb[/htb]$ man -s 2 write
...SNIP...
       ssize_t write(int fd, const void *buf, size_t count);

```

Comme nous pouvons le voir dans le résultat ci-dessus, la `write`fonction a la syntaxe suivante :

Code : c

```
ssize_t write(int fd, const void *buf, size_t count);

```

On voit que la fonction syscall attend `3`des arguments :

1.  Descripteur de fichier `fd`à imprimer ( *généralement `1`pour`stdout`* )
2.  Le pointeur d'adresse vers la chaîne à imprimer
3.  La longueur que nous voulons imprimer

Une fois que nous avons fourni ces arguments, nous pouvons utiliser l'instruction syscall pour exécuter la fonction et l'imprimer à l'écran. En plus de ces méthodes manuelles de localisation des appels système et des arguments de fonction, il existe des ressources en ligne que nous pouvons utiliser pour rechercher rapidement les appels système, leurs numéros et les arguments qu'ils attendent, comme [ce tableau](https://filippo.io/linux-syscall-table/) . De plus, on peut toujours se référer au `Linux`code source sur [Github](https://github.com/torvalds/linux/blob/master/arch/x86/entry/syscalls/syscall_64.tbl) .

Astuce : l' `-s 2`indicateur spécifie `syscall`les pages de manuel. Nous pouvons vérifier `man man`différentes sections pour chaque page de manuel.

* * * * *

Convention d'appel Syscall
--------------------------

Maintenant que nous comprenons comment localiser les différents appels système et leurs arguments, commençons à apprendre comment les appeler. Pour appeler un appel système, nous devons :

1.  Enregistrer les registres pour les empiler
2.  Définissez son numéro d'appel système dans`rax`
3.  Définir ses arguments dans les registres
4.  Utilisez l'instruction d'assemblage syscall pour l'appeler

`We usually should save any registers we use to the stack before any function call or syscall.`Cependant, comme nous exécutons cet appel système au début de notre programme avant d'utiliser des registres, nous n'avons aucune valeur dans les registres, nous ne devrions donc pas nous soucier de les sauvegarder.\
Nous discuterons de la sauvegarde des registres dans la pile lorsque nous arriverons à `Function Calls`.

#### Numéro d'appel système

Commençons par déplacer le numéro d'appel système vers le `rax`registre. Comme nous l'avons vu précédemment, l' `write`appel système a un numéro `1`, on peut donc commencer par la commande suivante :

Code : nasm

```
mov rax, 1

```

Maintenant, si nous atteignons l'instruction syscall, le noyau saura quel appel système nous appelons.

#### Arguments d'appel système

Ensuite, nous devons mettre chacun des arguments de la fonction dans son registre correspondant. La `x86_64`convention d'appel de l'architecture spécifie dans quel registre chaque argument doit être placé (par exemple, le premier argument doit être dans `rdi`). Toutes les fonctions et appels système doivent suivre cette norme et prendre leurs arguments dans les registres correspondants. Nous avons discuté du tableau suivant dans la `Registers`section :

| Description | Registre 64 bits | Registre 8 bits |
| --- | --- | --- |
| Numéro d'appel système/valeur de retour | `rax` | `al` |
| Appelé enregistré | `rbx` | `bl` |
| 1er argument | `rdi` | `dil` |
| 2ème argument | `rsi` | `sil` |
| 3ème argument | `rdx` | `cl` |
| 4ème argument | `rcx` | `bpl` |
| 5ème argument | `r8` | `r8b` |
| 6ème argument | `r9` | `r9b` |

Comme nous pouvons le voir, nous avons un registre pour chacun des premiers `6`arguments. Tous les arguments supplémentaires peuvent être stockés dans la pile (bien que peu d'appels système utilisent plus que `6`des arguments.).

Remarque : `rax`est également utilisé pour stocker le contenu `return value`d'un appel système ou d'une fonction. Donc, si nous nous attendions à récupérer une valeur d'un appel système/d'une fonction, ce sera dans `rax`.

Avec cela, nous devrions connaître nos arguments et dans quel registre nous devons les stocker. Pour en revenir à la `write`fonction syscall, nous devrions passer : `fd`, `pointer`, et `length`. Nous pouvons le faire de la manière suivante :

1.  `rdi`-> `1`(pour la sortie standard)
2.  `rsi`-> `'Fibonacci Sequence:\n'`(pointeur vers notre chaîne)
3.  `rdx`-> `20`(longueur de notre chaîne)

On peut utiliser `mov rcx, 'string'`. Cependant, nous ne pouvons stocker que jusqu'à 16 caractères dans un registre (c'est-à-dire 64 bits), donc notre chaîne d'introduction ne rentrerait pas. Au lieu de cela, créons une variable avec notre chaîne (comme nous l'avons appris dans la `Assembly File Structure`section), de la même manière que nous l'avons fait avec le `Hello World`programme :

Code : nasm

```
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a

```

Notez comment nous avons ajouté `0x0a`après notre chaîne, pour ajouter un nouveau caractère de ligne.

L' `message`étiquette est un pointeur vers l'endroit où notre chaîne sera stockée dans la mémoire. Nous pouvons donc l'utiliser comme deuxième argument. Ainsi, notre code d'appel système final devrait être le suivant :

Code : nasm

```
mov rax, 1       ; rax: syscall number 1
mov rdi, 1      ; rdi: fd 1 for stdout
mov rsi,message ; rsi: pointer to message
mov rdx, 20      ; rdx: print length of 20 bytes

```

Astuce : Si jamais nous avons besoin de créer un pointeur vers une valeur stockée dans un registre, nous pouvons simplement le pousser vers la pile, puis utiliser le `rsp`pointeur pour pointer vers elle.

Nous pouvons également utiliser une `length`variable calculée dynamiquement en utilisant `equ`, de la même manière que nous l'avons fait avec le `Hello World`programme.

* * * * *

Appeler un appel système
------------------------

Maintenant que nous avons notre numéro d'appel système et nos arguments en place, il ne reste plus qu'à exécuter l'instruction d'appel système. Ajoutons donc une instruction syscall et ajoutons les instructions au début de notre `fib.s`code, qui devrait ressembler à ceci :

Code : nasm

```
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a

section .text
_start:
    mov rax, 1       ; rax: syscall number 1
    mov rdi, 1      ; rdi: fd 1 for stdout
    mov rsi,message ; rsi: pointer to message
    mov rdx, 20      ; rdx: print length of 20 bytes
    syscall         ; call write syscall to the intro message
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0

```

Assemblons maintenant notre code et exécutons-le, et voyons si notre message d'introduction est imprimé :

  Arguments d'appel système

```
dsgsec@htb[/htb]$ ./assembler.sh fib.s

Fibonacci Sequence:
[1]    107348 segmentation fault  ./fib

```

On voit qu'en effet notre chaîne est imprimée à l'écran. Passons-le en revue `gdb`et arrêtons-nous à l'appel système pour voir comment tous les arguments sont configurés avant d'appeler syscall, comme suit :

  gdb
```
$ gdb -q ./fib gef➤   disas _start Dump du code assembleur pour la fonction _start : ..COUPER... 0x0000000000401011 <+17> : appel système
gef➤   b *_start+17 Point d'arrêt 1 à 0x401011
gef➤  r ────────────────────────────────────── ───────── ────────────────────────────────────── registres ─ ───
$rax   : 0x1 $rbx   : 0x0 $rcx   : 0x0 $rdx   : 0x14 $rsp   : 0x00007ffffffffe410 → 0x0000000000000001 $rbp   : 0x0 $rsi   : 0x0000000000402000 → "Séquence de Fibonacci :\n"
$rdi  : 0x1

gef➤   si
```
Séquence de Fibonacci :

Nous voyons quelques choses auxquelles nous nous attendions :

1.  Nos arguments sont correctement définis dans les registres correspondants avant chaque appel système.
2.  Un pointeur vers notre message est chargé dans `rsi`.

Maintenant, nous avons utilisé avec succès l' `write`appel système pour imprimer notre message d'introduction.

* * * * *

Quitter l'appel système
-----------------------

Enfin, puisque nous avons compris comment fonctionnent les appels système, passons en revue un autre appel système essentiel utilisé dans les programmes : `Exit syscall`. Nous avons peut-être remarqué que jusqu'à présent, chaque fois que notre programme termine son exécution, il se termine avec un `segmentation fault`, comme nous venons de le voir lorsque nous avons exécuté `./fib`. En effet, nous terminons notre programme brusquement, sans passer par la procédure appropriée pour quitter les programmes sous Linux, en appelant le `exit syscall`et en passant un code de sortie.

Alors ajoutons ceci à la fin de notre code. Tout d'abord, nous devons trouver le `exit syscall`numéro, comme suit :

  Arguments d'appel système

```
dsgsec@htb[/htb]$ grep exit /usr/include/x86_64-linux-gnu/asm/unistd_64.h

#define __NR_exit 60
#define __NR_exit_group 231

```

Nous devons utiliser le premier, avec un numéro d'appel système `60`. Voyons ensuite si le `exit syscall`besoin d'arguments :

  Arguments d'appel système

```
dsgsec@htb[/htb]$ man -s 2 exit

...SNIP...
void _exit(int status);

```

Nous voyons qu'il n'a besoin que d'un seul argument entier, `status`', qui est expliqué comme étant le code de sortie. Sous Linux, chaque fois qu'un programme se termine sans aucune erreur, il transmet un code de sortie de `0`. Sinon, le code de sortie est généralement un numéro différent `1`. Dans notre cas, comme tout s'est déroulé comme prévu, nous transmettrons le code de sortie de `0`. Notre `exit syscall`code devrait être le suivant :

Code : nasm

```
    mov rax, 60
    mov rdi, 0
    syscall

```

Maintenant, ajoutons-le à la fin de notre code :

Code : nasm

```
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a

section .text
_start:
    mov rax, 1       ; rax: syscall number 1
    mov rdi, 1      ; rdi: fd 1 for stdout
    mov rsi,message ; rsi: pointer to message
    mov rdx, 20      ; rdx: print length of 20 bytes
    syscall         ; call write syscall to the intro message
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0
    mov rax, 60
    mov rdi, 0
    syscall

```

Nous pouvons maintenant assembler notre code et le réexécuter :

  Arguments d'appel système

```
dsgsec@htb[/htb]$ ./assembler.sh fib.s

Fibonacci Sequence:

```

Super! Nous voyons que cette fois notre programme s'est terminé correctement sans `segmentation fault`. Nous pouvons vérifier le code de sortie qui a été transmis comme suit :

  Arguments d'appel système

```
dsgsec@htb[/htb]$ echo $?

0

```

Comme prévu, le code de sortie était `0`, comme nous l'avons spécifié dans notre appel système.

Pratique : Pour bien comprendre le fonctionnement des appels système, essayez d'implémenter l' `write`appel système pour imprimer le numéro de Fibonacci et placez-le après «`xchg rax, rbx` ».

Spoiler : ça ne marchera pas. Essayez de découvrir pourquoi et essayez de résoudre le problème pour imprimer les premiers nombres de Fibonacci ci-dessous `10`(indice : utilisez `ASCII`).
