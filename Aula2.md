# Teórica 2 - Segurança nos Sistemas Operativos

## Sistema operativo

Um sistema operativo é um conjunto de programas que gerem os recursos de um sistema computacional e oferecem serviços aos utilizadores e aplicações.

Contém **dois modos**:
- *User-mode*: Executa as aplicações e serviços em modo normal da CPU, sem acesso a instruções privilegiadas.
- *Supervisor-mode*: Executa intruções em modo privilegiado da CPU (através do kernel), interagindo diretamente com o hardware.

O Kernel **tem como função**:
 - Virtualizar os recursos físicos;
 - Reforçar politicas de proteção contra acessos não autorizados/involuntários;

## Aneis de proteção

Os aneis de proteção são uma forma de implementar a separação entre o *user-mode* e o *supervisor-mode*.

Hoje em dias os processadores têm 4 aneis de proteção:
 - *Ring 0*: Kernel;
 - *Ring 1*: Drivers;
 - *Ring 2*: Sistemas de ficheiros;
 - *Ring 3*: Aplicações.

## Virtualização

Existem dois **tipos de virtualização**:

*Hosted*: O sistema operativo hospedeiro é executado diretamente no hardware e o sistema operativo convidado é executado sobre o sistema operativo hospedeiro.

| Guest OS           |
| ------------------ |
| Hypervisor Process |
| Host OS            |
| Hardware           |

*Bare-metal*: O sistema operativo convidado é executado diretamente no hardware. É mais seguro pois permite o isolamento entre os sistemas operativos (sem necessitar de *on-the-fly translation*).

| Guest OS   |
| ---------- |
| Hypervisor |
| Hardware   |

### Máquinas Virtuais

É um ambiente de execução que simula um sistema computacional.

### *Hypervisors*
É um programa que permite a execução de várias máquinas virtuais num único sistema físico.

## Modelo de segurança do sistema operativo

O modelo de segurança do sistema operativo é composto por o seguinte conjunto de entidades (objetos) geridos pelo Kernel:

- *Processos*: Entidade que executa um programa;
- *Identificadores do Utilizador (UID)*: Identifica o utilizador;
- *Ficheiros*: Entidade que armazena informação;
- *Canais de comunicação*: Entidade que permite a comunicação entre processos (sockets, pipes, etc.);

### User ID (UID)

O UID é um identificador numérico único que identifica um utilizador no sistema atribuído durante o processo de autenticação.

Existem dois tipos de UID:
- *Real UID*: Identifica o utilizador que executou o processo;
- *Effective UID*: Identifica o utilizador que tem permissões para executar o processo.

### Group ID (GID)

O GID é um identificador numérico único que identifica um grupo de utilizadores no sistema.

**Nota:** Um utilizador pode pertencer a vários grupos.

## Controlo de acesso

É composto por:
- *Sujeitos*: Entidades que executam operações sobre objetos;
- *Monitor de acesso*: Entidade que verifica se o sujeito tem permissão para aceder ao objeto;
- *Objetos*: Entidades que são acedidas pelos sujeitos.

Obs.: O Kernel é um monitor de acesso.

### ACL (Access Control List)

É uma lista que define as permissões de acesso a um objeto.

Pode ser:
- *Discretionary*: O dono do objeto pode definir as permissões de acesso;
- *Mandatory*: As permissões de acesso são definidas pelo sistema.

Em Unix, as permissões de acesso são definidas por:
- *Read* (R): Permite a leitura/listagem do ficheiro;
- *Write* (W): Permite a escrita/remoção no ficheiro;
- *Execute* (X): Permite a execução do ficheiro.

Em Unix, também pode ser feita proteção usando *special protection bits*:
- *Set-UID*: Permite que o programa seja executado com os privilégios do dono do ficheiro;
- *Set-GID*: Permite que o programa seja executado com os privilégios do grupo do ficheiro;
- *Sticky bit*: Permite que apenas o dono do ficheiro possa remover o ficheiro.

### Capabilities

É um conjunto de permissões que um sujeito tem sobre um objeto.

## Elevação de privilégios

### Mecanismo usando *Set-UID*/ *Set-GID*

Caso o *Set-UID*/ *Set-GID* esteja ativado, o programa será executado num processo com o mesmo ID do dono/grupo do ficheiro. Isto permite a utilizadores que não têm permissões de administrador executar programas com privilégios de administrador num ambiente controlado.

Existe dois tipos de UID:
- *Real UID*: Identifica o utilizador que executou o processo;
- *Effective UID*: Identifica o utilizador que tem permissões para executar o processo.

### Mecanismo usando *sudo*

O comando `sudo` permite que um utilizador execute um comando com privilégios de administrador, mesmo que o utilizador não seja *root*. Para o executar o utilizador pode ser sujeito a autenticação.

## Redução de privilégios

Permite reduzir a visibilidade de um determinado processo.

### *Chroot*

O comando `chroot` significa *change root* e permite que **um processo seja executado numa diretoria diferente da diretoria raiz**, limitando o acesso/visibilidade a outras partes do sistema.

Nota: Embora este mecanismo permita uma forma de isolamento, **não é seguro** pois existem formas de *bypass*.

### *Chroot Jail*

É um ambiente de execução limitado que permite a execução de um processo numa diretoria diferente da diretoria raiz.

## Login no Linux

Não é uma operação do Kernel.

O processo de login é composto por:
 - pcb;
 - UID;
 - GID;
 - *Home directory*;

### Ficheiros de configuração

O caminho `/etc/passwd` contém a informação dos **utilizadores do sistema**, como:
- Nome de utilizador;
- UID;
- GID;
- *Home directory*;
- *Shell*.

O caminho `/etc/group` contém a informação dos **grupos do sistema**, como:
- Nome do grupo;
- GID;
- Lista de utilizadores.

O caminho `/etc/shadow` contém a informação das **passwords dos utilizadores do sistema** (em formato *hash*).

### PCB (Process Control Block)

É uma estrutura de dados que contém informação sobre o processo.


# Prática 2 - Segurança nos Sistemas Operativos

To check the UID and GID of the files in the current directory, use the command `ls` with the option `-n`.
```bash
ls -n
```

Create a C program that prints the effective and real UID of the current process, as well as its effective and
real GIDs. After its compilation, run the program and check the result.

```c
#include <stdio.h>
#include <unistd.h> // Unix Standard Library

int main() {
    printf("Real UID: %d\n", getuid());
    printf("Effective UID: %d\n", geteuid());
    printf("Real GID: %d\n", getgid());
    printf("Effective GID: %d\n", getegid());
    return 0;
}
```

Compile the program with the command gcc. Run the program and check the results.

```bash
gcc -o uid uid.c
./uid
```

Note: Hereafter, some of the commands have to be executed by a super-user. Use a preceding sudo command in
those cases.

Change the ownership of the executable just created (UID of the owner) with the command chown; change also
the owner group GID of the executable (with the chgrp command). Use the UID of any other user and the
GID of any other group. Run the program again and check the results.

```bash
sudo chown 1001 uid
sudo chgrp 1001 uid
./uid
```

Activate the Set-UID flag of the executable with the command chmod. Run the program again and check the
results.

```bash
sudo chmod u+s uid
./uid
```

Activate the Set-GID flag of the executable with the command chmod. Run the program again and check the
results.

```bash
sudo chmod g+s uid
./uid
```

Change again the ownership of the executable, or its owner group GID. Run the program again and check
the results.

```bash
sudo chown 1000 uid
sudo chgrp 1000 uid
./uid
```

This is the goal of the chroot system call, and Linux command; check how it works with the commands:

```bash
man 8 chroot # for linux commands
man 2 chroot # for system calls
```

For the system call:
 - it receives a `const char *path` that contains the path directory in the system
 - on success the zero is returned. On error, -1 is returned 

For the linux commands:
 - `--groups=G_LIST`specify sumplementary groups
 - `--userspec=USER` specify user to use
 - `--skip-chdir` do not change working directory to `/`

Is this experiment we will create a confined environment where only 4 commands exist: bash, ls, mkdir, and vi. 

To find the path of the commands, use the command `which`:
```bash
which bash
which ls
which mkdir
which vi
```

Create a directory, and copy the executables of the commands to the directory:
```bash
cd /new_root_aes
mkdir lib bin
cp /bin/bash /new_root_aes/bin
cp /bin/ls /new_root_aes/bin
cp /bin/mkdir /new_root_aes/bin
cp /bin/vi /new_root_aes/bin
```

Copy the shared libraries of the commands to the directory:
```bash
ldd /bin/bash
ldd /bin/ls
ldd /bin/mkdir
ldd /bin/vi
```

Copy the shared libraries to the directory:
```bash
cp /lib/x86_64-linux-gnu/libtinfo.so.5 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libdl.so.2 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libc.so.6 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libtinfo.so.5 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libdl.so.2 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libc.so.6 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libtinfo.so.5 /new_root_aes/lib
cp /lib/x86_64-linux-gnu/libdl.so.2 /new_root_aes/lib
```

Change the root directory of the process with the command chroot. Check the result with the command ls.

```bash
sudo chroot /new_root_aes /bin/bash
ls
```

