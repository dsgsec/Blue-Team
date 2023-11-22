Shellcodes
==========

* * * * *

Nous devons avoir une très bonne compréhension de l'architecture de l'ordinateur et du processeur et de la manière dont les programmes interagissent avec cette architecture sous-jacente grâce à ce que nous avons appris dans ce module. Nous devrions également être capables de désassembler et de déboguer les binaires et de bien comprendre quelles instructions machine ils exécutent et quel est leur objectif général. Nous allons maintenant découvrir `shellcodes`, qui est un concept essentiel pour les testeurs d'intrusion.

* * * * *

Qu'est-ce qu'un Shellcode
-------------------------

Nous savons que chaque binaire exécutable est constitué d'instructions machine écrites en Assembly puis assemblées en code machine. A `shellcode`est la représentation hexadécimale du code machine exécutable d'un binaire. Par exemple, prenons notre `Hello World`programme, qui exécute les instructions suivantes :

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

Comme nous l'avons vu dans la première section, ce `Hello World`programme assemble le shellcode suivant :

Code : shellcode

```
48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05

```

Ce shellcode doit représenter correctement les instructions machine, et s'il est transmis à la mémoire du processeur, il doit le comprendre et l'exécuter correctement.

* * * * *

Utilisation en test de pente
----------------------------

Avoir la possibilité de passer un shellcode directement dans la mémoire du processeur et de le faire exécuter joue un rôle essentiel dans `Binary Exploitation`. Par exemple, avec un exploit de dépassement de tampon, nous pouvons transmettre un `reverse shell`shellcode, le faire exécuter et recevoir un shell inversé.

Les systèmes modernes `x86_64`peuvent disposer de protections contre le chargement de shellcodes en mémoire. C'est pourquoi `x86_64`l'exploitation binaire s'appuie généralement sur `Return Oriented Programming (ROP)`, ce qui nécessite également une bonne compréhension du langage assembleur et de l'architecture informatique abordés dans ce module.

De plus, certaines techniques d'attaque reposent sur l'infection d'exécutables existants (comme `elf`ou `.exe`) ou de bibliothèques (comme `.so`ou `.dll`) avec du shellcode, de sorte que ce shellcode soit chargé en mémoire et exécuté une fois ces fichiers exécutés. Un autre avantage de l'utilisation des shellcodes en pentesting est la possibilité d'exécuter du code directement en mémoire sans rien écrire sur le disque, ce qui est très important pour réduire notre visibilité et notre empreinte sur le serveur distant.

* * * * *

Assemblage au code machine
--------------------------

Pour comprendre comment les shellcodes sont générés, il faut d'abord comprendre comment chaque instruction est convertie en code machine. Chaque `x86`instruction et chaque registre possède son propre `binary`code machine (généralement représenté en `hex`), qui représente le code binaire transmis directement au processeur pour lui indiquer quelle instruction exécuter (via le cycle d'instructions.)

De plus, les combinaisons courantes d'instructions et de registres ont également leur propre code machine. Par exemple, l' `push rax`instruction possède le code machine `50`, tandis que l' `push rbx`instruction possède le code machine `53`, et ainsi de suite. Lorsque nous assemblons notre code avec `nasm`, il convertit nos instructions d'assemblage en leur code machine respectif afin que le processeur puisse les comprendre.

N'oubliez pas : le langage assembleur est conçu pour être lisible par l'homme et le processeur ne peut pas le comprendre sans être converti en code machine. Nous l'utiliserons `pwntools`pour assembler et désassembler notre code machine, car c'est un outil essentiel pour l'exploitation binaire, et c'est une excellente opportunité pour commencer à l'apprendre. Tout d'abord, nous pouvons installer `pwntools`avec la commande suivante (elle devrait déjà être installée dans PwnBox) :

```
dsgsec@htb[/htb]$ sudo pip3 install pwntools

```

Maintenant, nous pouvons utiliser `pwn asm`pour assembler n'importe quel code assembleur dans son shellcode, comme suit :

```
dsgsec@htb[/htb]$ pwn asm 'push rax'  -c 'amd64'
   0:    50                       push   eax

```

Remarque : Nous avons utilisé l' `-c 'amd64'`indicateur pour nous assurer que l'outil interprète correctement notre code assembleur pour`x86_64`

Comme nous pouvons le voir, nous obtenons `50`, qui est le même code machine pour `push rax`. De même, nous pouvons convertir le code machine hexadécimal ou le shellcode en son code assembleur correspondant, comme suit :

```
dsgsec@htb[/htb]$ pwn disasm '50' -c 'amd64'
   0:    50                       push   eax

```

Nous pouvons en savoir plus sur `pwntools`les fonctionnalités d'assemblage et de démontage [ici](https://docs.pwntools.com/en/stable/asm.html) , et sur les `pwntools`outils de ligne de commande [ici](https://docs.pwntools.com/en/stable/commandline.html) .

* * * * *

Extraire le shellcode
---------------------

Maintenant que nous comprenons comment chaque instruction assembleur est convertie en code machine (et vice-versa), voyons comment extraire le shellcode de n'importe quel binaire.

Le shellcode d'un binaire représente `.text`uniquement sa section exécutable, car les shellcodes sont censés être directement exécutables. Pour extraire la `.text`section avec `pwntools`, nous pouvons utiliser la `ELF`bibliothèque pour charger un `elf`binaire, ce qui nous permettrait d'exécuter diverses fonctions dessus. Alors, exécutons l' `python3`interpréteur pour mieux comprendre comment l'utiliser. Tout d'abord, nous devrons importer `pwntools`, puis nous pourrons lire le `elf`binaire, comme suit :

```
dsgsec@htb[/htb]$ python3

>>> from pwn import *
>>> file = ELF('helloworld')

```

Maintenant, nous pouvons `pwntools`y exécuter diverses fonctions, sur lesquelles nous pouvons en savoir plus [ici](https://docs.pwntools.com/en/stable/elf/elf.html) . Nous devons vider le code machine de la `.text`section exécutable, ce que nous pouvons faire avec la `section()`fonction, comme suit :

```
>>> file.section(".text").hex()
'48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05'

```

Remarque : Nous avons ajouté «`hex()` » pour encoder le shellcode en hexadécimal, au lieu de l'imprimer en octets bruts.

On voit que nous avons pu très facilement extraire le shellcode du binaire. Transformons cela en un script Python afin que nous puissions l'utiliser rapidement pour extraire le shellcode de n'importe quel binaire :

Code : python

```
#!/usr/bin/python3

import sys
from pwn import *

context(os="linux", arch="amd64", log_level="error")

file = ELF(sys.argv[1])
shellcode = file.section(".text")
print(shellcode.hex())

```

Nous pouvons copier le script ci-dessus dans `shellcoder.py`, puis lui passer le nom de n'importe quel fichier binaire comme argument, et il extraira son shellcode :

```
dsgsec@htb[/htb]$ python3 shellcoder.py helloworld

48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05

```

Une autre méthode (un peu moins fiable) pour extraire le shellcode serait via `objdump`, que nous avons utilisée dans une section précédente. Nous pouvons écrire le `bash`script suivant `shellcoder.sh`et l'utiliser pour extraire le shellcode si jamais nous ne pouvons pas utiliser le premier script :

Code : bash

```
#!/bin/bash

for i in $(objdump -d $1 |grep "^ " |cut -f2); do echo -n $i; done; echo;

```

Encore une fois, nous pouvons essayer de l'exécuter `helloworld`pour obtenir son shellcode, comme suit :

```
dsgsec@htb[/htb]$ ./shellcoder.sh helloworld

48be0020400000000000bf01000000ba12000000b8010000000f05b83c000000bf000000000f05

```

* * * * *

Chargement du shellcode
-----------------------

Maintenant que nous avons un shellcode, essayons de l'exécuter, ce qui nous permettra de tester n'importe quel shellcode que nous avons préparé avant de l'utiliser dans l'exploitation binaire. Le shellcode que nous avons extrait ci-dessus ne répond pas à ce dont `Shellcoding Requirements`nous parlerons dans la section suivante et ne fonctionnera donc pas. Pour montrer comment exécuter des shellcodes, nous utiliserons le `fixed`shellcode suivant ( ), qui répond à tous les critères`Shellcoding Requirements` :

Code : shellcode

```
4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05

```

Pour exécuter notre shellcode avec `pwntools`, nous pouvons utiliser la `run_shellcode`fonction et lui passer notre shellcode, comme suit :

```
dsgsec@htb[/htb]$ python3

>>> from pwn import *
>>> context(os="linux", arch="amd64", log_level="error")
>>> run_shellcode(unhex('4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05')).interactive()

Hello HTB Academy!

```

Nous avons utilisé `unhex()`le shellcode pour le reconvertir en binaire.

Comme nous pouvons le voir, notre shellcode a exécuté et imprimé avec succès la chaîne `Hello HTB Academy!`. En revanche, si nous exécutons le shellcode précédent (qui ne répondait pas aux exigences `Shellcoding Requirements`), il ne s'exécutera pas :

```
>>> run_shellcode(unhex('b801000000bf0100000048be0020400000000000ba120000000f05b83c000000bf000000000f05')).interactive()

```

Encore une fois, pour faciliter l'exécution de nos shellcodes, transformons ce qui précède en un script Python :

Code : python

```
#!/usr/bin/python3

import sys
from pwn import *

context(os="linux", arch="amd64", log_level="error")

run_shellcode(unhex(sys.argv[1])).interactive()

```

Nous pouvons copier le script ci-dessus dans `loader.py`, passer notre shellcode comme argument et l'exécuter pour exécuter notre shellcode :

```
dsgsec@htb[/htb]$ python3 loader.py '4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05'

Hello HTB Academy!

```

Comme nous pouvons le voir, nous avons pu charger et exécuter notre shellcode avec succès.

* * * * *

Shellcode de débogage
---------------------

Enfin, voyons comment déboguer notre shellcode avec `gdb`. Si nous chargeons le code machine directement en mémoire, comment l'exécuterions-nous avec `gdb`? Il existe de nombreuses façons de le faire, et nous en passerons en revue quelques-unes ici.

Nous pouvons toujours exécuter notre shellcode avec `loader.py`, puis attacher son processus à `gdb`with `gdb -p PID`. Cependant, cela ne fonctionnera que si notre processus ne se termine pas avant que nous nous y attachions. Nous allons donc plutôt construire notre shellcode en `elf`binaire, puis utiliser ce binaire `gdb`comme nous l'avons fait tout au long du module.

#### Outils Pwn

Nous pouvons utiliser `pwntools`pour construire un `elf`binaire à partir de notre shellcode en utilisant la `ELF`bibliothèque, puis la `save`fonction pour l'enregistrer dans un fichier :

Code : python

```
ELF.from_bytes(unhex('4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05')).save('helloworld')

```

Pour faciliter son utilisation, nous pouvons transformer ce qui précède en script et l'écrire dans`assembler.py` :

Code : python

```
#!/usr/bin/python3

import sys, os, stat
from pwn import *

context(os="linux", arch="amd64", log_level="error")

ELF.from_bytes(unhex(sys.argv[1])).save(sys.argv[2])
os.chmod(sys.argv[2], stat.S_IEXEC)

```

Nous pouvons maintenant exécuter `assembler.py`, passer le shellcode comme premier argument et le nom du fichier comme deuxième argument, et cela assemblera le shellcode dans un exécutable :

  Outils Pwn

```
dsgsec@htb[/htb]$ python assembler.py '4831db66bb79215348bb422041636164656d5348bb48656c6c6f204854534889e64831c0b0014831ff40b7014831d2b2120f054831c0043c4030ff0f05' 'helloworld'

```

  Outils Pwn

```
dsgsec@htb[/htb]$ ./helloworld

Hello HTB Academy!

```

Comme nous pouvons le voir, il a construit le `helloworld`binaire avec le nom de fichier que nous avons spécifié. Nous pouvons maintenant l'exécuter avec `gdb`, et utiliser `b *0x401000`to break au point d'entrée binaire par défaut :

  gdb
```
$ gdb -q bonjour monde gef➤  b *0x401000 gef➤   r Point d'arrêt 1, 0x0000000000401000 dans ?? () ...COUPER...
──────────────────────────────────────── ────────── ───────────────────────────────────── code:x86:64 ────
● → 0x401000 xor rbx, rbx 0x401003 boîte de déplacement, 0x2179 0x401007 pousser rbx

#### CCG
```

Il existe d'autres méthodes pour créer notre shellcode dans un `elf`exécutable. Nous pouvons ajouter notre shellcode au `C`code suivant, l'écrire dans un `helloworld.c`, puis le construire avec `gcc`(les octets hexadécimaux doivent être échappés avec `\x`) :

Code : c

```
#include <stdio.h>

int main()
{
    int (*ret)() = (int (*)()) "\x48\x31\xdb\x66\xbb\...SNIP...\x3c\x40\x30\xff\x0f\x05";
    ret();
}

```

Ensuite, nous pouvons compiler notre `C`code avec `gcc`, et l'exécuter avec `gdb`:

  CCG

```
dsgsec@htb[/htb]$ gcc helloworld.c -o helloworld
dsgsec@htb[/htb]$ gdb -q helloworld

```

Cependant, cette méthode n'est pas très fiable pour plusieurs raisons. Premièrement, il enveloppera l'intégralité du binaire dans `C`le code, de sorte que le binaire ne contiendra pas notre shellcode, mais contiendra diverses autres `C`fonctions et bibliothèques. Cette méthode peut également ne pas toujours compiler, en fonction des protections de mémoire existantes, nous devrons donc peut-être ajouter des indicateurs pour contourner les protections de mémoire, comme suit :

  CCG

```
dsgsec@htb[/htb]$ gcc helloworld.c -o helloworld -fno-stack-protector -z execstack -Wl,--omagic -g --static
dsgsec@htb[/htb]$ ./helloworld

Hello HTB Academy!

```

Avec cela, nous devrions avoir une bonne compréhension des bases des shellcodes. Nous pouvons maintenant créer nos propres shellcodes pour nos prochaines étapes.
