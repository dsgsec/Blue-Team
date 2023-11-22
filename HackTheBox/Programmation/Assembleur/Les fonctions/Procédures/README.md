Procédures
==========

* * * * *

À mesure que notre code devient de plus en plus complexe, nous devons commencer à le refactoriser pour utiliser plus efficacement les instructions et le rendre plus facile à lire et à comprendre. Une manière courante de le faire consiste à utiliser `functions`et `procedures`. Bien que les fonctions nécessitent une procédure d'appel pour les appeler et transmettre leurs arguments (comme nous le verrons dans la section suivante), elles `procedures`sont généralement plus simples et principalement utilisées pour la refactorisation du code.

A `procedure`(parfois appelé a `subroutine`) est généralement un ensemble d'instructions que nous souhaitons exécuter à des moments spécifiques du programme. Ainsi, au lieu de réutiliser le même code, nous le définissons sous une étiquette de procédure et `call`ce, chaque fois que nous en avons besoin. De cette façon, nous n'avons besoin d'écrire le code qu'une seule fois mais pouvons l'utiliser plusieurs fois. De plus, nous pouvons utiliser des procédures pour diviser un code plus grand et plus complexe en segments plus petits et plus simples.

Revenons à notre code :

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

Nous voyons que nous faisons maintenant plusieurs choses dans un gros morceau de code :

1.  Impression du message d'introduction
2.  Définition des valeurs initiales de Fibonacci sur `0`et`1`
3.  Utiliser une boucle pour calculer le nombre de Fibonacci suivant
4.  Quitter le programme

Notre boucle est déjà définie sous une étiquette, nous pouvons donc l'appeler quand nous en avons besoin. Cependant, les trois autres parties du code peuvent être refactorisées sous forme de procédures pour les appeler chaque fois que nous en avons besoin, augmentant ainsi l'efficacité du code.

* * * * *

Définir des procédures
----------------------

Pour commencer, ajoutons un label au-dessus de chacune des trois parties du code que nous souhaitons transformer en procédures :

Code : nasm

```
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a

section .text
_start:

printMessage:
    mov rax, 1       ; rax: syscall number 1
    mov rdi, 1      ; rdi: fd 1 for stdout
    mov rsi,message ; rsi: pointer to message
    mov rdx, 20      ; rdx: print length of 20 bytes
    syscall         ; call write syscall to the intro message

initFib:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1

loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0

Exit:
    mov rax, 60
    mov rdi, 0
    syscall

```

Nous voyons que notre code est déjà meilleur. Cependant, cela n'est pas plus efficace qu'il ne l'était, car nous aurions pu obtenir le même résultat en utilisant des commentaires. Notre prochaine étape consiste donc à utiliser `calls`pour diriger le programme vers chacune de nos procédures.

* * * * *

APPEL/RET
---------

Lorsque nous voulons commencer à exécuter une procédure, nous pouvons `call`le faire et il suivra ses instructions. L' `call`instruction pousse (c'est-à-dire enregistre) le pointeur d'instruction suivant `rip`vers la pile, puis passe à la procédure spécifiée.

Une fois la procédure exécutée, nous devons la terminer par une `ret`instruction pour revenir au point où nous en étions avant de passer à la procédure. L' `ret`instruction adresse `pops`l'adresse en haut de la pile dans `rip`, de sorte que l'instruction suivante du programme soit restaurée à ce qu'elle était avant de passer à la procédure.

L' `ret`instruction joue un rôle essentiel dans [la programmation orientée retour (ROP)](https://en.wikipedia.org/wiki/Return-oriented_programming) , une technique d'exploitation habituellement utilisée avec l'exploitation binaire.

| Instruction | Description | Exemple |
| --- | --- | --- |
| `call` | poussez le pointeur d'instruction suivante `rip`vers la pile, puis passez à la procédure spécifiée | `call printMessage` |
| `ret` | insérez l'adresse `rsp`dans `rip`, puis accédez-y | `ret` |

Donc avec ça, nous pouvons configurer nos appels au début de notre code pour définir le flux d'exécution que nous voulons :

Code : nasm

```
global  _start

section .data
    message db "Fibonacci Sequence:", 0x0a

section .text
_start:
    call printMessage   ; print intro message
    call initFib        ; set initial Fib values
    call loopFib        ; calculate Fib numbers
    call Exit           ; Exit the program

printMessage:
    mov rax, 1      ; rax: syscall number 1
    mov rdi, 1      ; rdi: fd 1 for stdout
    mov rsi,message ; rsi: pointer to message
    mov rdx, 20     ; rdx: print length of 20 bytes
    syscall         ; call write syscall to the intro message
    ret

initFib:
    xor rax, rax    ; initialize rax to 0
    xor rbx, rbx    ; initialize rbx to 0
    inc rbx         ; increment rbx to 1
    ret

loopFib:
    add rax, rbx    ; get the next number
    xchg rax, rbx   ; swap values
    cmp rbx, 10		; do rbx - 10
    js loopFib		; jump if result is <0
    ret

Exit:
    mov rax, 60
    mov rdi, 0
    syscall

```

De cette façon, notre code devrait exécuter les mêmes instructions qu'auparavant tout en ayant notre code plus propre et plus efficace. Désormais, si nous devons éditer une procédure spécifique, nous n'aurons plus besoin d'afficher l'intégralité du code, mais seulement cette procédure. Nous pouvons également constater que nous ne l'avons pas utilisé `ret`dans notre `Exit`procédure, car nous ne voulons pas revenir là où nous étions. Nous voulons quitter le code. Nous utiliserons presque toujours un `ret`, et la `Exit`fonction est l'une des rares exceptions.

Remarque : Il est important de comprendre le flux d'exécution de l'assemblage basé sur les lignes. Si nous n'utilisons pas a `ret`à la fin d'une procédure, il exécutera simplement la ligne suivante. De même, si nous étions revenus à la fin de notre `Exit`fonction, nous reviendrions simplement en arrière et exécuterions la ligne suivante, qui serait la première ligne de `printMessage`.

Enfin, il convient également de mentionner les instructions `enter`et `leave`, qui sont parfois utilisées avec les procédures pour sauvegarder et restaurer les adresses de `rsp`et `rbp`et allouer un espace de pile spécifique à utiliser par la procédure. Nous n'aurons cependant pas besoin de les utiliser dans ce module.
