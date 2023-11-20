Registres, adresses et types de données
=======================================

* * * * *

Maintenant que nous comprenons l'architecture générale des ordinateurs et des processeurs, nous devons comprendre quelques éléments d'assemblage avant de commencer à apprendre Assembly : `Registers`, `Memory Addresses`, `Address Endianness`, et `Data Types`. Chacun de ces éléments est important, et une bonne compréhension nous aidera à éviter des problèmes et des heures de dépannage lors de l'écriture et du débogage du code assembleur.

* * * * *

Registres
---------

Comme mentionné précédemment, chaque cœur de processeur possède un ensemble de registres. Les registres sont les composants les plus rapides de tout ordinateur, car ils sont intégrés au cœur du processeur. Cependant, les registres sont de taille très limitée et ne peuvent contenir que quelques octets de données à la fois. Il existe de nombreux registres dans l'architecture x86, mais nous nous concentrerons uniquement sur ceux nécessaires à l'apprentissage de l'Assembly de base et indispensables à l'exploitation future du binaire.

Il existe deux principaux types de registres sur lesquels nous nous concentrerons : `Data Registers`et `Pointer Registers`.

| Registres de données | Registres de pointeurs |
| --- | --- |
| `rax` | `rbp` |
| `rbx` | `rsp` |
| `rcx` | `rip` |
| `rdx` |  |
| `r8` |  |
| `r9` |  |
| `r10` |  |

-   `Data Registers`- sont généralement utilisés pour stocker les instructions/arguments d'appel système. Les principaux registres de données sont : `rax`, `rbx`, `rcx`et `rdx`. Les registres `rdi`et `rsi`existent également et sont généralement utilisés pour l'instruction `destination`et `source`les opérandes. Ensuite, nous avons des registres de données secondaires qui peuvent être utilisés lorsque tous les registres précédents sont utilisés, à savoir `r8`, `r9`, et `r10`.

-   `Pointer Registers`- sont utilisés pour stocker des pointeurs d'adresse importants spécifiques. Les principaux registres de pointeurs sont le Base Stack Pointer `rbp`, qui pointe vers le début de la Stack, le Current Stack Pointer `rsp`, qui pointe vers l'emplacement actuel dans la Stack (en haut de la Stack) et le Instruction Pointer `rip`, qui contient l'adresse de la prochaine instruction.

* * * * *

Sous-registres
--------------

Chaque `64-bit`registre peut être divisé en sous-registres plus petits contenant les bits inférieurs, à un octet `8-bits`, 2 octets `16-bits`et 4 octets `32-bits`. Chaque sous-registre peut être utilisé et accessible indépendamment, nous n'avons donc pas besoin de consommer la totalité des 64 bits si nous disposons d'une plus petite quantité de données.

![assembly_register_parts](https://github.com/dsgsec/Blue-Team/assets/82456829/0b41cb4d-7417-437d-99c5-85fdfda4a2b7)

Les sous-registres sont accessibles comme :

| Taille en bits | Taille en octets | Nom | Exemple |
| --- | --- | --- | --- |
| `16-bit` | `2 bytes` | le nom de base | `ax` |
| `8-bit` | `1 bytes` | nom de base et/ou se termine par`l` | `al` |
| `32-bit` | `4 bytes` | nom de base + commence par le `e`préfixe | `eax` |
| `64-bit` | `8 bytes` | nom de base + commence par le `r`préfixe | `rax` |

Par exemple, pour le `bx`registre de données, le 16 bits est `bx`, donc le 8 bits est `bl`, le 32 bits serait `ebx`, et le 64 bits serait `rbx`. Il en va de même pour les registres de pointeurs. Si nous prenons le pointeur de pile de base `bp`, son sous-registre de 16 bits est `bp`, donc le 8 bits est `bpl`, le 32 bits est `ebp`, et le 64 bits est `rbp`.

Voici les noms des sous-registres pour tous les registres essentiels dans une architecture x86_64 :

| Description | Registre 64 bits | Registre 32 bits | Registre 16 bits | Registre 8 bits |
| --- | --- | --- | --- | --- |
| Registres de données/arguments |  |  |  |  |
| Numéro d'appel système/valeur de retour | `rax` | `eax` | `ax` | `al` |
| Appelé enregistré | `rbx` | `ebx` | `bx` | `bl` |
| 1er argument - Opérande de destination | `rdi` | `edi` | `di` | `dil` |
| 2ème argument - Opérande source | `rsi` | `esi` | `si` | `sil` |
| 3ème argument | `rdx` | `edx` | `dx` | `dl` |
| 4ème argument -- ​​Compteur de boucles | `rcx` | `ecx` | `cx` | `cl` |
| 5ème argument | `r8` | `r8d` | `r8w` | `r8b` |
| 6ème argument | `r9` | `r9d` | `r9w` | `r9b` |
| Registres de pointeurs |  |  |  |  |
| Pointeur de pile de base | `rbp` | `ebp` | `bp` | `bpl` |
| Pointeur de pile actuel/supérieur | `rsp` | `esp` | `sp` | `spl` |
| Pointeur d'instruction 'appel uniquement' | `rip` | `eip` | `ip` | `ipl` |

Au fil du module, nous verrons comment utiliser chacun de ces registres.

Il existe d'autres registres, mais nous ne les aborderons pas dans ce module, car ils ne sont pas nécessaires à l'utilisation de base de Assembly. À titre d'exemple, il y a le `RFLAGS`registre, qui est utilisé pour conserver divers indicateurs utilisés par le CPU, comme l'indicateur zéro `ZF`, qui est utilisé pour les instructions conditionnelles.

* * * * *

Adresses mémoire
----------------

Comme mentionné précédemment, les processeurs x86 64 bits ont des adresses de 64 bits allant de `0x0`à `0xffffffffffffffff`, nous nous attendons donc à ce que les adresses se situent dans cette plage. Cependant, la RAM est segmentée en diverses régions, comme la pile, le tas et d'autres régions spécifiques au programme et au noyau. Chaque région de mémoire possède des autorisations spécifiques qui spécifient si nous pouvons y lire, y écrire ou y appeler une `read`adresse `write`.`execute`

Chaque fois qu'une instruction passe par le cycle d'instructions pour être exécutée, la première étape consiste à récupérer l'instruction à l'adresse à laquelle elle se trouve, comme indiqué précédemment. Il existe plusieurs types de récupération d'adresses (c'est-à-dire les modes d'adressage) dans l'architecture x86 :

| Mode d'adressage | Description | Exemple |
| --- | --- | --- |
| `Immediate` | La valeur est donnée dans l'instruction | `add 2` |
| `Register` | Le nom du registre qui contient la valeur est donné dans l'instruction | `add rax` |
| `Direct` | L'adresse complète directe est indiquée dans les instructions | `call 0xffffffffaa8a25ff` |
| `Indirect` | Un pointeur de référence est donné dans l'instruction | `call 0x44d000`ou`call [rax]` |
| `Stack` | L'adresse est en haut de la pile | `add rsp` |

Dans le tableau ci-dessus, plus bas est plus lent. Moins la valeur est immédiate, plus sa récupération est lente.

Même si la vitesse n'est pas notre plus grande préoccupation lors de l'apprentissage de base de l'Assembly, nous devons comprendre où et comment se trouve chaque adresse. Avoir cette compréhension nous aidera dans les futures exploitations binaires, avec les exploits Buffer Overflow, par exemple. La même compréhension aura une implication encore plus significative avec l'exploitation binaire avancée, comme l'exploitation ROP ou Heap.

* * * * *

Aborder l'endianité
-------------------

L'endianité d'une adresse est l'ordre de ses octets dans lesquels ils sont stockés ou récupérés de la mémoire. Il existe deux types d'endianité : `Little-Endian`et `Big-Endian`. Avec les processeurs Little-Endian, l'octet de petite extrémité de l'adresse est rempli/récupéré en premier `right-to-left`, tandis qu'avec les processeurs Big-Endian, l'octet de grande extrémité est rempli/récupéré en premier `left-to-right`.

Par exemple, si nous avons l'adresse `0x0011223344556677`à stocker en mémoire, les processeurs little-endian stockeraient l' `0x00`octet sur les octets les plus à droite, puis l' `0x11`octet serait rempli après, il deviendrait donc `0x1100`, puis l' `0x22`octet, donc il devient `0x221100`, et ainsi de suite. Une fois que tous les octets sont en place, ils ressembleront à `0x7766554433221100`, ce qui est l'inverse de la valeur d'origine. Bien entendu, lors de la récupération de la valeur, le processeur utilisera également la récupération petit-boutiste, de sorte que la valeur récupérée sera la même que la valeur d'origine.

Un autre exemple qui montre comment cela peut affecter les valeurs stockées est le binaire. Par exemple, si nous avions l'entier sur 2 octets `426`, sa représentation binaire est `00000001 10101010`. L'ordre dans lequel ces deux octets sont stockés modifierait sa valeur. Par exemple, si nous le stockons à l'envers sous la forme `10101010 00000001`, sa valeur devient `43521`.

Les processeurs big-endian stockeraient ces octets sous la forme `00000001 10101010` `left-to-right`, tandis que les processeurs small-endian les stockeraient sous la forme `10101010 00000001` `right-to-left`. Lors de la récupération de la valeur, le processeur doit utiliser le même endianisme que celui utilisé lors de leur stockage, sinon il obtiendra une valeur erronée. Cela indique que l'ordre dans lequel les octets sont stockés/récupérés fait une grande différence.

Le tableau suivant montre comment fonctionne l'endianité :

| Adresse | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | Valeur de l'adresse |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| Petit endian | 77 | 66 | 55 | 44 | 33 | 22 | 11 | 00 | 0x |
| Gros boutien | 00 | 11 | 22 | 33 | 44 | 55 | 66 | 77 | 0x |

Adresse : 0x0011223344556677

Charger l'adresse

Vous pouvez cliquer sur le bouton "Load Address" pour visualiser comment chaque endianness charge les données/adresses en mémoire.

Comme on peut le voir, cela signifie qu'une adresse écrite en petit-boutiste ou en gros-boutiste ferait référence à des emplacements différents dans la mémoire, car elle serait lue différemment par chaque type de processeur.

Ce module utilisera toujours l'ordre des octets petit-boutiste, comme il est utilisé avec Intel/AMD x86 dans la plupart des systèmes d'exploitation modernes, de sorte que le shellcode est toujours représenté de droite à gauche.

`The important thing we need to take from this is knowing that our bytes are stored into memory from right-to-left.`Ainsi, si nous devions pousser une adresse ou une chaîne avec Assembly, nous devrions la pousser à l'envers. Par exemple, si nous voulons stocker le mot `Hello`, nous pousserions ses octets à l'envers : `o`, `l`, `l`, `e`et enfin `H`.

Cela peut sembler un peu contre-intuitif puisque la plupart des gens sont habitués à lire de gauche à droite. Cependant, cela présente de multiples avantages lors du traitement des données, comme pouvoir récupérer un sous-registre sans avoir à parcourir tout le registre ou pouvoir effectuer des calculs dans le bon ordre de droite à gauche.

* * * * *

Types de données
----------------

Enfin, l'architecture x86 prend en charge de nombreux types de tailles de données, qui peuvent être utilisées avec diverses instructions. Voici les types de données les plus courants que nous utiliserons avec des instructions :

| Composant | Longueur | Exemple |
| --- | --- | --- |
| `byte` | 8 bits | `0xab` |
| `word` | 16 bits - 2 octets | `0xabcd` |
| `double word (dword)` | 32 bits - 4 octets | `0xabcdef12` |
| `quad word (qword)` | 64 bits - 8 octets | `0xabcdef1234567890` |

`Whenever we use a variable with a certain data type or use a data type with an instruction, both operands should be of the same size.`

Par exemple, nous ne pouvons pas utiliser une variable définie comme `byte`with `rax`, car `rax`elle a une taille de 8 octets. Dans ce cas, nous devrons utiliser `al`, qui a la même taille de 1 octet. Le tableau suivant montre le type de données approprié pour chaque sous-registre :

| Sous-registre | Type de données |
| --- | --- |
| `al` | `byte` |
| `ax` | `word` |
| `eax` | `dword` |
| `rax` | `qword` |

Nous en discuterons plus en détail dans les prochaines sections. Une fois tous les principes fondamentaux de l'Assembly abordés, nous pouvons commencer à en apprendre davantage sur les instructions d'assemblage x86 et à écrire du code Assembly de base.
