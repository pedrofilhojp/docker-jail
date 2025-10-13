# 🧪 Exercício Prático: Criação de Jail no Linux com `chroot` e `unshare`

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

***Copie binários essenciais para dentro da jail:***

```bash
sudo cp /usr/bin/ps ./jail/bin/
```
***Copie as bibliotecas necessárias (verifique com ldd):***

```bash
ldd /usr/bin/ps

sudo cp /lib/x86_64-linux-gnu/{libprocps.so.8,libc.so.6,libsystemd.so.0,liblzma.so.5,libzstd.so.1,liblz4.so.1,libcap.so.2,libgcrypt.so.20,libgpg-error.so.0} ./jail/lib/x86_64-linux-gnu/
```

***Monte o sistema de arquivos /proc na jail:***
```bash
sudo mount --bind /proc ./jail/proc
```


***Acesse o chroot, execute o htop***
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

# 🔐 Parte 4 – Criando rede no container

Ao iniciar nosso container com isolamento de rede, nele haverá apenas interface de loopback. para conectá-lo ao mundo externo pela rede, temos que realizar uma ligação interna no kernel de uma interface em nosso host (ou melhor, no namespace atual do nosso linux), com outra interface dentro do namespace do processo do nosso container.

Esse tipo de ligação é realizado com interface do tipo **veth**. Ela funciona como duas "interfaces de rede" com um "cabo de rede" interligando-as diretamente. Mas tudo isso é virtual dentro do kernel.

## Opção 01: Ligação direta do container com o host

![alt text](image.png)

1. Inicialize o jail com namespace de network, observe que tem apenas a interface de **loopback**

Obs. É necessário colocar dentro do `jail` o programa `ip`
```bash
sudo unshare -p -f -n --mount-proc=./jail/proc chroot jail
ip a
```

2. Em outro terminal, vamos cria a veth pair
```bash
ip link add veth-host type veth peer name veth-jail
```
Agora temos um par de interfaces:

`veth-host` <==> `veth-jail`

3. Coloca veth-jail no namespace do PID do jail

Busque o PID do processo do jail com `ps aux` coloque-o na variável PID


```bash
PID=139502
ip link set veth-jail netns $PID
```
No terminal do jail, executer `ip  a` e você verá a interface veth_jail lá dentro

4. Configura host
```bash
ip addr add 192.168.100.1/24 dev veth-host
ip link set veth-host up
```

5. Configura dentro do jail
```bash
nsenter -t $PID -n ip addr add 192.168.100.2/24 dev veth-jail
nsenter -t $PID -n ip link set veth-jail up
nsenter -t $PID -n ip link set lo up
```

## Outra opção 02: Ligando o container com uma brigde
Este modelo é semelhante a network do docker, para cada network criada o docker criar uma bridge, que tem nome de docker0, por padrão ela vem com ip 172.17.0.0/16, igual a figura abaixo.

![alt text](image-2.png){width=100px}

Para configuração, repita todos os passos passado e depois:
1. Crie uma bridge
```bash
sudo ip link add name br-test type bridge
sudo ip link set br-test up
```

2. Adicione a interface do lado do host (veth-ns1) à bridge
```bash
sudo ip link set veth-ns1 master br-test
```

Desta forma, a bridge atua como um switch, permitindo que outros container que estiverem também conectados na bridge se comuniquem.

Lembrando que para isso, retiraremos o IP da interface veth-ns1, e colocamos na interface da Bridge.

![alt text](image-1.png)