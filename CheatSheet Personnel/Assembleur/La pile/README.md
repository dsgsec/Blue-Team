La pile
-------

La mémoire d'un processus peut être segmentée en plusieurs parties, notamment :

-   **Heap (le tas)**
-   **BSS**
-   **Data**
-   **Mapping du noyau**
-   **Librairies partagées / mappées**
-   **Text**

Ces segments sont disposés dans la mémoire selon des adresses spécifiques, généralement allant de `0x00000000` à `0xffffffff`.

### Segmentation de la mémoire d'un processus

#### Heap (le tas)

-   Manipulable par le programmeur.
-   Contient les zones mémoires allouées dynamiquement (utilisation de `malloc()` ou `calloc()`).
-   Sa taille est variable selon les besoins du programme.
-   Le tas augmente vers le bas (vers les adresses mémoire hautes).

#### La Pile (Stack)

-   Taille variable.
-   Sa taille augmente lorsque les adresses mémoires diminuent.
-   Contient les variables locales des fonctions du programme.
-   Contient le cadre de pile (stack frame) de ces fonctions.

#### La Stack Frame (d'une fonction)

-   Zone mémoire située dans la pile.
-   Contient les informations nécessaires à l'appel de cette fonction.
-   Last In, First Out (LIFO) : Il est possible d'ajouter ou retirer une valeur sur le sommet (le sommet étant orienté vers le bas).

### Exemple de l'usage de la pile

Si l'on empile des feuilles les unes sur les autres, il faudra enlever la dernière, puis l'avant dernière, puis l'avant avant dernière (etc.) pour récupérer la première feuille posée. La stack empile ses éléments vers le bas ; le haut de la stack est l'adresse la plus basse de la stack. Plus on empile des valeurs dans la stack, plus les adresses diminuent.

#### La pile est construite comme file d'attente LIFO

-   Dernier entré, Premier sorti.
-   Sa hiérarchie d'adresses est conçue pour passer d'une adresse supérieure à une adresse inférieure (cela signifie que la pile grandit vers le bas).
-   Chaque donnée envoyée est positionnée sur le dessus de la pile (instruction Push).
-   Vous ne pouvez retirer que ce qui se trouve sur le dessus de cette pile (instruction Pop).
-   La pile sert à stocker des informations pour une utilisation ultérieure par le programme.

#### Exemple de la pile

```
Stackframe 0
Stackframe 1
Stackframe 2
Stackframe 3
Stackframe 0
Stackframe 1
Stackframe 2
Stackframe 3
Stackframe 4
Stackframe 4
```

-   **Push** : Ajouter une valeur.
-   **Pop** : Retirer une valeur.

La pile empile ses éléments vers le bas ; le haut de la pile est l'adresse la plus basse de la pile. L'ajout ou le retrait d'une valeur se fait uniquement sur le sommet de la pile.
