## Registres

Dans les processeurs 64 bits, le nom du registre commencera par un "R" au lieu d'un "E" (EAX → RAX) et le registre aura une taille de 64 bits. Plus l’architecture est haute, plus la taille des registres augmente.

### Introduction

- Les registres sont des emplacements mémoire à l’intérieur du processeur permettant le stockage d’information, le pointage mémoire ou la gestion de la pile mémoire.
- Ce sont les emplacements les plus rapides d’accès (car interne au processeur).
- Le nom et la taille des registres varient en fonction de l’architecture utilisée (X86 ou X64).
- Le nom des registres x86 est similaire aux registres x64.

### Registres x86

- **Registres généraux (stockage des résultats)**: EAX, EBX, ECX, EDX.
- **Registres d’index (manipulations de strings ou copies mémoire)**: EDI, ESI, EBP, ESP.
- **Registres de segments (accès aux instructions ou données du programme)**: CS, DS, ES, SS.
- **Registre FLAGS (indicateurs d’état d’opération, comparaison, etc ..)**: Registre composé de plusieurs flags d’états : OF, DF, IF, TF, SF, ZF, AF, PF, CF.
- **Registre de pointeurs (utilisé pour connaître la prochaine instruction à exécuter)**: EIP ( utilisé aussi conjointement avec CS).

### Type de registre

- **EAX** est utilisé pour les calculs ou stocker les valeurs des returns ou syscalls.
- **EBX** est souvent utilisé pour accéder aux tableaux.
- **ECX** est souvent utilisé comme compteur pour les boucles.
- **EDX** est utilisé en complément de EAX pour stocker davantage d’informations.

### Registres de travaux

- **ESI** est souvent utilisé comme un pointeur de source dans des opérations de chaînes de caractères, où il pointe vers la source des données à lire.
- **EDI** est utilisé comme un pointeur de destination dans des opérations de chaînes, où il pointe vers la destination des données à écrire.
- **ESP** est le pointeur de pile qui tracke le sommet de la pile dans la mémoire. Il est automatiquement modifié par des instructions de push, pop, call, et return. C'est un registre crucial pour la gestion de la pile d'exécution et le suivi du contexte d'exécution des fonctions.
- **EBP** facilite l'accès aux variables passées à une fonction et aux variables locales dans la fonction. Garder la base de la pile permet à d'autres opérations de manipuler ESP tout en maintenant un accès facile à tous les paramètres de fonction et variables locales.
- **EIP** est un pointeur vers la prochaine instruction à exécuter.

### Registres d’index

Chaque chiffre hexadécimal a une taille de 4 bits et est appelé quartet. Par exemple, le nombre binaire 01011101 se traduit par 5D en hexadécimal. Le chiffre 5 (0101) et le chiffre D (1101) sont les quartets. Outre les octets, il existe d'autres types de données, comme un mot, qui est de 2 octets (16 bits) dans taille, un double mot (dword) est de 4 octets (32 bits) et un quadword (qword) est de 8 octets (64 bits).

### Unités de mesure

Les unités de mesure de données en termes de taille mémoire sont :
- **Quartet (nibble)** : 1/2 octet > 4 bits > 0x0
- **Byte** : 1 octet > 8 bits > 0x00
- **Word** : 2 octets > 16 bits > 0x0000
- **DWord** : 4 octets > 32 bits > 0x00000000
- **QWord** : 8 octets > 64 bits > 0x0000000000000000

Les termes ci-présents sont des unités de mesures de données en termes de taille mémoire :
- **Char** : 1 octet (8 bits).
- **Int** : Il existe des entiers 16 bits, 32 bits et 64 bits.
- **Booléen** : 1 octet.

### Little Endian et Big Endian

- Little endian et Big endian (Boutisme) sont deux conventions d'encodage des données numériques sur des systèmes informatiques.
- Elles définissent l'ordre dans lequel les octets (unités de 8 bits) d'un nombre entier sont stockés en mémoire.
- Il est important de savoir quelle convention d'encodage est utilisée par votre système car cela peut affecter la manière dont les données sont interprétées et manipulées dans des situations telles que la transmission de données entre des systèmes utilisant des conventions différentes.

