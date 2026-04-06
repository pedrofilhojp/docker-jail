# Exercício Prático: Criação de Jail no Linux com `chroot` e `unshare`

## Objetivo

Neste exercício, o aluno irá criar um ambiente isolado (**jail**) utilizando o comando `chroot`, complementando com técnicas avançadas de isolamento usando `unshare` (namespaces) e `seccomp` (filtro de chamadas de sistema).
O propósito é entender como ambientes seguros e isolados podem ser criados sem uso de virtualização completa.

---

## Parte 1 – Criando o Ambiente da Jail com `chroot`

### 1.1 Crie a estrutura de diretórios da jail:

```bash
sudo mkdir -p ./jail/{bin,lib64,lib/x86_64-linux-gnu,dev,etc,home,usr,proc}
```

### 1.2 Copie binários essenciais para nosso estudo e suas libs dentro da jail:

```bash
sudo cp /bin/bash ./jail/bin/
sudo cp /bin/ls ./jail/bin/
sudo cp /bin/cat ./jail/bin/
sudo cp /usr/bin/ps ./jail/bin/
```

Exemplo com o **bash**, copie as bibliotecas necessárias (verifique com ldd):

```bash
ldd /bin/bash

        linux-vdso.so.1 (0x0000754d07d8b000)
	libtinfo.so.6 => /lib/x86_64-linux-gnu/libtinfo.so.6 (0x0000754d07bd5000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x0000754d07800000)
	/lib64/ld-linux-x86-64.so.2 (0x0000754d07d8d000)
```

Exemplo de cópia:

```bash
sudo cp /lib/x86_64-linux-gnu/{libtinfo.so.6,libc.so.6}  ./jail/lib/x86_64-linux-gnu/
sudo cp /lib64/ld-linux-x86-64.so.2 ./jail/lib64/
```

==> Faça o mesmo para o **/bin/ls**, **/bin/cat**, e **/usr/bin/ps**

### 1.3 Copie arquivos de configuração mínimos:

```bash
sudo cp /etc/passwd ./jail/etc/
sudo cp /etc/group ./jail/etc/
```

### 1.4 – Acessando o Ambiente com chroot

```bash
sudo chroot ./jail
ou
sudo chroot ./jail /bin/bash
```

Dentro da jail, execute comandos como "ls" e "cat /etc/passwd" para testar o ambiente.

Tente também executar o "cd ../../../" e perceba que não sai de dentro do "jail"

#### 1.4.1 **Comando "ps aux"**

Observe que todos os processos estão sendo exibidos. Não há isolamento dos processo do sistema com o chroot

Neste momento, apenas alteramos o PATH ROOT do processo. Ao executar um chroot, acontece isso internamente:

* Cada processo no Linux tem uma estrutura chamada`fs_struct`
* Essa estrutura contém:
  * `root` → diretório raiz (`/`)
  * `pwd` → diretório atual

Quando você usa `chroot`:

* O campo`root` passa a apontar para o diretório que você definiu
* O`/` daquele processo agora é esse novo caminho

```bash
/jail/
  ├── bin/
  ├── lib/
  ├── etc/
  ├── ...
```

dsfsd

Então, dentro do shell:

* `/bin` → na verdade é`/jail/bin`
* `/etc` →`/jail/etc

### 1.5 Sair da jail

digite "**exit**"

```bash
exit
```

> #### Qual o Problema esta solução?
>
> * Precisamos elevar a root no sistema para executar chroot
> * Mesmo dentro do chroot, não tivemos isolamento de processos

## Parte 2 – NameSpaces Linux - Isolamento Avançado com unshare


### 2.1 O que é unshare?

O comando unshare permite que você execute processos em namespaces isolados, criando ambientes com visibilidade limitada de processos, rede, montagem, etc. É uma tecnologia base para containers.

Com usuário simples (sem root), **Execute**:

```bash
sudo unshare \
  --mount \
  --uts \
  --ipc \
  --pid \
  --fork \
  --user \
  --map-root-user \
  bash -c "
    mount --make-rprivate / &&
    mount -t proc proc /proc &&
    exec bash
  "
```

sdfsdfsd



```bash
sudo unshare -p -f --mount-proc=./jail/proc chroot ./jail
ou
sudo unshare --mount --mount-proc=./jail/proc --uts --ipc --net --pid --fork --user --map-root-user chroot ./jail /bin/bash
```

**Explicação das opções:**

- **-m, --mount**: isola pontos de montagem.
- **-u, --uts**: isola nome do host (hostname).
- **-i, --ipc**: isola comunicação entre processos.
- **-n, --net**: isola a rede.
- **-p, --pid**: isola processos.
- **-U, --user**: cria novo namespace de usuários.
- **-r, --map-root-user**: permite agir como root dentro da jail.
- **-f, --fork**: força o processo a rodar isolado.

> Teste:
>
> Dentro da jail, use **ps aux** e verifique que os processos do host não aparecem.

## Parte 4. Explorando mais o unshare
