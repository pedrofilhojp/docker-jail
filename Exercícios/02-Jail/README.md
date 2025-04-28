# 🧪 Exercício Prático: Criação de Jail no Linux com `chroot`, `unshare` e `seccomp`

## 📘 Objetivo

Neste exercício, o aluno irá criar um ambiente isolado (**jail**) utilizando o comando `chroot`, complementando com técnicas avançadas de isolamento usando `unshare` (namespaces) e `seccomp` (filtro de chamadas de sistema).  
O propósito é entender como ambientes seguros e isolados podem ser criados sem uso de virtualização completa.

---

## 🧱 Parte 1 – Criando o Ambiente da Jail com `chroot`

### 1.1 Crie a estrutura de diretórios da jail:

```bash
sudo mkdir -p /jail/{bin,lib64,lib/x86_64-linux-gnu,dev,etc,home,usr,proc}
```


### 1.2 Copie binários essenciais para dentro da jail:

```bash
cd /
sudo cp /bin/bash ./jail/bin/
sudo cp /bin/ls ./jail/bin/
sudo cp /bin/cat ./jail/bin/
```

### 1.3 Copie as bibliotecas necessárias (verifique com ldd):

```bash
ldd /bin/bash
ldd /bin/ls
ldd /bin/cat
```

### Exemplo de cópia:
```bash
sudo cp /lib/x86_64-linux-gnu/{libtinfo.so.6,libc.so.6}  ./jail/lib/x86_64-linux-gnu/
sudo cp /lib64/ld-linux-x86-64.so.2 ./jail/lib64/
```


### 1.4 Crie dispositivos básicos:

```bash
sudo mknod -m 666 ./jail/dev/null c 1 3
sudo mknod -m 666 ./jail/dev/zero c 1 5
```
### 1.5 Copie arquivos de configuração mínimos:

```bash
sudo cp /etc/passwd ./jail/etc/
sudo cp /etc/group ./jail/etc/
```


# 🚪 Parte 2 – Acessando o Ambiente com chroot
```bash
sudo chroot ./jail
ou
sudo chroot ./jail /bin/bash
```
Dentro da jail, execute comandos como ls, whoami e cat /etc/passwd para testar o ambiente.

Para sair da jail, digite exit, e depois desmonte o /proc:
```bash
exit
```

# 🔐 Parte 3 – NameSpaces Linux - Isolamento Avançado com unshare

## 3.1. Mapeamento do programa ps

Antes de iniciar, vamos copiar mais 1 programa (ps) para dentro do chroot e mapear o /proc 

### 3.1.1 Copie binários essenciais para dentro da jail:

```bash
sudo cp /usr/bin/ps ./jail/bin/
```
### 3.1.2 Copie as bibliotecas necessárias (verifique com ldd):

```bash
ldd /usr/bin/ps

cp /lib/x86_64-linux-gnu/{libprocps.so.8,libc.so.6,libsystemd.so.0,liblzma.so.5,libzstd.so.1,liblz4.so.1,libcap.so.2,libgcrypt.so.20,libgpg-error.so.0} ./jail/lib/x86_64-linux-gnu/
```

### 3.1.3 Monte o sistema de arquivos /proc na jail:
```bash
sudo mount --bind /proc ./jail/proc
```


### 3.1.4 Acesse o chroot, execute o htop
```bash
sudo chroot ./jail /bin/bash

ps aux
```
Observe que todos os processos estão sendo exibidos. Não há isolamento dos processo do sistema com o chroot


## 3.2. O que é unshare?
O comando unshare permite que você execute processos em namespaces isolados, criando ambientes com visibilidade limitada de processos, rede, montagem, etc. É uma tecnologia base para containers.


### ✅ Execute o seguinte comando para isolar totalmente a jail:
```bash
sudo unshare -p -f --mount-proc=./jail/proc chroot jail
ou
sudo unshare --mount --mount-proc=./jail/proc --uts --ipc --net --pid --fork --user --map-root-user chroot jail /bin/bash
```
**Explicação das opções:**

- **--mount**: isola pontos de montagem.
- **--uts**: isola nome do host (hostname).
- **--ipc**: isola comunicação entre processos.
- **--net**: isola a rede.
- **--pid**: isola processos.
- **--user**: cria novo namespace de usuários.
- **--map-root-user**: permite agir como root dentro da jail.
- **--fork**: força o processo a rodar isolado.

## 🧠 Teste: 
Dentro da jail, use ps aux e verifique que os processos do host não aparecem.

# 🛡️ Parte 4 – Restrição de Syscalls com seccomp
##📘 O que é seccomp?
seccomp (Secure Computing Mode) é um recurso do kernel Linux que permite restringir as chamadas de sistema (syscalls) que um processo pode executar. Ele é muito utilizado para aumentar a segurança de containers e aplicações sandbox.

## 4.1 Instale a biblioteca libseccomp e ferramentas:
```bash
sudo apt install libseccomp-dev seccomp-tools
```
## 4.2 Crie o código seccomp_jail.c:
```C
#define _GNU_SOURCE
#include <seccomp.h>
#include <stdio.h>
#include <unistd.h>

int main() {
    scmp_filter_ctx ctx;

    ctx = seccomp_init(SCMP_ACT_KILL); // Bloqueia tudo por padrão

    // Permitir algumas chamadas seguras
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(read), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(write), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(exit), 0);
    seccomp_rule_add(ctx, SCMP_ACT_ALLOW, SCMP_SYS(fstat), 0);

    seccomp_load(ctx);

    printf("Seccomp ativado! Chamadas restritas.\n");

    execl("/bin/bash", "bash", NULL);

    return 0;
}
```
## 4.3 Compile e copie para a jail:
```bash
gcc seccomp_jail.c -o seccomp_jail -lseccomp
sudo cp seccomp_jail ./bin/
```
## 4.4 Execute com chroot:
```bash
sudo chroot ./jail /bin/seccomp_jail
```
## 🧠 Teste: 
Tente rodar comandos simples. A jail permitirá somente os que utilizam syscalls permitidas. Qualquer tentativa de uso de chamadas não permitidas causará o encerramento do processo.