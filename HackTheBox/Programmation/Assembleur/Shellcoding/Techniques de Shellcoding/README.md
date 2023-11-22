Techniques de Shellcoding
=============================

* * * * *

Comme nous l'avons vu dans la section précédente, notre `Hello World`code assembleur a dû être modifié pour produire un shellcode fonctionnel. Ainsi, dans cette section, nous passerons en revue certaines des techniques et astuces que nous pouvons utiliser pour contourner les problèmes rencontrés dans notre code assembleur.

* * * * *

Exigences de codage shell
-------------------------

Comme nous l'avons brièvement mentionné dans la section précédente, tous les binaires ne fournissent pas de shellcodes fonctionnels qui peuvent être chargés directement dans la mémoire et exécutés. En effet, un shellcode doit répondre à des exigences spécifiques. Sinon, il ne sera pas correctement démonté lors de l'exécution dans ses instructions d'assemblage correctes.

Pour mieux comprendre cela, essayons de démonter le shellcode que nous avons extrait du `Hello World`programme dans la section précédente, en utilisant le même `pwn disasm`outil que nous avons utilisé précédemment :

  pwn

$ pwn disasm '48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05' -c 'amd64' 0 : 48 être 00 20 40 00  00      movabs  rsi , 0x402000 7 : 00  00  00 a : bf 01 00  00  00            mov     edi , 0x1 f : ba 12 00  00  00            mov     edx , 0x12 14 : b8 01 00  00  0 0            mouvement     par axe , 0x1 19 : 0f 05 appel système 1b : b8 3c 00  00  00            mov     eax , 0x3c 20 : bf 00  00  00  00            mov     edi , 0x0 25 : 0f 05 appel système

Nous pouvons voir que les instructions sont relativement similaires au `Hello World`code assembleur que nous avions auparavant, mais elles ne sont pas identiques. Nous voyons qu'il y a une ligne d'instructions vide, ce qui pourrait potentiellement casser le code. De plus, notre `Hello World`chaîne est introuvable. Nous voyons également de nombreux rouges `00`, que nous aborderons dans un instant.

C'est ce qui se passera si notre code assembleur n'est pas `shellcode compliant`et ne répond pas aux exigences `Shellcoding Requirements`. Pour pouvoir produire un shellcode fonctionnel, `Shellcoding Requirements`notre code assembleur doit respecter trois critères principaux :

1.  Ne contient pas de variables
2.  Ne fait pas référence aux adresses mémoire directes
3.  Ne contient aucun octet NULL`00`

Commençons donc par le `Hello World`programme que nous avons vu dans la section précédente, passons en revue chacun des points ci-dessus et corrigeons-les :

Code : nasm

```
global _start

section .data
    message db "Hello HTB Academy!"

section .text
_start:
    mov rsi, message
    mov rdi, 1
    mov rdx, 18
    mov rax, 1
    syscall

    mov rax, 60
    mov rdi, 0
    syscall

```

* * * * *

Supprimer des variables
-----------------------

Un shellcode devrait être directement exécutable une fois chargé en mémoire, sans charger de données provenant d'autres segments de mémoire, comme `.data`ou `.bss`. En effet, les `text`segments de mémoire ne le sont pas `writable`, nous ne pouvons donc écrire aucune variable. En revanche, le `data`segment n'est pas exécutable, nous ne pouvons donc pas écrire de code exécutable.

Ainsi, pour exécuter notre shellcode, nous devons le charger dans le `text`segment mémoire et perdre la possibilité d'écrire des variables.`Hence, our entire shellcode must be under '.text' in the assembly code.`

Remarque : Certaines techniques de shellcoding plus anciennes (comme la technique jmp-call-pop) ne fonctionnent plus avec les protections de mémoire modernes, car beaucoup d'entre elles reposent sur l'écriture de variables dans le segment de mémoire, ce qui, comme nous venons de le dire, n'est plus `text`possible.

Il existe de nombreuses techniques que nous pouvons utiliser pour éviter d'utiliser des variables, telles que :

1.  Déplacement des chaînes immédiates vers les registres
2.  Pousser des chaînes vers la pile, puis les utiliser

Dans le code ci-dessus, nous pouvons déplacer notre chaîne vers `rsi`, comme suit :

Code : nasm

```
    mov rsi, 'Academy!'

```

Cependant, un registre de 64 bits ne peut contenir que 8 octets, ce qui peut ne pas suffire pour des chaînes plus grandes. Ainsi, notre autre option consiste à s'appuyer sur la pile en poussant notre chaîne de 16 octets à la fois (dans l'ordre inverse), puis en l'utilisant `rsp`comme pointeur de chaîne, comme suit :

Code : nasm

```
    push 'y!'
    push 'B Academ'
    push 'Hello HT'
    mov rsi, rsp

```

Cependant, cela dépasserait les limites autorisées des chaînes immédiates `push`, qui correspondent à un `dword`(4 octets) à la fois. Nous allons donc plutôt déplacer notre chaîne vers `rbx`, puis la pousser `rbx`vers la pile, comme suit :

Code : nasm

```
    mov rbx, 'y!'
    push rbx
    mov rbx, 'B Academ'
    push rbx
    mov rbx, 'Hello HT'
    push rbx
    mov rsi, rsp

```

Remarque : chaque fois que nous poussons une chaîne vers la pile, nous devons pousser un `00`avant pour terminer la chaîne. Cependant, nous n'avons pas à nous en soucier dans ce cas, puisque nous pouvons spécifier la longueur d'impression de l' `write`appel système.

Nous pouvons maintenant appliquer ces modifications à notre code, l'assembler et l'exécuter pour voir s'il fonctionne :

```
dsgsec@htb[/htb]$ ./assembler.sh helloworld.s

Hello HTB Academy!

```

Nous voyons que cela fonctionne comme prévu, sans avoir besoin d'utiliser de variables. Nous pouvons le vérifier avec `gdb`pour voir à quoi il ressemble au point d'arrêt :

  gdb

$ gdb -q ./helloworld ──────────────────────────────────── ─────── ──────────────────────────────────────── ──── enregistre ────
$rax : 0x1 $rbx   : 0x5448206f6c6c6548 (" Bonjour HT " ?) $rcx   : 0x0 $rdx : 0x12 $rsp   : 0x00007fffffffe3b8 → "Bonjour HTB Academy !"
$rbp : 0x0 $rsi   : 0x00007fffffffe3b8 → "Bonjour HTB Academy !"
$rdi   : 0x1 ───────────────────────────────────── ───────── ──────────────────────────────────────── ───── pile ────
0x00007fffffffe3b8 │+0x0000 : "Bonjour HTB Academy !" ← $rsp, $rsi
0x00007ffffffffe3c0 │+0x0008 : "B Académie !"
0x00007fffffffe3c8 │+0x0010 : 0x0000000000002179 (" y ! " ?) ─────────────────────── ────────────── ──────────────────────────────────────── ──────── code : x86:64 ────
→ 0x40102e <_start+46> appel système
─────────────────────────── ──────── ──────────────────────────────────────── ────────── ─────────────────

Comme nous pouvons le constater, la chaîne a été construite progressivement dans la pile et, lorsque nous `rsp`y sommes passés `rsi`, elle contenait l'intégralité de notre chaîne.

* * * * *

Supprimer des adresses
----------------------

Nous n'utilisons désormais aucune adresse dans notre code ci-dessus puisque nous avons supprimé la seule référence d'adresse lorsque nous avons supprimé notre seule variable. Cependant, nous pouvons voir des références dans de nombreux cas, notamment avec `calls`ou `loops`et tel. Nous devons donc nous assurer que notre shellcode saura comment effectuer l'appel quel que soit l'environnement dans lequel il s'exécute.

Pour pouvoir le faire, nous ne pouvons pas référencer une adresse mémoire directe (c'est-à-dire `call 0xffffffffaa8a25ff`), et à la place, nous effectuons uniquement des appels à des étiquettes (c'est-à-dire `call loopFib`) ou à des adresses mémoire relatives (c'est-à-dire `call 0x401020`). Nous avons discuté de l'adressage relatif à la déchirure dans la `gdb`section.

Heureusement, tout au long de ce module, nous avons uniquement créé `calls`des étiquettes pour nous assurer que nous apprenons à écrire du code facilement codé en shell. Si nous créons `call`une étiquette, `nasm`cela changera automatiquement cette étiquette en une adresse relative, qui devrait fonctionner avec les shellcodes.

Si nous avons déjà eu des appels ou des références à des adresses mémoire directes, nous pouvons résoudre ce problème en :

1.  Remplacement par des appels à des étiquettes ou à des adresses relatives à l'extraction (pour `calls`et `loops`)
2.  Poussez vers la pile et utilisez `rsp`-le comme adresse (pour `mov`et d'autres instructions d'assemblage)

Si nous sommes efficaces lors de l'écriture de notre code assembleur, nous n'aurons peut-être pas à résoudre ce type de problèmes.

* * * * *

Supprimer NULL
--------------

Les caractères NULL (ou `0x00`) sont utilisés comme terminateurs de chaîne dans le code assembleur et machine. Ainsi, s'ils sont rencontrés, ils entraîneront des problèmes et pourront conduire le programme à se terminer prématurément. Il faut donc s'assurer que notre shellcode ne contient aucun octet NULL `00`. Si nous revenons au `Hello World`démontage de notre shellcode, nous avons remarqué beaucoup de rouge `00`dedans :

  pwn

$ pwn disasm '48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05' -c 'amd64' 0 : 48 être 00 20 40 00  00      movabs  rsi , 0x402000 7 : 00  00  00 a : bf 01 00  00  00            mov     edi , 0x1 f : ba 12 00  00  00            mov     edx , 0x12 14 : b8 01 00  00  0 0            mouvement     par axe , 0x1 19 : 0f 05 appel système 1b : b8 3c 00  00  00            mov     eax , 0x3c 20 : bf 00  00  00  00            mov     edi , 0x0 25 : 0f 05 appel système

Cela se produit généralement lors du déplacement d'un petit entier dans un grand registre, de sorte que l'entier est complété par un supplément `00`pour s'adapter à la taille du plus grand registre.

Par exemple, dans notre code ci-dessus, lorsque nous utilisons `mov rax, 1`, il se déplacera `00 00 00 01`vers `rax`, de telle sorte que la taille du nombre corresponde à la taille du registre. Nous pouvons le voir lorsque nous assemblons l'instruction ci-dessus :

```
dsgsec@htb[/htb]$ pwn asm 'mov rax, 1' -c 'amd64'

48c7c001000000

```

Pour éviter d'avoir ces octets NULL, `we must use registers that match our data size.`pour l'exemple précédent, nous pouvons utiliser l'instruction la plus efficace `mov al, 1`, comme nous l'avons appris tout au long du module. Cependant, avant de le faire, nous devons d'abord mettre à zéro le `rax`registre avec `xor rax, rax`, pour garantir que nos données ne soient pas mélangées avec des données plus anciennes. Voyons le shellcode de ces deux instructions :

```
dsgsec@htb[/htb]$ pwn asm 'xor rax, rax' -c 'amd64'

4831c0
$ pwn asm 'mov al, 1' -c 'amd64'

b001

```

Comme nous pouvons le voir, non seulement notre nouveau shellcode ne contient aucun octet NULL, mais il est également plus court, ce qui est très recherché dans les shellcodes.

Nous pouvons commencer avec la nouvelle instruction que nous avons ajoutée plus tôt, `mov rbx, 'y!'`. Nous voyons que cette instruction déplace 2 octets dans un registre de 8 octets. Donc, pour résoudre ce problème, nous allons d'abord mettre à zéro `rbx`, puis utiliser le registre à 2 octets (c'est-à-dire 16 bits) `bx`, comme suit :

Code : nasm

```
    xor rbx, rbx
    mov bx, 'y!'

```

Ces nouvelles instructions ne doivent contenir aucun octet NULL dans leur shellcode. Appliquons la même chose au reste de notre code, comme suit :

Code : nasm

```
    xor rax, rax
    mov al, 1
    xor rdi, rdi
    mov dil, 1
    xor rdx, rdx
    mov dl, 18
    syscall

    xor rax, rax
    add al, 60
    xor dil, dil
    syscall

```

Nous voyons que nous avons appliqué cette technique à trois endroits et utilisé le registre 8 bits pour chacun.

Astuce : si jamais nous devons passer `0`à un registre, nous pouvons mettre ce registre à zéro, comme nous l'avons fait ci- `rdi`dessus. De même, si nous en avons même besoin `push 0`dans la pile (par exemple pour la terminaison de chaîne), nous pouvons mettre à zéro n'importe quel registre, puis pousser ce registre vers la pile.

Si nous appliquons tout ce qui précède, nous devrions avoir le code assembleur suivant :

Code : nasm

```
global _start

section .text
_start:
    xor rbx, rbx
    mov bx, 'y!'
    push rbx
    mov rbx, 'B Academ'
    push rbx
    mov rbx, 'Hello HT'
    push rbx
    mov rsi, rsp
    xor rax, rax
    mov al, 1
    xor rdi, rdi
    mov dil, 1
    xor rdx, rdx
    mov dl, 18
    syscall

    xor rax, rax
    add al, 60
    xor dil, dil
    syscall

```

Enfin, nous pouvons assembler notre code et l'exécuter :

```
dsgsec@htb[/htb]$ ./assembler.sh helloworld.s

Hello HTB Academy!

```

Comme nous pouvons le voir, notre code fonctionne comme prévu.

* * * * *

Shellcodage
-----------

Nous pouvons maintenant tenter d'extraire le shellcode de notre nouveau `helloworld`programme, en utilisant notre précédent `shellcoder.py`script :

```
dsgsec@htb[/htb]$ python3 shellcoder.py helloworld

4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05

```

Ce shellcode est bien meilleur. Mais contient-il des octets NULL ? Difficile à dire. Ajoutons donc la ligne suivante à la fin de `shellcoder.py`, qui nous indiquerait si notre code contient des octets NULL et nous indiquerait également la taille de notre shellcode :

Code : python

```
    print("%d bytes - Found NULL byte" % len(shellcode)) if [i for i in shellcode if i == 0] else print("%d bytes - No NULL bytes" % len(shellcode))

```

Exécutons notre script mis à jour pour voir si notre shellcode contient des octets NULL :

```
dsgsec@htb[/htb]$ python3 shellcoder.py helloworld

4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05
61 bytes - No NULL bytes

```

Comme nous pouvons le voir, le `No NULL bytes`nous indique que notre shellcode est `NULL-byte free`.

Essayez d'exécuter le script sur le `Hello World`programme précédent pour voir s'il contenait des octets NULL. Enfin, nous atteignons le moment de vérité, et essayons d'exécuter notre shellcode avec notre `loader.py`script pour voir s'il s'exécute avec succès :

```
dsgsec@htb[/htb]$ python3 loader.py '4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05'

Hello HTB Academy!

```

Comme nous pouvons le voir, nous avons réussi à créer un shellcode fonctionnel pour notre `Hello World`programme.
