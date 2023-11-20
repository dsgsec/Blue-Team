L'architecture des ordinateurs
==============================

* * * * *

Aujourd'hui, la plupart des ordinateurs modernes sont construits sur ce que l'on appelle l' [architecture Von Neumann](https://en.wikipedia.org/wiki/Von_Neumann_architecture) , qui a été développée en 1945 pour `Von Neumann`permettre la création d'« ordinateurs à usage général » comme `Alan Turing`les décrivaient à l'époque. `Alan Turing`à son tour, il a basé ses idées sur `Charles Babbage`le concept d'« ordinateur programmable » du milieu du XIXe siècle. Notez que toutes ces personnes étaient des mathématiciens.

Cette architecture exécute du code machine pour exécuter des algorithmes spécifiques. Il se compose principalement des éléments suivants :

-   Unité centrale de traitement (CPU)
-   Unité de mémoire
-   Périphériques d'entrée/sortie
    -   Unité de stockage de masse
    -   Clavier
    -   Afficher

De plus, le processeur lui-même se compose de trois composants principaux :

-   Unité de contrôle (CU)
-   Unité arithmétique/logique (ALU)
-   Registres

![Architecture Von Neumann](https://academy.hackthebox.com/storage/modules/85/von_neumann_arch.jpg)

Bien que très ancienne, cette architecture constitue toujours la base de la plupart des ordinateurs, serveurs et même smartphones modernes.

Les langages assembleurs fonctionnent principalement avec le CPU et la mémoire. C'est pourquoi il est crucial de comprendre la conception générale de l'architecture informatique. Ainsi, lorsque nous commençons à utiliser des instructions d'assemblage pour déplacer et traiter des données, nous savons d'où elles vont et viennent et à quelle vitesse/coûteuse chaque instruction est.

De plus, l'exploitation binaire de base et avancée nécessite une bonne compréhension de l'architecture informatique. Avec les débordements de pile de base, il suffit de connaître la conception générale. Une fois que nous commençons à utiliser les exploits ROP et Heap, notre compréhension devrait être approfondie. Examinons maintenant de plus près certains composants essentiels.

* * * * *

Mémoire
-------

La mémoire d'un ordinateur est l'endroit où `temporary`se trouvent les données et les instructions des programmes en cours d'exécution. La mémoire d'un ordinateur est également appelée mémoire primaire. Il s'agit de l'emplacement principal utilisé par le processeur pour récupérer et traiter les données. Cela se produit très fréquemment (des milliards de fois par seconde), la mémoire doit donc être extrêmement rapide pour stocker et récupérer les données et les instructions.

Il existe deux principaux types de mémoire :

1.  `Cache`
2.  `Random Access Memory (RAM)`

#### Cache

La mémoire cache est généralement située dans le processeur lui-même et est donc extrêmement rapide par rapport à la RAM, car elle fonctionne à la même vitesse d'horloge que le processeur. Cependant, sa taille est très limitée, très sophistiquée et coûteuse à fabriquer en raison de sa proximité avec le cœur du processeur.

Étant donné que la vitesse d'horloge de la RAM est généralement beaucoup plus lente que celle des cœurs du processeur, en plus d'être éloignée du processeur, si un processeur devait attendre que la RAM récupère chaque instruction, il fonctionnerait effectivement à des vitesses d'horloge beaucoup plus faibles. C'est le principal avantage de la mémoire cache. Il permet au processeur d'accéder aux instructions et aux données à venir plus rapidement que de les récupérer de la RAM.

Il existe généralement trois niveaux de mémoire cache, en fonction de leur proximité avec le cœur du processeur :

| Niveau | Description |
| --- | --- |
| `Level 1 Cache` | Généralement en kilo-octets, la mémoire la plus rapide disponible, située dans chaque cœur de processeur. (Seuls les registres sont plus rapides.) |
| `Level 2 Cache` | Généralement en mégaoctets, extrêmement rapide (mais plus lent que L1), partagé entre tous les cœurs du processeur. |
| `Level 3 Cache` | Généralement en mégaoctets (plus grand que L2), plus rapide que la RAM mais plus lent que L1/L2. (Tous les processeurs n'utilisent pas L3.) |

#### RAM

La RAM est beaucoup plus grande que la mémoire cache, et sa taille varie de gigaoctets à téraoctets. La RAM est également située loin des cœurs du processeur et est beaucoup plus lente que la mémoire cache. L'accès aux données à partir des adresses RAM nécessite beaucoup plus d'instructions.

Par exemple, récupérer une instruction dans les registres ne prend qu'un seul cycle d'horloge, et la récupérer depuis le cache L1 prend quelques cycles, tandis que la récupérer depuis la RAM prend environ 200 cycles. Lorsque cela est effectué des milliards de fois par seconde, cela fait une énorme différence dans la vitesse d'exécution globale.

Dans le passé, avec les adresses 32 bits, les adresses mémoire étaient limitées de `0x00000000`à `0xffffffff`. Cela signifiait que la taille maximale possible de la RAM était de 2 à 32 octets, soit seulement 4 gigaoctets, auquel cas nous manquions d'adresses uniques. Avec des adresses 64 bits, la plage atteint désormais `0xffffffffffffffff`, avec une taille de RAM maximale théorique de 2,64 octets , soit environ 18,5 exaoctets (18,5 millions de téraoctets), nous ne devrions donc pas manquer d'adresses mémoire de si tôt.

![](https://academy.hackthebox.com/storage/modules/85/memory_structure.jpg)

Lorsqu'un programme est exécuté, toutes ses données et instructions sont déplacées de l'unité de stockage vers la RAM pour être accessibles en cas de besoin par le processeur. Cela se produit parce que leur accès depuis l'unité de stockage est beaucoup plus lent et augmentera les temps de traitement des données. Lorsqu'un programme est fermé, ses données sont supprimées ou mises à disposition pour être réutilisées à partir de la RAM.

Comme on peut le voir, la RAM est divisée en quatre principaux `segments`:

| Segment | Description |
| --- | --- |
| `Stack` | A une conception dernier entré, premier sorti (LIFO) et est de taille fixe. Les données qu'il contient ne sont accessibles que dans un ordre spécifique en poussant et en affichant des données. |
| `Heap` | A une conception hiérarchique et est donc beaucoup plus grand et plus polyvalent dans le stockage des données, car les données peuvent être stockées et récupérées dans n'importe quel ordre. Cependant, cela rend le tas plus lent que la pile. |
| `Data` | Comporte deux parties : `Data`, qui est utilisée pour contenir les variables, et `.bss`, qui est utilisée pour contenir les variables non affectées (c'est-à-dire, la mémoire tampon pour une allocation ultérieure). |
| `Text` | Les instructions d'assemblage principales sont chargées dans ce segment pour être récupérées et exécutées par le CPU. |

Bien que cette segmentation s'applique à l'ensemble de la RAM, `each application is allocated its Virtual Memory when it is run`. Cela signifie que chaque application aurait ses propres segments `stack`, `heap`, `data`et .`text`

* * * * *

E/S/stockage
------------

Enfin, nous avons les périphériques d'entrée/sortie, comme le clavier, l'écran ou encore l'unité de stockage longue durée, également appelée mémoire secondaire. Le processeur peut accéder et contrôler les périphériques IO à l'aide de `Bus Interfaces`, qui agissent comme des « autoroutes » pour transférer des données et des adresses, en utilisant des charges électriques pour les données binaires.

Chaque bus a une capacité de bits (ou de charges électriques) qu'il peut transporter simultanément. Il s'agit généralement d'un multiple de 4 bits, allant jusqu'à 128 bits. Les interfaces de bus sont également généralement utilisées pour accéder à la mémoire et à d'autres composants en dehors du processeur lui-même. Si nous regardons de plus près un processeur ou une carte mère, nous pouvons voir les interfaces de bus partout :

![bus](https://academy.hackthebox.com/storage/modules/85/cpu_bus.jpg)

Contrairement à la mémoire primaire qui est volatile et stocke des données et des instructions temporaires pendant l'exécution des programmes, l'unité de stockage stocke des données permanentes, telles que les fichiers du système d'exploitation ou des applications entières et leurs données.

L'unité de stockage est la plus lente d'accès. Premièrement, comme ils sont les plus éloignés du processeur, leur accès via des interfaces de bus telles que SATA ou USB prend beaucoup plus de temps pour stocker et récupérer les données. Ils sont également plus lents dans leur conception pour permettre davantage de stockage de données. Tant qu'il y aura plus de données à traiter, elles seront plus lentes.

Ces dernières années, les unités de stockage magnétiques classiques, comme les bandes ou les disques durs (HDD), sont passées aux disques SSD (Solid State Drives). En effet, les SSD utilisent une conception similaire à celle des RAM, utilisant des circuits non volatils qui conservent les données même sans électricité. Cela a rendu les unités de stockage beaucoup plus rapides dans le stockage et la récupération des données. Néanmoins, comme ils sont éloignés du processeur et connectés via des interfaces spéciales, ils constituent l'unité la plus lente d'accès.

* * * * *

Vitesse
-------

Comme nous pouvons le voir ci-dessus, plus un composant est éloigné du cœur du processeur, plus il est lent. De plus, plus il peut contenir de données, plus il est lent, car il doit simplement en parcourir davantage pour récupérer les données. Le tableau ci-dessous résume chaque composant, sa taille et sa vitesse :

| Composant | Vitesse | Taille |
| --- | --- | --- |
| `Registers` | Le plus rapide | Octets |
| `L1 Cache` | Le plus rapide, autre que les registres | Kilooctets |
| `L2 Cache` | Très vite | Mégaoctets |
| `L3 Cache` | Rapide, mais plus lent que ci-dessus | Mégaoctets |
| `RAM` | Beaucoup plus lent que tout ce qui précède | Gigaoctets-Téraoctets |
| `Storage` | Le plus lent | Téraoctets et plus |

La vitesse ici est relative en fonction de la vitesse d'horloge du processeur. Maintenant que nous avons une idée générale de l'architecture informatique, nous aborderons les registres et l'architecture du processeur dans la section suivante.