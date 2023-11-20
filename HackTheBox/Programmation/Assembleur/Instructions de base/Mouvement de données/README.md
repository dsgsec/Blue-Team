# Mouvement de données

* * *

Commençons par les instructions de déplacement de données, qui comptent parmi les instructions les plus fondamentales de tout programme assembleur. Nous utiliserons fréquemment des instructions de mouvement de données pour déplacer des données entre des adresses, déplacer des données entre des registres et des adresses mémoire et charger des données immédiates dans des registres ou des adresses mémoire. Les principales `Data Movement`instructions sont :

| Instruction | Description | Exemple |
| --- | --- | --- |
| `mov` | Déplacer des données ou charger des données immédiates | `mov rax, 1`\->`rax = 1` |
| `lea` | Charger une adresse pointant vers la valeur | `lea rax, [rsp+5]`\->`rax = rsp+5` |
| `xchg` | Échanger des données entre deux registres ou adresses | `xchg rax, rbx`\->`rax = rbx, rbx = rax` |

* * *

## Déplacement de données

Utilisons l' `mov`instruction comme toute première instruction dans notre projet de module `fibonacci`. Nous devrons charger les valeurs initiales ( et ) dans et , telles que et . Copions le code suivant dans un fichier :`F0=0``F1=1``rax``rbx``rax = 0``rbx = 1``fib.s`

Code : nasm

    global  _start
    
    section .text
    _start:
        mov rax, 0
        mov rbx, 1
    

Maintenant, assemblons ce code et exécutons-le `gdb`pour voir comment l' `mov`instruction fonctionne en action :

  gdb
```

**$** ./assembler.sh fib.s -g **gef➤  ** b \_start Point d'arrêt 1 à 0x401000
**gef➤  **r **────────────────────────────────────── ───────── ────────────────────────────────────** code:x86:64 ─ **───** 
 → 0x401000 <\_start+ 0> déplacement eax, 0x0 0x401005 <\_start+5> mouvement ebx, 0x1
**──────────────────────────────────────── ────────── ───────────────────────────────────** **rax** : 0x0 $ rbx : 0x0 
 ...COUPER...
**──────────────────────────────────────── ────────── ─────────────────────────────────** code:x86:64 **────** 
   **0x401000 <\_start+0> déplacement eax , 0x0** 
 → 0x401005 <\_start+5> mov ebx, 0x1 
**────────────────────────── ──────── ──────────────────────────────────────── ────────** registres **─ ───** 
$rax   : 0x0 **$rbx** : 0x1 
```

Ainsi, nous avons chargé les valeurs initiales dans nos registres pour effectuer ultérieurement d'autres opérations et instructions sur celles-ci.

Remarque : Dans l'assemblage, le déplacement des données n'affecte pas l'opérande source. Nous pouvons donc le considérer `mov`comme une `copy`fonction plutôt que comme un mouvement réel.

* * *

## Chargement des données

Nous pouvons charger des données immédiates en utilisant l' `mov`instruction. Par exemple, nous pouvons charger la valeur de `1`dans le `rax`registre à l'aide de l' `mov rax, 1`instruction. Nous devons nous rappeler ici que `the size of the loaded data depends on the size of the destination register`. Par exemple, dans l' `mov rax, 1`instruction ci-dessus, puisque nous avons utilisé le registre 64 bits `rax`, il déplacera une représentation 64 bits du nombre `1`(c'est-à-dire `0x00000001`), ce qui n'est pas très efficace.

`This is why it is more efficient to use a register size that matches our data size`. Par exemple, nous obtiendrons le même résultat que l'exemple ci-dessus si nous utilisons `mov al, 1`, puisque nous déplaçons 1 octet ( `0x01`) dans un registre de 1 octet ( `al`), ce qui est beaucoup plus efficace. Cela est évident lorsque nous regardons le démontage des deux instructions dans `objdump`.

Prenons le code assembleur de base suivant pour comparer le démontage des deux instructions :

Code : nasm
```
    global  _start
    
    section .text
    _start:
        mov rax, 0
    	mov rbx, 1
        mov bl, 1
  ```

Maintenant, assemblons-le et visualisons son shellcode avec`objdump` :

```
    dsgsec@htb[/htb]$ nasm -f elf64 fib.s && objdump -M intel -d fib.o
    ...SNIP...
    0000000000000000 <_start>:
       0:	b8 00 00 00 00       	mov    eax,0x0
       5:	bb 01 00 00 00       	mov    ebx,0x1
       a:	b3 01                	mov    bl,0x1
``` 

Nous pouvons voir que le shellcode de la première instruction fait plus du double de la taille de la dernière instruction.

`This understanding will become very handy when writing shellcodes.`

Modifions notre code pour utiliser des sous-registres afin de le rendre plus efficace :

Code : nasm
```
    global  _start
    
    section .text
    _start:
        mov al, 0
        mov bl, 1
```    

L' `xchg`instruction échangera les données entre les deux registres. Essayez d'ajouter `xchg rax, rbx`à la fin du code, assemblez-le, puis exécutez-le `gdb`pour voir comment il fonctionne.

* * *

## Pointeurs d'adresse

Un autre concept essentiel à comprendre est l’utilisation de pointeurs. Dans de nombreux cas, nous verrions que le registre ou l'adresse que nous utilisons ne contient pas immédiatement la valeur finale mais contient une autre adresse qui pointe vers la valeur finale. C'est toujours le cas avec les registres *de pointeurs* , comme `rsp`, `rbp`, et `rip`, mais est également utilisé avec tout autre registre ou adresse mémoire.

Par exemple, assemblons et exécutons `gdb`notre `fib`binaire assemblé, et vérifions les registres `rsp`et :`rip`

  gdb
```
gdb -q ./fib **gef➤  ** b \_start Point d'arrêt 1 à 0x401000
**gef➤r  ** \_ ...COUPER...
$rsp   : 0x00007fffffffe490 → 0x0000000000000001 $rip   : 0x0000000000401000 → **<\_start+0> mov eax, 0x0**
```

Nous voyons que les deux registres contiennent des adresses de pointeurs vers d’autres emplacements. `GEF`fait un excellent travail en nous montrant la valeur de la destination finale.

#### Déplacement des valeurs du pointeur

Nous pouvons voir que le `rsp`registre contient la valeur finale de `0x1`, et sa valeur immédiate est une adresse de pointeur vers `0x1`. Donc, si nous devions utiliser `mov rax, rsp`, nous ne déplacerons pas la valeur `0x1`vers `rax`, mais nous déplacerons l'adresse du pointeur `0x00007fffffffe490`vers `rax`.

Pour déplacer la valeur réelle, nous devrons utiliser des crochets `[]`, ce qui signifie en `x86_64`assemblage et en syntaxe . Ainsi, dans le même exemple ci-dessus, si nous voulons déplacer la valeur finale vers laquelle pointe, nous pouvons la mettre entre crochets, comme , et cette instruction déplacera la valeur finale plutôt que la valeur immédiate (qui est une adresse vers la valeur finale). valeur).`Intel``load value at address``rsp``rsp``mov rax, [rsp]``mov`

Nous pouvons utiliser des crochets pour calculer un décalage d'adresse par rapport à un registre ou une autre adresse. Par exemple, nous pouvons `mov rax, [rsp+10]`éloigner la valeur stockée de 10 adresses de `rsp`.

Pour bien démontrer cela, prenons l’exemple de code suivant :

Code : nasm
```
    global  _start
    
    section .text
    _start:
        mov rax, rsp
        mov rax, [rsp]
```    

Ceci est juste un programme simple pour démontrer ce point et voir la différence entre les deux instructions.

Maintenant, assemblons le code et exécutons le programme avec gdb :


  gdb
```
**$** ./assembler.sh rsp.s -g **gef➤  ** b \_start Point d'arrêt 1 à 0x401000
**gef➤r  ** \_ ...COUPER...
**──────────────────────────────────────── ────────── ─────────────────────────────────** code:x86:64 **────** 
 → 0x401000 <\_start+0> mouvement rax, rsp 
**────────────────────────────────────── ───────── ──────────────────────────────────────** registres **─ ───** 
$rax   : 0x00007fffffffe490 → 0x0000000000000001 $ rsp   : 0x00007fffffffe490 → 0x0000000000000001 
```

Comme nous pouvons le voir, `mov rax, rsp`la valeur immédiate stockée dans `rsp`(qui est une adresse de pointeur vers `rsp`) a été déplacée vers le `rax`registre. Maintenant, appuyons `si`et vérifions à quoi `rax`ressemblera la deuxième instruction :

  gdb
```
**$** ./assembler.sh rsp.s -g **gef➤  ** b \_start Point d'arrêt 1 à 0x401000
**gef➤r  ** \_ ...COUPER...
**──────────────────────────────────────── ────────── ─────────────────────────────────** code:x86:64 **────** 
 → 0x401003 <\_start+3> mouvement rax, QWORD PTR \[rsp\] 
**──────────────────────────────────── ─────── ──────────────────────────────────────── ──** enregistre **────**
$rax   : 0x1 $rsp   : 0x00007fffffffe490 → 0x0000000000000001 
```

Nous pouvons voir que cette fois, la valeur finale de `0x1`a été déplacée dans le `rax`registre.

Remarque : lors de l'utilisation de `[]`, nous devrons peut-être définir la taille des données avant les crochets, comme `byte`ou `qword`. Cependant, dans la plupart des cas, `nasm`cela sera automatiquement fait pour nous. Nous pouvons voir ci-dessus que l'instruction finale est en réalité `mov rax, QWORD PTR [rsp]`. Nous voyons également que cela `nasm`a également été ajouté `PTR`pour spécifier le déplacement d'une valeur à partir d'un pointeur.

#### Chargement des pointeurs de valeur

Enfin, nous devons comprendre comment charger une adresse de pointeur sur une valeur, en utilisant l' instruction `lea`(ou `Load Effective Address`), qui charge un pointeur vers la valeur spécifiée, comme dans `lea rax, [rsp]`. C'est le contraire de ce que nous venons d'apprendre ci-dessus (c'est-à-dire charger le pointeur vers une valeur plutôt que déplacer la valeur à partir du pointeur).

Dans certains cas, nous devons charger l'adresse d'une valeur dans un certain registre plutôt que de charger directement la valeur dans ce registre. Cela se produit généralement lorsque les données sont volumineuses et ne tiennent pas dans un seul registre. Les données sont donc placées sur la pile ou dans le tas et un pointeur vers leur emplacement est stocké dans le registre.

Par exemple, l' `write`appel système que nous avons utilisé dans notre `HelloWorld`programme nécessite un pointeur vers le texte à imprimer, au lieu de fournir directement le texte, qui peut ne pas tenir entièrement dans le registre, car celui-ci ne fait que 64 bits ou 8 octets.

Premièrement, si nous souhaitons charger un pointeur direct vers une variable ou une étiquette, nous pouvons toujours utiliser `mov`des instructions. Étant donné que le nom de la variable est un pointeur vers l'endroit où elle se trouve en mémoire, `mov`ce pointeur sera stocké vers l'adresse de destination. Par exemple, les deux `mov rax, rsp`et `lea rax, [rsp]`feront la même chose en stockant le pointeur `message`vers `rax`.

Cependant, si nous voulions charger un pointeur avec un décalage (c'est-à-dire à quelques adresses d'une variable ou d'une adresse), nous devrions utiliser `lea`. C'est pourquoi `lea`l'opérande source est généralement une variable, une étiquette ou une adresse entourée de crochets, comme dans `lea rax, [rsp+10]`. Cela permet d'utiliser des décalages (c'est-à-dire `[rsp+10]`).

Notez que si nous utilisons `mov rax, [rsp+10]`, cela déplacera en fait la valeur `[rsp+10]`vers `rax`, comme indiqué précédemment. Nous ne pouvons pas déplacer un pointeur avec un décalage en utilisant `mov`.

Prenons l'exemple suivant pour montrer comment `lea`fonctionne et en quoi il peut différer de`mov` :

Code : nasm
```
    global  _start
    
    section .text
    _start:
        lea rax, [rsp+10]
        mov rax, [rsp+10]
```    

Maintenant, assemblons-le et exécutons-le avec`gdb` :

  gdb
```
**$** ./assembler.sh lea.s -g **gef➤  ** b \_start Point d'arrêt 1 à 0x401000
**gef➤r  ** \_ ...COUPER...
**──────────────────────────────────────── ────────── ─────────────────────────────────** code:x86:64 **────** 
 → 0x401003 <\_start+0> lecture rax, \[rsp+0xa\] 
**──────────────────────────────────── ─────── ──────────────────────────────────────── ──** enregistre **────**
$rax   : 0x00007ffffffffe49a → 0x000000007fffffff $rsp   : 0x00007ffffffffe490 → 0x0000000000000001 
```
Nous voyons que `lea rax, [rsp+10]`l'adresse qui se trouve à 10 adresses a été chargée `rsp`(en d'autres termes, à 10 adresses du haut de la pile). Voyons maintenant `si`ce que cela `mov rax, [rsp+10]`donnerait :

  gdb
```
**──────────────────────────────────────── ────────── ─────────────────────────────────** code:x86:64 **────** 
 → 0x401008 <\_start+8> mouvement rax, QWORD PTR \[rsp+0xa\] 
**────────────────────────────────── ─────── ──────────────────────────────────────── ────** enregistre **────**
$rax   : 0x7fffffff $rsp   : 0x00007ffffffffe490 → 0x0000000000000001 
```

Comme prévu, nous voyons que `mov rax, [rsp+10]`la valeur qui y est stockée a été déplacée vers `rax`.
