Architectures de jeux d'instructions
====================================

* * * * *

Un `Instruction Set Architecture`( `ISA`) spécifie la syntaxe et la sémantique du langage assembleur sur chaque architecture. Il ne s'agit pas simplement d'une syntaxe différente, mais elle est intégrée à la conception de base d'un processeur, car elle affecte la manière dont les instructions sont exécutées ainsi que leur niveau de complexité. `ISA`se compose principalement des éléments suivants :

-   Instructions
-   Registres
-   Adresses mémoire
-   Types de données

| Composant | Description | Exemple |
| --- | --- | --- |
| `Instructions` | Instruction à traiter au `opcode operand_list`format. Il y a généralement 1, 2 ou 3 opérandes séparés par des virgules. | `add rax, 1`, `mov rsp, rax`,`push rax` |
| `Registers` | Utilisé pour stocker temporairement des opérandes, des adresses ou des instructions. | `rax`, `rsp`,`rip` |
| `Memory Addresses` | L'adresse à laquelle les données ou les instructions sont stockées. Peut pointer vers la mémoire ou les registres. | `0xffffffffaa8a25ff`, `0x44d0`,`$rax` |
| `Data Types` | Le type de données stockées. | `byte`, `word`,`double word` |

Ce sont les principaux composants qui distinguent les différents langages ISA et assembleurs. Nous aborderons chacun d'eux plus en profondeur dans les sections à venir et nous apprendrons comment utiliser diverses instructions.

Il existe deux principales architectures de jeux d'instructions largement utilisées :

1.  `Complex Instruction Set Computer`( `CISC`) - Utilisé dans `Intel`les `AMD`processeurs de la plupart des ordinateurs et serveurs.

2.  `Reduced Instruction Set Computer`( `RISC`) - Utilisé dans les processeurs `ARM`et `Apple`dans la plupart des smartphones et certains ordinateurs portables modernes.

Voyons les avantages et les inconvénients de chacun ainsi que les principales différences entre eux.

* * * * *

ICSC
----

L'architecture CISC a été l'une des premières ISA jamais développées. Comme son nom l'indique, l'architecture CISC favorise l'exécution simultanée d'instructions plus complexes afin de réduire le nombre total d'instructions. Ceci est fait pour s'appuyer autant que possible sur le CPU en combinant des instructions mineures en instructions plus complexes.

Par exemple, supposons que nous devions ajouter deux registres avec l' `add rax, rbx`instruction « ». Dans ce cas, un processeur CISC peut le faire en un seul cycle d'instructions 'Fetch-Decode-Execute-Store', sans avoir à le diviser en plusieurs instructions pour les récupérer, puis les récupérer, puis les ajouter, puis les stocker `rax`dans `rbx``rax. , dont chacun prendrait son propre cycle d'instructions « Fetch-Decode-Execute-Store ».

Deux raisons principales ont conduit à cela :

1.  Permettre à davantage d'instructions d'être exécutées simultanément en concevant le processeur pour qu'il exécute des instructions plus avancées dans son noyau.
2.  Dans le passé, la mémoire et les transistors étaient limités, il était donc préférable d'écrire des programmes plus courts en combinant plusieurs instructions en une seule.

Pour permettre aux processeurs d'exécuter des instructions complexes, la conception du processeur devient plus compliquée, car elle est conçue pour exécuter une grande quantité d'instructions complexes différentes, chacune possédant sa propre unité pour l'exécuter.

De plus, même s'il faut un seul cycle d'instruction pour exécuter une seule instruction, les instructions étant plus complexes, chaque cycle d'instruction prend plus de cycles d'horloge. Ce fait entraîne une consommation d'énergie et une chaleur accrues pour exécuter chaque instruction.

* * * * *

RISQUE
------

L'architecture RISC favorise le fractionnement des instructions en instructions mineures, et le CPU est donc conçu uniquement pour gérer des instructions simples. Ceci est fait pour relayer l'optimisation au logiciel en écrivant le code assembleur le plus optimisé.

Par exemple, la même `add r1, r2, r3`instruction précédente sur un processeur RISC récupérerait `r2`, puis récupérerait `r3`, les ajouterait et enfin les stockerait dans `r1`. Chaque instruction de ceux-ci prend un cycle d'instructions complet « Fetch-Decode-Execute-Store », ce qui conduit, comme on peut s'y attendre, à un plus grand nombre d'instructions totales par programme, et donc à un code assembleur plus long.

En ne prenant pas en charge différents types d'instructions complexes, les processeurs RISC ne prennent en charge qu'un nombre limité d'instructions ( `~200`) par rapport aux processeurs CISC ( `~1500`). Ainsi, pour exécuter des instructions complexes, cela doit être fait via une combinaison d'instructions mineures via Assembly.

On dit qu'on peut construire un ordinateur polyvalent avec un processeur qui ne supporte qu'une seule instruction ! Cela indique que nous pouvons créer des instructions très complexes en utilisant `sub`uniquement l'instruction. Pouvez-vous imaginer comment cela pourrait être réalisé ?

D'un autre côté, l'avantage de diviser les instructions complexes en instructions mineures est que toutes les instructions ont la même longueur, soit 32 bits, soit 64 bits. Cela permet de concevoir la vitesse d'horloge du processeur en fonction de la longueur de l'instruction, de sorte que l'exécution de chaque étape du cycle d'instruction prenne toujours précisément un cycle d'horloge machine.

Le diagramme ci-dessous montre comment les instructions CISC prennent un nombre variable de cycles d'horloge, tandis que les instructions RISC prennent un nombre fixe : ![assembly_cisc_risk_cycles](https://github.com/dsgsec/Blue-Team/assets/82456829/0681f7e3-5838-472a-9496-a55dd4a24c5c)


L'exécution de chaque étape d'instruction en un seul cycle d'horloge et l'exécution uniquement d'instructions simples conduisent les processeurs RISC à consommer une fraction de l'énergie consommée par les processeurs CISC, ce qui rend ces processeurs idéaux pour les appareils fonctionnant sur batteries, comme les smartphones et les ordinateurs portables.

* * * * *

CISC contre RISC
----------------

Le tableau suivant résume les principales différences entre CISC et RISC :

| Zone | ICSC | RISQUE |
| --- | --- | --- |
| `Complexity` | Favorise les instructions complexes | Favorise les instructions simples |
| `Length of instructions` | Instructions plus longues - Longueur variable « multiples de 8 bits » | Instructions plus courtes - Longueur fixe « 32 bits/64 bits » |
| `Total instructions per program` | Moins d'instructions totales - Code plus court | Instructions plus complètes - Code plus long |
| `Optimization` | S'appuie sur l'optimisation matérielle (dans le CPU) | S'appuie sur l'optimisation logicielle (en Assembly) |
| `Instruction Execution Time` | Variable - Plusieurs cycles d'horloge | Fixe - Un cycle d'horloge |
| `Instructions supported by CPU` | De nombreuses instructions (~1500) | Moins d'instructions (~200) |
| `Power Consumption` | Haut | Très lent |
| `Examples` | Intel, AMD | BRAS, Apple |

Dans le passé, avoir un code assembleur plus long en raison d'un plus grand nombre d'instructions totales par programme constituait un inconvénient important pour les processeurs RISC en raison des ressources limitées en mémoire et en stockage. Cependant, aujourd'hui, ce n'est plus un problème aussi grave, car la mémoire et le stockage ne sont plus aussi coûteux et limités qu'ils l'étaient par le passé.

De plus, avec de nouveaux assembleurs et compilateurs écrivant du code extrêmement optimisé au niveau logiciel, les processeurs RISC deviennent plus rapides que les processeurs CISC, même dans l'exécution et le traitement d'applications lourdes, tout en consommant beaucoup moins d'énergie.

Tout cela a rendu les processeurs RISC plus courants ces dernières années. RISC pourrait devenir l'architecture dominante dans les années à venir. Mais au moment où nous parlons, l'écrasante majorité des ordinateurs et des serveurs que nous allons tester fonctionnent sur des processeurs Intel/AMD avec l'architecture CISC, ce qui fait de l'apprentissage de l'assemblage CISC notre priorité. Comme les bases de toutes les variantes du langage Assembly sont assez similaires, l'apprentissage d'ARM Assembly devrait être plus simple après avoir terminé ce module.
