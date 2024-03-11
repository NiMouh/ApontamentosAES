# Aula 3 - Virtualização

## Modos de operação

Os processadores x86 têm vários modos de operação, presentes nos anéis de proteção, sendo estes:
- *Real-address mode*: O processador opera em modo real quando é iniciado. Neste modo, o processador pode aceder a 1MB de memória e executar instruções de 16 bits.
- *Protected mode* (anél 1 até ao 3): O processador opera em modo protegido quando o sistema operativo é carregado. 
- *hypervisor mode* (anél -1): O processador opera em modo hipervisor quando o hipervisor é carregado.
- *system management mode* (anél -2): O processador opera em modo de gestão de sistema quando o sistema operativo é carregado. 
- *management engine mode* (anél -3): O processador opera em modo de gestão de motor quando o sistema operativo é carregado. 

## TEE (*Trusted Execution Environment*)

É um ambiente de execução que permite a execução de código de forma segura.

## Enclaves

São uma forma de executar código de forma segura. O código é apenas decifrado dentro do processador e é mesmo seguro até por leituras diretamente na memória RAM.

### Intel SGX (*Software Guard eXtensions*)

A Intel SGX é uma tecnologia que permite a criação de enclaves. Tudo **fora do processador** é considerado **inseguro**. 

Em particular, os seguintes componentes são considerados **inseguros**:
- BIOS (*Basic Input/Output System*);
- SMM (*System Management Mode*);
- ME (*Management Engine*, anél -3);
- Sistema operativo (*kernel*, anél 0);

Existem dois fluxos de execução:
- *ECALL*: Transferência de código/memória para **dentro** do enclave;
- *OCALL*: Transferência de código/memória para **fora** do enclave.

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

Nota: O código enclave corre no anél 3 (*least privileged mode*).

### Memória Virtual

É técnica utilizada pelos sistemas operativos para fornecer aos programas a ilusão de que têm acesso a uma grande quantidade de memória principal (RAM) contígua, quando na verdade essa memória pode estar fragmentada e pode ser armazenada tanto na RAM quanto em dispositivos de armazenamento secundário, como discos rígidos.

É **útil para a virtualização**, pois:
1. **Permite o isolamento/gestão de recursos**: ajuda a garantir que um sistema operativo não interfere com outro;
2. **Evita conflitos de endereços**: cada sistema operativo pode ter o seu próprio espaço de endereçamento (através de *page tables*);
3. **Alocação dinâmica de memória**: permite que mesmo com limitações de memória física, as máquinas virtuais possam ter acesso a mais memória do que a memória física disponível.