Architecture du processeur
==========================

* * * * *

L'unité centrale de traitement (CPU) est la principale unité de traitement d'un ordinateur. Le CPU contient à la fois le `Control Unit`(CU), qui est chargé du déplacement et du contrôle des données, et le `Arithmetic/Logic Unit`(ALU), qui est chargé d'effectuer divers calculs arithmétiques et logiques demandés par un programme via les instructions d'assemblage.

La manière et l'efficacité avec lesquelles un processeur traite ses instructions dépendent de son `Instruction Set Architecture`(ISA). Il existe plusieurs ISA dans l'industrie, chacune ayant sa manière de traiter les données. `RISC`L'architecture est basée sur le traitement d'instructions plus simples, qui prennent plus de cycles, mais chaque cycle est plus court et consomme moins d'énergie. L' `CISC`architecture est basée sur moins d'instructions plus complexes, qui peuvent terminer les instructions demandées en moins de cycles, mais chaque instruction prend plus de temps et de puissance à traiter.

Jetons un coup d'œil à `RISC`et `CISC`et apprenons-en davantage sur les cycles d'instructions et les registres.

* * * * *

Vitesse d'horloge et cycle d'horloge
------------------------------------

Chaque processeur a une vitesse d'horloge qui indique sa vitesse globale. Chaque tick de l'horloge exécute un cycle d'horloge qui traite une instruction de base, telle que la récupération d'une adresse ou le stockage d'une adresse. Plus précisément, cela est effectué par la CU ou l'ALU.

La fréquence à laquelle les cycles se produisent est comptée en cycles par seconde ( `Hertz`). Si un processeur a une vitesse de 3,0 GHz, il peut exécuter 3 milliards de cycles par seconde (par cœur).

![cycle d'enseignement](https://academy.hackthebox.com/storage/modules/85/assembly_clock_cycle_0.jpg)

Les processeurs modernes ont une conception multicœur, ce qui leur permet d'avoir plusieurs cycles en même temps.

* * * * *

Cycle d'enseignement
--------------------

An `Instruction Cycle`est le cycle nécessaire au processeur pour traiter une seule instruction machine.

![cycle d'enseignement](https://academy.hackthebox.com/storage/modules/85/assembly_instruction_cycle.jpg)

Un cycle d'instruction se compose de quatre étapes : `Fetch`, `Decode`, `Execute`, et `Store`:

| Instruction | Description |
| --- | --- |
| `1\. Fetch` | Prend l'adresse de l'instruction suivante du `Instruction Address Register`(IAR), qui lui indique où se trouve l'instruction suivante. |
| `2\. Decode` | Prend l'instruction de l'IAR et la décode en binaire pour voir ce qui doit être exécuté. |
| `3\. Execute` | Récupérez les opérandes d'instruction à partir du registre/de la mémoire et traitez l'instruction dans le `ALU`ou `CU`. |
| `4\. Store` | Stockez la nouvelle valeur dans l'opérande de destination. |

Toutes les étapes du cycle d'instructions sont réalisées par l'Unité de Contrôle, sauf lorsqu'il faut exécuter des instructions arithmétiques "add, sub, ..etc", qui sont exécutées par l'ALU.

Chaque cycle d'instruction prend plusieurs cycles d'horloge pour se terminer, en fonction de l'architecture du processeur et de la complexité de l'instruction. Une fois qu'un seul cycle d'instruction est terminé, la CU passe à l'instruction suivante et exécute le même cycle dessus, et ainsi de suite.

![cycle d'enseignement](https://academy.hackthebox.com/storage/modules/85/assembly_clock_cycle_1.jpg)

Par exemple, si nous devions exécuter l'instruction d'assemblage `add rax, 1`, elle suivrait un cycle d'instruction :

1.  Récupérez l'instruction dans le `rip`registre `48 83 C0 01`(en binaire).
2.  Décodez ' `48 83 C0 01`' pour savoir qu'il doit effectuer un `add`of `1`sur la valeur à `rax`.
3.  Obtenez la valeur actuelle à `rax`(par `CU`), ajoutez `1`-y (par le `ALU`).
4.  Stockez la nouvelle valeur dans `rax`.

Dans le passé, les processeurs traitaient les instructions de manière séquentielle, ils devaient donc attendre la fin d'une instruction pour démarrer la suivante. D'un autre côté, les processeurs modernes peuvent traiter plusieurs instructions en parallèle en exécutant plusieurs cycles d'instructions/d'horloge en même temps. Ceci est rendu possible grâce à une conception multithread et multicœur.

![cycle d'enseignement](https://academy.hackthebox.com/storage/modules/85/assembly_clock_cycle_2.jpg)

* * * * *

Spécifique au processeur
------------------------

Comme mentionné précédemment, chaque processeur comprend un ensemble d'instructions différent. Par exemple, alors qu'un processeur Intel basé sur l'architecture x86 64 bits peut interpréter le code machine `4883C001`comme `add rax, 1`, un processeur ARM traduit le même code machine que l' `biceq r8, r0, r8, asr #6`instruction. Comme nous pouvons le constater, le même code machine exécute une instruction totalement différente sur chaque processeur.

En effet, chaque type de processeur possède une architecture de langage assembleur de bas niveau différente appelée `Instruction Set Architectures`(ISA). Par exemple, l'instruction d'ajout vue ci-dessus `add rax, 1`concerne les processeurs Intel x86 64 bits. La même instruction écrite pour le langage d'assemblage du processeur ARM est représentée par `add r1, r1, 1`.

`It is important to understand that each processor has its own set of instructions and corresponding machine code.`

De plus, une seule architecture de jeu d'instructions peut avoir plusieurs interprétations syntaxiques pour le même code assembleur. Par exemple, les `add`instructions ci-dessus sont basées sur l'architecture x86, qui est prise en charge par plusieurs processeurs tels qu'Intel, AMD et les anciens processeurs AT&T. L'instruction est écrite `add rax, 1`avec la syntaxe Intel et `addb $0x1,%rax`avec la syntaxe AT&T.

Comme nous pouvons le voir, même si nous pouvons dire que les deux instructions sont similaires et font la même chose, leur syntaxe est différente et les emplacements des opérandes source et destination sont également inversés. Pourtant, les deux codes assemblent le même code machine et exécutent la même instruction.

`So, each processor type has its Instruction Set Architectures, and each architecture can be further represented in several syntax formats`

Ce module se concentrera principalement sur le langage assembleur Intel x86 64 bits (également connu sous le nom de x86_64 et AMD64), car la majorité des ordinateurs et serveurs modernes fonctionnent sur cette architecture de processeur. Nous utiliserons également la syntaxe Intel.

Si nous voulons savoir si notre système Linux prend en charge `x86_64`l'architecture, nous pouvons utiliser la `lscpu`commande :

```
dsgsec@htb[/htb]$ lscpu

Architecture:                    x86_64
CPU op-mode(s):                  32-bit, 64-bit
Byte Order:                      Little Endian

<SNIP>

```

Comme nous pouvons le voir dans le résultat ci-dessus, l'architecture du processeur est `x86_64`et prend en charge 32 bits et 64 bits. L'ordre des octets est Little Endian. Nous pouvons également utiliser la `uname -m`commande pour obtenir l'architecture du CPU. Nous aborderons les deux architectures de jeu d'instructions les plus courantes dans la section suivante : `CISC`et `RISC`.