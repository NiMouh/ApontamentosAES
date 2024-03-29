# Teórica 1 - Introdução a Ambientes de Execução Seguros

## Trusted Computing Base (TCB)

O TCB é composto por hardware, software e procedimentos que são necessários para garantir a segurança do sistema.

Incluí:
- Mecanismos de segurança do CPU:
  - Anéis de proteção
  - Virtualização
  - Etc.;
- Modelo de segurança do sistema operativo:
  - Modelo computacional;
  - Firmware;
  - Permissões de acesso e privilégios;

## TEE (Trusted Execution Environment)

O TEE é um ambiente de execução seguro que permite a execução de código de forma segura e isolada.

ARM TrustZone é um exemplo de TEE.

## Secure bootstrapping

O Secure bootstrapping é um processo que permite garantir a integridade do sistema durante o arranque.

Protege contra ameaças como:
- Bootkits;
- Rootkits;
- Malware;
- Etc.

Existem duas fases no processo de arranque seguro:
- TPM Attestation: Verifica a integridade do sistema comparando os valores de hash dos componentes do arranque com os valores armazenados no TPM.
- UEFI Secure Boot: Ele envolve a verificação da assinatura digital dos componentes do arranque, como o bootloader.

## Intel SGX (Software Guard Extensions)

O Intel SGX é uma extensão de segurança que permite a criação de regiões de memória protegidas (enclaves).

## Sandbox

O Sandbox é um mecanismo de segurança que permite a execução de código de forma isolada. É utilizado para executar código não confiável.