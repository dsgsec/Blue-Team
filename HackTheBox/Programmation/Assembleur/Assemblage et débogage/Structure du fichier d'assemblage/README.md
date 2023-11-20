Structure du fichier d'assemblage
=================================

* * * * *

Au fur et à mesure que nous apprendrons diverses instructions d'assemblage dans les sections à venir, nous allons constamment écrire du code, l'assembler et le déboguer. C'est la meilleure façon d'apprendre ce que fait chaque instruction. Nous devons donc apprendre la structure de base d'un fichier de code Assembly, puis l'assembler et le déboguer.

Dans cette section, nous passerons en revue la structure de base d'un fichier Assembly, et dans les deux sections suivantes, nous aborderons son assemblage et son débogage. Nous travaillerons sur un modèle `Hello World!`de code Assembly comme exemple, pour d'abord apprendre la structure générale d'un fichier assembleur, puis comment l'assembler et le déboguer. Commençons par examiner et disséquer un exemple `Hello World!`de modèle de code Assembly :

```
         global  _start

         section .data
message: db      "Hello HTB Academy!"

         section .text
_start:
         mov     rax, 1
         mov     rdi, 1
         mov     rsi, message
         mov     rdx, 18
         syscall

         mov     rax, 60
         mov     rdi, 0
         syscall

```

Ce code Assembly (une fois assemblé et lié) doit imprimer la chaîne «`Hello HTB Academy!` » à l'écran. Nous n'entrerons pas encore dans les détails de la façon dont cela est traité, mais nous devons comprendre les principaux éléments du modèle de code.

* * * * *

Structure du fichier d'assemblage
---------------------------------

Tout d'abord, examinons la manière dont le code est distribué :

![nasm_structure](https://github.com/dsgsec/Blue-Team/assets/82456829/73e389da-477b-44a9-bc3b-18c4c0c894a3)


En regardant les parties verticales du code, chaque ligne peut contenir trois éléments :

| 1.`Labels` | 2.`Instructions` | 3.`Operands` |
| --- | --- | --- |

Nous avons discuté des `instructions`et des leurs `operands`dans les sections précédentes, et nous entrerons dans les détails des diverses instructions d'assemblage dans les sections suivantes. En plus de cela, nous pouvons définir un `label`à chaque ligne. Chaque étiquette peut être référencée par `instructions`ou par `directives`.

Ensuite, si nous regardons le code ligne par ligne, nous voyons qu'il comporte trois parties principales :

| Section | Description |
| --- | --- |
| `global _start` | Il s'agit d'un `directive`qui ordonne au code de commencer à s'exécuter au niveau de l' `_start`étiquette définie ci-dessous. |
| `section .data` | Il s'agit de la `data`section qui doit contenir toutes les variables. |
| `section .text` | Il s'agit de la `text`section contenant tout le code à exécuter. |

Les sections `.data`et `.text`font référence aux segments de mémoire `data`et `text`dans lesquels ces instructions seront stockées.

* * * * *

Directives
----------

Un code Assembly est basé sur la ligne, ce qui signifie que le fichier est traité ligne par ligne, en exécutant l'instruction de chaque ligne. Nous voyons sur la première ligne une directive `global _start`qui demande à la machine de commencer à traiter les instructions après l' `_start`étiquette. Ainsi, la machine se dirige vers l' `_start`étiquette et commence à y exécuter les instructions, ce qui imprimera le message à l'écran. Ceci sera abordé plus en détail dans les `Control Instructions`sections.

* * * * *

Variables
---------

Ensuite, nous avons la `.data`section. La `data`section contient nos variables pour nous permettre de définir plus facilement des variables et de les réutiliser sans les écrire plusieurs fois. Une fois notre programme exécuté, toutes nos variables seront chargées en mémoire dans le `data`segment.

Lorsque nous exécutons le programme, il chargera toutes les variables que nous avons définies en mémoire afin qu'elles soient prêtes à être utilisées lorsque nous les appellerons. Nous remarquerons plus tard dans le module qu'au moment où nous commencerons à exécuter des instructions sur l' `_start`étiquette, toutes nos variables seront déjà chargées en mémoire.

Nous pouvons définir des variables en utilisant `db`pour une liste d'octets, `dw`pour une liste de mots, `dd`pour une liste de chiffres, etc. Nous pouvons également étiqueter n'importe laquelle de nos variables afin de pouvoir l'appeler ou la référencer plus tard. Voici quelques exemples de définition de variables :

| Instruction | Description |
| --- | --- |
| `db 0x0a` | Définit l'octet `0x0a`, qui est une nouvelle ligne. |
| `message db 0x41, 0x42, 0x43, 0x0a` | Définit l'étiquette `message => abc\n`. |
| `message db "Hello World!", 0x0a` | Définit l'étiquette `message => Hello World!\n`. |

De plus, nous pouvons utiliser l' `equ`instruction avec le `$`jeton pour évaluer une expression, comme la longueur de la chaîne d'une variable définie. Cependant, les étiquettes définies avec l' `equ`instruction sont des constantes et ne peuvent pas être modifiées ultérieurement.

Par exemple, le code suivant définit une variable puis définit une constante pour sa longueur :

```
section .data
    message db "Hello World!", 0x0a
    length  equ $-message

```

Remarque : le `$`jeton indique la distance actuelle depuis le début du tronçon en cours. Comme la `message`variable se trouve au début de la `data`section, l'emplacement actuel, c'est-à-dire. la valeur de `$`, est égale à la longueur de la chaîne. Dans le cadre de ce module, nous utiliserons uniquement ce jeton pour calculer la longueur des chaînes, en utilisant la même ligne de code présentée ci-dessus.

* * * * *

Code
----

La deuxième (et la plus importante) section est la `.text`section. Cette section contient toutes les instructions d'assemblage et les charge dans le `text`segment de mémoire. Une fois toutes les instructions chargées dans le `text`segment, le processeur commence à les exécuter les unes après les autres.

La convention par défaut est d'avoir l' `_start`étiquette au début de la `.text`section, qui - conformément à la `global _start`directive - démarre le code principal qui sera exécuté lors de l'exécution du programme. Comme nous le verrons plus loin dans le module, nous pouvons définir d'autres étiquettes au sein de la `.text`section, des boucles for et d'autres fonctions.

Le `text`segment dans la mémoire est en lecture seule, nous ne pouvons donc y écrire aucune variable. La `data`section, en revanche, est en lecture/écriture, c'est pourquoi nous y écrivons nos variables. Cependant, le `data`segment dans la mémoire n'est pas exécutable, donc tout code que nous y écrivons ne peut pas être exécuté. Cette séparation fait partie des protections de mémoire visant à atténuer des problèmes tels que les débordements de mémoire tampon et d'autres types d'exploitation binaire.

Astuce : Nous pouvons ajouter des commentaires à notre code assembleur avec un point-virgule `;`. Nous pouvons utiliser des commentaires pour expliquer le but de chaque partie du code et ce que fait chaque ligne. Cela nous fera gagner beaucoup de temps à l'avenir si jamais nous revisitons le code et devons le comprendre.

Avec cela, nous devons comprendre la structure de base d'un fichier Assembly.
