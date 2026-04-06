# Exercício Prático: Criação de Jail no Linux com `chroot` e `unshare`

## Objetivo

Neste exercício, o aluno irá criar um ambiente isolado (**jail**) utilizando o comando `chroot`, complementando com técnicas avançadas de isolamento usando `unshare` (namespaces) e `seccomp` (filtro de chamadas de sistema).
O propósito é entender como ambientes seguros e isolados podem ser criados sem uso de virtualização completa.

---

## Parte 1 – Criando o Ambiente da Jail com `chroot`

### 1.1 Crie a estrutura de diretórios da jail:

```bash
mkdir -p ./jail/{bin,lib64,lib/x86_64-linux-gnu,dev,etc,home,usr,proc}
```

### 1.2 Copie binários essenciais para nosso estudo e suas libs dentro da jail:

```bash
which bash

cp /bin/bash ./jail/bin/
cp /bin/ls ./jail/bin/
cp /bin/cat ./jail/bin/
cp /usr/bin/ps ./jail/bin/
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
cp /lib/x86_64-linux-gnu/{libtinfo.so.6,libc.so.6}  ./jail/lib/x86_64-linux-gnu/
cp /lib64/ld-linux-x86-64.so.2 ./jail/lib64/
```

==> Faça o mesmo para o **/bin/ls**, **/bin/cat**, e **/usr/bin/ps**

### 1.3 Copie arquivos de configuração mínimos:

```bash
cp /etc/passwd ./jail/etc/
cp /etc/group ./jail/etc/
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

> #### Qual o Problema esta solução? 👀️
>
> * Precisamos elevar a root no sistema para executar chroot
> * Mesmo dentro do chroot, não tivemos isolamento de processos

## Parte 2 – NameSpaces Linux - Isolamento Avançado com unshare

### 2.1 O que é unshare?

O comando unshare permite que você execute processos em namespaces isolados, criando ambientes com visibilidade limitada de processos, rede, montagem, etc. É uma tecnologia base para containers.

Com usuário simples (sem root), **Execute**:

```bash
unshare \
  --mount \
  --uts \
  --ipc \
  --pid \
  --fork \
  --user \
  --net \
  --map-root-user \
  bash -c "
    mount --make-rprivate / &&
    mount -t proc proc /proc &&
    exec bash
  "
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

> **Atenção**:
>
> - O "**--map-root-user**", esta opção permite que você fique com root dentro do namespace, **mas este não é o root do seu sistema**. É assim que o Docker faz.
> - Devido ao argumento **--pid** e **--fork**, juntamente com o comando "**mount -t proc proc /proc**" permitiu que o sistema mapeasse uma nova hierarquia de processo dentro do namespace.

Vamos alguns testes utilizando como referencia o namespace PID:

Primeiro, observe que no shell, agora termina com "#", indicando que você de fato está root.

Para provar que é root apenas dentro do namespace, execute "**cat /etc/shadow**", observe que não tem acesso.

Agora, execute "**ps aux**", e você vê poucos processos, sendo o PID 1 do comando /bash

```bash
ps aux

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  1.0  0.0  21484  5888 pts/0    S    11:31   0:00 bash
root          10  0.0  0.0  22604  3640 pts/0    R+   11:31   0:00 ps aux
```

Ainda é possível visualizar todos os processos do sistema, basta desmontar o /proc

```bash
umount /proc
ps aux 

USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
nobody         1  0.0  0.0 166500 12068 ?        SNs  10:20   0:01 /sbin/init splash
nobody         2  0.0  0.0      0     0 ?        S    10:20   0:00 [kthreadd]
nobody         3  0.0  0.0      0     0 ?        S    10:20   0:00 [pool_workqueue_release]
nobody         4  0.0  0.0      0     0 ?        I<   10:20   0:00 [kworker/R-rcu_gp]
nobody         5  0.0  0.0      0     0 ?        I<   10:20   0:00 [kworker/R-sync_wq]
nobody         6  0.0  0.0      0     0 ?        I<   10:20   0:00 [kworker/R-kvfree_rcu_reclaim]
...
...

```

Para sair do **namespace** basta executar o comando:

```bash
exit
```

> #### Vamos algumas análises 👀️
>
> * O chroot sozinho não tras isolamento de processo;
> * O namespace sozinho não tras isolamento de "diretório";
> * O namespace mesmo isolando processo, basta desmontar o /proc e lhe permite ver os demais processos do sistema

## Parte 3 - Juntando chroot + namespace

No momento, temos toda uma estrutura de sistema criado na Parte 1 dentro do diretório ./jail. Vamos em 3 passos:

* 1º Criar namespace
* 2º Montagem do **/proc** em **./jail/proc**
* 3º Criando o chroot no **./jail**

```bash
unshare   --mount   --uts   --ipc   --pid   --fork  --net --user   --map-root-user
mount -t proc proc ./jail/proc
chroot ./jail
```

Agora teste o comando ps, tente acessar alguns recursos dentro do nosso "container"... Observe que já estamos contruindo um ambiente mais confinado.

<!--
<details>
<summary>Clique para ver o segredo</summary>
Texto oculto que aparece ao clicar.
</details>

sudo unshare -p -f --mount-proc=./jail/proc chroot ./jail
ou
sudo unshare --mount --mount-proc=./jail/proc --uts --ipc --net --pid --fork --user --map-root-user chroot ./jail /bin/bash
 -->
