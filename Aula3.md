# Aula 3 - Virtualização

## Modos de operação

Os processadores x86 têm vários modos de operação, presentes nos anéis de proteção, sendo estes:
- *Real-address mode*: O processador opera em modo real quando é iniciado. Neste modo, o processador pode aceder a 1MB de memória e executar instruções de 16 bits.
- *Protected mode* (anél 1 até ao 3): O processador opera em modo protegido quando o sistema operativo é carregado. 
- *hypervisor mode* (anél -1): O processador opera em modo hipervisor quando o hipervisor é carregado.
- *system management mode* (anél -2): O processador opera em modo de gestão de sistema quando o sistema operativo é carregado. 
- *management engine mode* (anél -3): O processador opera em modo de gestão de motor quando o sistema operativo é carregado. 

## Enclaves

São uma forma de executar código de forma segura. O código é apenas decifrado dentro do processador e é mesmo seguro até por leituras diretamente na memória RAM.

### Intel SGX (*Software Guard Extensions*)

A Intel SGX é uma tecnologia que permite a criação de enclaves seguros.

Existem duas formas de chamar funções de um enclave:
- *ECALL*: Chama uma função do enclave **fora** do enclave.
- *OCALL*: Chama uma função do enclave **dentro** do enclave.

Exemplo de código de um enclave:

```c
#include "enclave_t.h"

void ecall_hello_world() {
    ocall_print_string("Hello, World!\n");
}

int main() {
    ecall_hello_world();
    return 0;
}
```