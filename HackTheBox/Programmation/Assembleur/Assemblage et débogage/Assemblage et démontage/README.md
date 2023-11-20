Assemblage et démontage
=======================

* * * * *

Maintenant que nous comprenons la structure de base et les éléments d'un fichier Assembly, nous pouvons commencer à l'assembler à l'aide de l' `nasm`outil. Toute la structure du fichier assembly que nous avons apprise dans la section précédente est basée sur la `nasm`structure du fichier. Lors de l'assemblage de notre code avec `nasm`, il comprend les différentes parties du fichier puis les assemble correctement pour être correctement exécuté pendant l'exécution.

Après avoir assemblé notre code avec `nasm`, nous pouvons le lier en utilisant `ld`diverses fonctionnalités et bibliothèques du système d'exploitation.

* * * * *

Assemblage
----------

Tout d'abord, nous allons copier le code ci-dessus dans un fichier appelé `helloWorld.s`.

Remarque : les fichiers d'assemblage utilisent généralement les extensions `.s`ou `.asm`. Nous utiliserons `.s`dans ce module.

Nous n'avons pas besoin de continuer à utiliser des onglets pour séparer les parties d'un fichier d'assemblage, car c'était uniquement à des fins de démonstration. Nous pouvons écrire le code suivant dans notre `helloWorld.s`fichier :

Code : nasm

```
global _start

section .data
    message db "Hello HTB Academy!"
    length equ $-message

section .text
_start:
    mov rax, 1
    mov rdi, 1
    mov rsi, message
    mov rdx, length
    syscall

    mov rax, 60
    mov rdi, 0
    syscall

```

Notez comment nous avions l'habitude `equ`de calculer dynamiquement la longueur de `message`, au lieu d'utiliser un fichier statique `18`. Cela deviendra très pratique par la suite. Une fois cela fait, nous assemblerons le fichier en utilisant `nasm`, avec la commande suivante :

```
dsgsec@htb[/htb]$ nasm -f elf64 helloWorld.s

```

Remarque : Le `-f elf64`flag est utilisé pour noter que l'on souhaite assembler un code assembleur 64 bits. Si nous voulions assembler un code 32 bits, nous utiliserions `-f elf`.

Cela devrait générer un `helloWorld.o`fichier objet, qui est ensuite assemblé en code machine, avec les détails de toutes les variables et sections. Ce fichier n'est pas encore exécutable.

* * * * *

Mise en relation
----------------

La dernière étape consiste à lier notre fichier en utilisant `ld`. Le `helloWorld.o`fichier objet, bien qu'assemblé, ne peut toujours pas être exécuté. En effet, de nombreuses références et étiquettes utilisées `nasm`doivent être résolues en adresses réelles, ainsi que la liaison du fichier avec diverses bibliothèques du système d'exploitation qui peuvent être nécessaires.

C'est pourquoi un binaire Linux est appelé `ELF`, ce qui signifie un `Executable and Linkable Format`. Pour lier un fichier à l'aide de `ld`, nous pouvons utiliser la commande suivante :

```
dsgsec@htb[/htb]$ ld -o helloWorld helloWorld.o

```

Remarque : si nous devions assembler un binaire 32 bits, nous devons ajouter l' `-m elf_i386`indicateur « ».

Une fois que nous avons lié le fichier avec `ld`, nous devrions avoir le fichier exécutable final :

```
dsgsec@htb[/htb]$ ./helloWorld
Hello HTB Academy!

```

Nous avons assemblé et lié avec succès notre premier fichier d'assemblage. Nous allons assembler, relier et exécuter notre code fréquemment via ce module, alors construisons un `bash`script simple pour le rendre plus facile :

Code : bash

```
#!/bin/bash

fileName="${1%%.*}" # remove .s extension

nasm -f elf64 ${fileName}".s"
ld ${fileName}".o" -o ${fileName}
[ "$2" == "-g" ] && gdb -q ${fileName} || ./${fileName}

```

Nous pouvons maintenant écrire ce script dessus `assembler.sh`, `chmod +x`puis l'exécuter sur notre fichier assembleur. Il va l'assembler, le relier et l'exécuter :

```
dsgsec@htb[/htb]$ ./assembler.sh helloWorld.s
Hello HTB Academy!

```

Super! Avant de continuer, démontons et examinons nos fichiers pour en savoir plus sur le processus que nous venons de suivre.

* * * * *

Démontage
---------

Pour désassembler un fichier, nous utiliserons l' `objdump`outil qui extrait le code machine d'un fichier et interprète l'instruction d'assemblage de chaque code hexadécimal. Nous pouvons démonter un binaire en utilisant le `-D`flag.

Remarque : nous utiliserons également le flag `-M intel`, afin `objdump`d'écrire les instructions dans la syntaxe Intel, que nous utilisons, comme nous l'avons vu précédemment.

Commençons par démonter notre `ELF`fichier exécutable final :

```
dsgsec@htb[/htb]$ objdump -M intel -d helloWorld

helloWorld:     file format elf64-x86-64

Disassembly of section .text:

0000000000401000 <_start>:
  401000:	b8 01 00 00 00       	mov    eax,0x1
  401005:	bf 01 00 00 00       	mov    edi,0x1
  40100a:	48 be 00 20 40 00 00 	movabs rsi,0x402000
  401011:	00 00 00
  401014:	ba 12 00 00 00       	mov    edx,0x12
  401019:	0f 05                	syscall
  40101b:	b8 3c 00 00 00       	mov    eax,0x3c
  401020:	bf 00 00 00 00       	mov    edi,0x0
  401025:	0f 05                	syscall

```

Nous voyons que notre code assembleur d'origine est hautement préservé, le seul changement étant `0x402000`utilisé à la place de la `message`variable et en remplaçant la `length`constante par sa valeur `0x12`. Nous constatons également que nous `nasm`avons modifié efficacement nos `64-bit`registres en `32-bit`sous-registres lorsque cela est possible, pour utiliser moins de mémoire lorsque cela est possible, comme en passant `mov rax, 1`à `mov eax,0x1`.

Si nous voulions afficher uniquement le code assembleur, sans code machine ni adresses, nous pourrions ajouter les `--no-show-raw-insn --no-addresses`indicateurs, comme suit :

```
dsgsec@htb[/htb]$ objdump -M intel --no-show-raw-insn --no-addresses -d helloWorld

helloWorld:     file format elf64-x86-64

Disassembly of section .text:

<_start>:
        mov    eax,0x1
        mov    edi,0x1
        movabs rsi,0x402000
        mov    edx,0x12
        syscall
        mov    eax,0x3c
        mov    edi,0x0
        syscall

```

Remarque : Notez que `objdump`la troisième instruction a été modifiée en `movabs`. C'est la même chose que `mov`, donc si vous avez besoin de réassembler le code, vous pouvez le remplacer par `mov`.

Le `-d`drapeau ne démontera que la `.text`section de notre code. Pour vider des chaînes, nous pouvons utiliser le `-s`drapeau et ajouter `-j .data`pour examiner uniquement la `.data`section. Cela signifie que nous n'avons pas non plus besoin d'ajouter `-M intel`. La commande finale est la suivante :

```
dsgsec@htb[/htb]$ objdump -sj .data helloWorld

helloWorld:     file format elf64-x86-64

Contents of section .data:
 402000 48656c6c 6f204854 42204163 6164656d  Hello HTB Academ
 402010 7921                                 y!

```

Comme on peut le voir, la `.data`section contient bien la `message`variable avec la chaîne `Hello HTB Academy!`. Cela devrait nous donner une meilleure idée de la façon dont notre code a été assemblé en code machine et de son aspect une fois assemblé. Passons ensuite aux bases du débogage de code, qui est une compétence essentielle que nous devons acquérir.
