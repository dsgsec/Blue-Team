## Instructions d’opération arithmétique

### Add, Sub, Mul

Instructions d’opération arithmétique (addition, soustraction, multiplication).

- `add eax, 4` : Somme de la valeur EAX et de 4 (résultat stocké dans EAX)
- `sub eax, 4` : Soustrait 4 à la valeur EAX (résultat stocké dans EAX)

## Instructions d’opération logique

### And, Or, Xor, Not, Test

Instructions d’opération logique manipulant les données au niveau des bits.

- `xor eax, eax` : Met le registre à 0x0 (une valeur xor par elle-même la met à 0)
- `or eax, 0x1F` : OU logique entre eax et la valeur 0x1F (résultat stocké dans eax)

#### Exemple d’un OR (semblable à une addition)

| Binaire | Décimal |
|---------|---------|
| 0011    | 3       |
| 0101    | 5       |
| **0111**| **8**   |

#### Exemple d’un AND (semblable à une soustraction)

| Binaire | Décimal |
|---------|---------|
| 0011    | 3       |
| 0101    | 5       |
| **0001**| **1**   |

#### Exemple d’un XOR

Imaginons une chaîne en binaire :

`mystring = 01101101 01111001 01110011 01110100 01110010 01101001 01101110 01100111`

si l’on xor cette chaîne avec une clef xor (k = 01101011) :
`Original = 01101101 01111001 01110011 01110100 01110010 01101001 01101110 01100111`
`Key      = 01101011 01101011 01101011 01101011 01101011 01101011 01101011 01101011`

### Le XOR logique est souvent utilisé pour ré-initialiser une variable/registre à 0

`Data xored = 00000110 00010010 00011000 00011111 00011001 00000010 00000101 00001100`


Le XOR peut-être utilisé à des fins cryptographiques/d’obfuscation. Il est souvent utilisé pour mettre une donnée à 0 (une donnée xorée par elle-même donne 0).

### Cmp

Compare les deux valeurs passées en argument (peut être conditionnée également).

- `mov eax, 0x00000042` : Charge la valeur hexadécimale 0x00000042 dans eax

### Mov

Assigne une valeur à une variable (destination -> source).

### Call

Permet d’appeler une fonction.

- `call printf` : Appel la fonction printf

### Jmp

Saut logique pour accéder à l’adresse mémoire appelée (peut-être conditionné).

- `JMP 0x80844264` : Saute à l'adresse 0x80844264
- `cmp eax, 4` : Vérifie si eax est égal à 4

### Int

Permet d’appeler system_call sous Linux.

- `int 0x80` : system_call avec l’argument 0x80

### Instruction CMP

En assembleur, CMP est utilisé pour comparer deux valeurs arithmétiques passées en argument. CMP effectue une opération entre deux opérandes et stocke le résultat (d’une comparaison) dans le registre FLAGS (le résultat de l’opération n’y est pas stocké).

- `cmp opérande1, opérande2` : syntaxe
- `cmp eax, 0` : Vérifie si EAX == 0

Le résultat d’une comparaison est stocké dans le registre FLAGS pour être utilisé plus tard si nécessaire. Les bits du registre FLAGS sont positionnés suivant le résultat de la comparaison.




## Bits FLAGS

Les flags sont positionnés à 1 selon le résultat des opérations arithmétiques :

- Une addition avec une retenue déclenche le carry flag.
- Une opération ayant le résultat 0 déclenche le zero flag.
- Une opération ayant en résultat un nombre négatif déclenche le sign flag.
- Etc...

### CMP - Jump

Les instructions CMP sont souvent suivies d'instructions JMP permettant d'aller vérifier l'état de certains bits du registre FLAGS et d'agir en conséquence.

```assembly
cmp eax, 0          ; Vérifie si EAX == 0
jz 0x891769         ; Jump à l'adresse 0x891769 (Si égal à zero)
mov ebx, 2
jmp 0x471625        ; Jump à l'adresse 0x471625 sinon (else)
```

Plusieurs types de jump existent (liste non exhaustive) :

-   `JMP` : Jump inconditionnel
-   `JE` : Jump if equal
-   `JNE` : Jump if not equal
-   `JLE` : Jump if less or equal
-   `JGE` : Jump if greater or equal
-   `JZ` : Jump if zero

#### Exemple de condition
```
if (EAX == 0)
    EBX = 1;
else
    EBX = 2;
```

```assembly
cmp eax, 10         ; Vérifie si EAX == 10
jge 0x351496        ; Jump si EAX supérieur ou égal à 10
mov ebx, 2
jmp 0x117356        ; Jump à l'adresse 0x117356

0x351496:
mov ebx, 1

0x117356:
```


`if (EAX >= 5)
    EBX = 1;
else
    EBX = 2;`

Pile mémoire
------------

### Push

Placement d'une donnée sur le dessus de la pile mémoire (stack).

```assembly
push eax            ; Met le contenu d'EAX sur la pile
```

### Pop

Retire la dernière valeur de la pile mémoire (stack) pour la stocker dans un registre.

```assembly
pop eax             ; Met l'élément du sommet de la pile dans EAX
```

### Leave

Ferme la fonction en cours en récupérant les variables enregistrées (réordonne la pile mémoire).

### Ret

Sauvegarde la position actuelle d'EIP afin de pouvoir revenir à l'adresse après désempilage.

```
leave               ; Ferme la fonction en cours
ret                 ; Retourne à l'adresse sauvegardée
```

### Notes sur POP

(Si nous voulons être plus exacts, l'élément au sommet de la pile reste là où il est, et le registre ESP qui pointe sur le sommet de la pile est mis à jour en pointant vers la valeur précédente sur la pile).

### Ret & Leave

```assembly
mov esp, ebp
pop eip
```

Ici, `Ret` et `Leave` sont donc des alias vers l'ensemble d'instructions associé.
