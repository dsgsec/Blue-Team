Language assembleur
====================

* * * * *

La plupart de nos interactions avec nos ordinateurs personnels et nos smartphones se font via le système d'exploitation et d'autres applications. Ces applications sont généralement développées à l'aide de langages de haut niveau, comme C++, Java, Python et bien d'autres. Nous savons également que chacun de ces appareils dispose d'un processeur central qui exécute tous les processus nécessaires à l'exécution des systèmes et des applications, ainsi que de la mémoire vive (RAM), de la mémoire vidéo et d'autres composants similaires.

Cependant, ces composants physiques ne peuvent pas interpréter ou comprendre les langages de haut niveau, car ils ne peuvent essentiellement traiter que `1`les ' et `0`'. C'est là qu'intervient le langage Assembly, en tant que langage de bas niveau capable d'écrire des instructions directes que les processeurs peuvent comprendre. Étant donné que le processeur ne peut traiter que des données binaires « c'est-à- `1`dire » et `0`« », il serait difficile pour les humains d'interagir avec les processeurs sans se référer aux manuels pour savoir quel code hexadécimal exécute quelle instruction.

C'est pourquoi les langages assembleurs de bas niveau ont été construits. En utilisant Assembly, les développeurs peuvent écrire des instructions machine lisibles par l'homme, qui sont ensuite assemblées dans leur équivalent en code machine, afin que le processeur puisse les exécuter directement. C'est pourquoi certains qualifient le langage Assembly de code machine symbolique. Par exemple, le code Assembly ' `add rax, 1`' est beaucoup plus intuitif et plus facile à retenir que son shellcode machine équivalent ' `4883C001`', et plus facile à retenir que le code machine binaire équivalent ' `01001000 10000011 11000000 00000001`'. Comme nous pouvons le constater, sans le langage Assembly, il est très difficile d'écrire des instructions machine ou d'interagir directement avec le processeur.

Le code machine est souvent représenté sous la forme `Shellcode`, une représentation hexadécimale des octets du code machine. Le shellcode peut être traduit en son homologue Assembly et peut également être chargé directement en mémoire sous forme d'instructions binaires à exécuter.

* * * * *

Niveau élevé ou niveau bas
--------------------------

Comme il existe différentes conceptions de processeur, chaque processeur comprend un ensemble différent d'instructions machine et un langage d'assemblage différent. Dans le passé, les applications devaient être écrites en assembleur pour chaque processeur, il n'était donc pas facile de développer une application pour plusieurs processeurs. Au début des années 1970, des langages de haut niveau (comme `C`) ont été développés pour permettre d'écrire un code unique, facile à comprendre, pouvant fonctionner sur n'importe quel processeur sans le réécrire pour chaque processeur. Pour être plus précis, cela a été rendu possible grâce à la création de compilateurs pour chaque langage.

Lorsque le code de haut niveau est compilé, il est traduit en instructions d'assemblage pour le processeur pour lequel il est compilé, qui sont ensuite assemblées en code machine à exécuter sur le processeur. C'est pourquoi les compilateurs sont conçus pour différents langages et différents processeurs afin de convertir le code de haut niveau en code assembleur, puis en code machine correspondant au processeur en cours d'exécution.

Plus tard, des langages interprétés ont été développés, comme `Python`, `PHP`, `Bash`, `JavaScript`et d'autres, qui ne sont généralement pas compilés mais sont interprétés au moment de l'exécution. Ces types de langages utilisent des bibliothèques prédéfinies pour exécuter leurs instructions. Ces bibliothèques sont généralement écrites et compilées dans d'autres langages de haut niveau comme `C`ou `C++`. Ainsi, lorsque nous émettons une commande dans un langage interprété, il utilisera la bibliothèque compilée pour exécuter cette commande, qui utilise son code assembleur/code machine pour exécuter toutes les instructions nécessaires à l'exécution de cette commande sur le processeur.

* * * * *

Étapes de compilation
---------------------

![Étapes de compilation](https://academy.hackthebox.com/storage/modules/85/assembly_Compilation_Stages_1.jpg)

Prenons un `Hello World!`programme de base « » qui imprime ces mots à l'écran et montrons comment il passe du code de haut niveau au code machine. Dans un langage interprété, comme Python, ce serait la ligne de base suivante :

Code : python

```
print("Hello World!")

```

Si nous exécutons cette ligne Python, cela équivaudrait essentiellement à exécuter le `C`code suivant :

Code : c

```
#include <unistd.h>

int main()
{
    write(1, "Hello World!", 12);
    _exit(0);
}

```

Remarque : le `C`code source réel est beaucoup plus long, mais ce qui précède est l'essence même de la façon dont la chaîne «`Hello World!` » est imprimée. Si jamais vous souhaitez en savoir plus, vous pouvez consulter le code source de la fonction d'impression Python3 sur ce [lien](https://github.com/python/cpython/blob/0332e569c12d3dc97171546c6dc10e42c27de34b/Python/bltinmodule.c#L1829) et ce [lien](https://github.com/python/cpython/blob/9975cc5008c795e069ce11e2dbed2110cc12e74e/Objects/fileobject.c#L119)

Le `C`code ci-dessus utilise l' `write`appel système Linux, intégré aux processus pour écrire à l'écran. Le même appel système appelé dans Assembly ressemble à ceci :

Code : nasm

```
mov rax, 1
mov rdi, 1
mov rsi, message
mov rdx, 12
syscall

mov rax, 60
mov rdi, 0
syscall

```

Comme nous pouvons le voir, lorsque l' `write`appel système est appelé dans `C`ou dans Assembly, les deux utilisent `1`, le texte et `12`comme arguments. Ceci sera abordé plus en profondeur plus tard dans le module. À partir de ce point, le code assembleur, le shellcode et le code machine binaire sont pour la plupart identiques mais écrits dans des formats différents. Le code Assembly précédent peut être assemblé dans le code machine hexadécimal suivant (c'est-à-dire le shellcode) :

Code : shellcode

```
48 c7 c0 01
48 c7 c7 01
48 8b 34 25
48 c7 c2 0d
0f 05

48 c7 c0 3c
48 c7 c7 00
0f 05

```

Enfin, pour que le processeur exécute les instructions liées à cette machine, il faudrait qu'il soit traduit en binaire, ce qui ressemblerait à ceci :

Code : binaire

```
01001000 11000111 11000000 00000001
01001000 11000111 11000111 00000001
01001000 10001011 00110100 00100101
01001000 11000111 11000010 00001101
00001111 00000101

01001000 11000111 11000000 00111100
01001000 11000111 11000111 00000000
00001111 00000101

```

Un processeur utilise des charges électriques différentes pour a `1`et a `0`et peut donc calculer ces instructions à partir des données binaires une fois qu'il les reçoit.

Remarque : avec les langages multiplateformes, comme `Java`, le code est compilé dans un bytecode Java, qui est le même pour tous les processeurs/systèmes, puis est compilé en code machine par l'environnement Java Runtime local. *C'est ce qui rend Java relativement plus lent que d'autres langages comme C++ qui se compilent directement en code machine. Les langages comme C++ sont plus adaptés aux applications gourmandes en processeur comme les jeux.*

Nous voyons maintenant comment les langages informatiques ont évolué depuis un langage assembleur unique pour chaque processeur jusqu'à des langages de haut niveau pouvant fonctionner sur n'importe quel appareil sans même avoir besoin d'être compilés.

* * * * *

Valeur pour les pentesters
--------------------------

Comprendre les instructions du langage assembleur est essentiel pour l'exploitation binaire, qui constitue un élément essentiel des tests d'intrusion. Lorsqu'il s'agit d'exploiter des programmes compilés, la seule façon de les attaquer serait d'utiliser leurs binaires. Pour démonter, déboguer et suivre les instructions binaires en mémoire et trouver des vulnérabilités potentielles, nous devons avoir une compréhension de base du langage Assembly et de la façon dont il circule à travers les composants du processeur.

C'est pourquoi, une fois que nous aurons commencé à apprendre les techniques d'exploitation binaire, comme les débordements de tampon, les chaînes ROP, l'exploitation du tas et autres, nous nous occuperons beaucoup des instructions d'assemblage et les suivrons en mémoire. De plus, pour exploiter ces vulnérabilités, nous devrons créer des exploits personnalisés qui utilisent des instructions d'assemblage pour manipuler le code en mémoire et injecter du shellcode d'assemblage à exécuter.

L'apprentissage du langage d'assemblage Intel x86 est crucial pour écrire des exploits pour les binaires sur les machines modernes. En plus d'Intel x86, ARM devient de plus en plus courant, car la plupart des smartphones modernes et certains ordinateurs portables modernes comme le MacBook Pro M1 sont équipés de processeurs ARM. L'exploitation de binaires dans ces systèmes nécessite des connaissances en assemblage ARM. Ce module ne couvrira pas le langage d'assemblage ARM. Cela étant dit, les bases du langage assembleur seront sans aucun doute utiles à toute personne souhaitant apprendre l'assemblage ARM, car les deux langages présentent de nombreuses similitudes.