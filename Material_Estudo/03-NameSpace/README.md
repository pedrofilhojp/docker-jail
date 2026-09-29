# Exercício Prático: Criação de Jail no Linux com `chroot` e `unshare`

## Objetivo

Neste exercício, utilizar o isolamento com `Jail`  realizado na sesão anterior, complementando com técnicas avançadas de isolamento usando `unshare` (namespaces) e `seccomp` (filtro de chamadas de sistema).
O propósito é entender como ambientes seguros e isolados podem ser criados sem uso de virtualização completa.

---

# Parte 1 – NameSpaces Linux - Isolamento Avançado com unshare

## 1.1 O que é unshare?

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

### 1.1.1 - Vamos realizar alguns testes utilizando como referencia o namespace PID:

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

# Parte 2 - Juntando chroot + namespace

No momento, temos toda uma estrutura de sistema criado na sesão anterior dentro do diretório ./jail. Vamos em 3 passos:

* 1º Criar namespace
* 2º Montagem do **/proc** em **./jail/proc**
* 3º Criando o chroot no **./jail**

```bash
unshare --mount \
        --uts \
        --ipc \
        --pid \
        --fork \
        --net \
        --user \
        --map-root-user

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

# Parte 3. Explorando mais o unshare (Opcional, mas importante)

## 3.1 – Descobrindo namespaces de um processo
Todo processo no Linux possui namespaces associados.
```bash
ps aux | grep bash
```

Pegue o PID de um processo (ex: 1234) e rode:

```bash
ls -l /proc/1234/ns
mnt:[4026531840]
pid:[4026531836]
net:[4026532000]
uts:[4026531838]
user:[4026531837]
ipc:[4026531839]
```
Cada número representa um namespace.

### 3.7 – Inspecionando namespaces com lsns

Use:
```bash
lsns

        NS TYPE   NPROCS    PID USER  COMMAND
4026531832 mnt       136   3368 pedro /lib/systemd/systemd --user
4026531833 net       109   3368 pedro /lib/systemd/systemd --user
4026531834 time      136   3368 pedro /lib/systemd/systemd --user
4026531835 cgroup    136   3368 pedro /lib/systemd/systemd --user
4026531836 pid       109   3368 pedro /lib/systemd/systemd --user
4026531837 user      109   3368 pedro /lib/systemd/systemd --user
4026531838 uts       136   3368 pedro /lib/systemd/systemd --user
4026531839 ipc       136   3368 pedro /lib/systemd/systemd --user
4026532581 pid         1 141752 pedro /opt/google/chrome/chrome --type=utility -
...
```
Mostra:
- todos os namespaces do sistema
- quais processos pertencem a eles

### 3.2 – Entrando em um namespace com **nsenter**
Em um terminal, crie o namespace com nosso container
```bash
unshare   --mount \
          --uts \
          --ipc \
          --pid \
          --fork \
          --net \
          --user \
          --map-root-user

mount -t proc proc ./jail/proc

chroot ./jail
```
Em outro terminal, descubra o PID desse bash:
```bash
ps aux | grep unshare
```
Entre no namespace:
```bash
sudo nsenter -t <PID> -a /bin/bash
```
Agora você está dentro do mesmo ambiente isolado

> Isso equivale ao "docker exec -it <ID do Container> /bin/bash"

### 3.3 – Entrando em namespaces específicos

Você pode entrar em namespaces isoladamente:
```bash
sudo nsenter -t <PID> --net bash
sudo nsenter -t <PID> --pid bash
sudo nsenter -t <PID> --mount bash
```
Isso permite:
- Entrar só na rede
- Só no filesystem
- Só nos processos

