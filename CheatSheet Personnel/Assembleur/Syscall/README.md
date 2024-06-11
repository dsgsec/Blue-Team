Syscalls
--------

Les syscalls sont des fonctionnalités système fournies par le kernel et les librairies du système. Ils sont invoqués par des instructions spécifiques telles que `call` (Windows) ou `int 0x80` (Linux). Ces appels système permettent, entre autres, la gestion des processus, de la mémoire, des fichiers, des E/S et de la manipulation réseau.

### Appels systèmes courants

#### Catégorie Windows / Linux

| Informations et maintenance | Windows | Linux |
| --- | --- | --- |
| GetCurrentProcessID() | getpid() |  |
| SetTimer() | alarm() |  |
| Sleep() | sleep() |  |

| Contrôle des processus | Windows | Linux |
| --- | --- | --- |
| CreateProcess() | fork() |  |
| ExitProcess() | exit() |  |
| WaitForSingleObject() | wait() |  |

| Manipulation de fichiers | Windows | Linux |
| --- | --- | --- |
| CreateFile() | open() |  |
| ReadFile() | read() |  |
| WriteFile() | write() |  |
| CloseHandle() | close() |  |

| I/O | Windows | Linux |
| --- | --- | --- |
| SetConsoleMode() | ioctl() |  |
| ReadConsole() | read() |  |
| WriteConsole() | write() |  |

### Documentation syscall

Pour obtenir la documentation d'un appel système ou d'une fonction C sous Linux, vous pouvez consulter des ressources telles que :

-   [Syscalls.w3challs](https://syscalls.w3challs.com/)
-   Utilisez la commande `man` suivie du nom de l'appel système ou de la fonction C (par exemple, `man exit` pour obtenir le manuel du syscall exit).

Pour Windows, des ressources similaires sont disponibles :

-   [Syscalls.w3challs](https://syscalls.w3challs.com/)
-   [Documentation Microsoft Windows API](https://learn.microsoft.com/fr-fr/windows/win32/api/)
-   [Fonctions de registre Windows](https://learn.microsoft.com/fr-fr/windows/win32/sysinfo/registry-functions)

### Exemple : exit() / ExitProcess()

Un exemple d'utilisation de syscall pour quitter un programme avec un code de sortie spécifique :

#### Sous Linux (x86)

```assembly
section .text
global _start

_start:
    mov eax, 1      ; syscall exit()
    mov ebx, 10     ; code de sortie du programme
    int 0x80        ; appel système
```

#### Sous Windows

```assembly
section .text
global _start
extern _ExitProcess@4

_start:
    mov ecx, 0      ; code de sortie
    push ecx        ; pousse le code de sortie dans la pile
    call _ExitProcess@4
```
