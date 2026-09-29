# Exercício Prático: Criação de Jail no Linux com `chroot` e `unshare`

## Objetivo

Neste exercício, o aluno irá criar um ambiente isolado (**jail**) utilizando o comando `chroot`. Em sesões posteriores, usaremos com técnicas avançadas de isolamento usando `unshare` (namespaces) e `seccomp` (filtro de chamadas de sistema).
O propósito é entender como ambientes seguros e isolados podem ser criados sem uso de virtualização completa.

---

# Parte 1 – Preparando o Ambiente para criar Jail com `chroot`

## 1.1 Processo manual 

### 1.1.1 Crie a estrutura de diretórios da jail:

```bash
mkdir -p ./jail/{bin,lib64,lib/x86_64-linux-gnu,dev,etc,home,usr,proc}
```

### 1.1.2 Copie binários essenciais para nosso estudo e suas libs dentro da jail:

```bash
which bash

cp /bin/bash ./jail/bin/
cp /bin/ls ./jail/bin/
cp /bin/cat ./jail/bin/
cp /usr/bin/ps ./jail/bin/
```
Ao execuar algum comando no linux, há bibliotecas que são utilizadas pelos processos, estas bibliotecas são armazenadas normalmente nos diretório /lib ou /lib64. A seguir, um exemplo com o binário **bash**, identifique as bibliotecas que ele usa com ldd e copie-as para o diretório equivalente dentro do `jail`:

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

### 1.1.3 Copie arquivos de configuração mínimos:

```bash
cp /etc/passwd ./jail/etc/
cp /etc/group ./jail/etc/
```

### 1.1.4 – Acessando o Ambiente com chroot

```bash
sudo chroot ./jail
# ou
sudo chroot ./jail /bin/bash
```

Dentro da jail, execute comandos como "ls" e "cat /etc/passwd" para testar o ambiente.

Tente também executar o "cd ../../../" e perceba que não sai de dentro do "jail"

## 1.2 Utilizando o bootstrap do Debian
Se você estiver utilizando uma distribuição baseada em Debian ou Ubuntu, instale o pacote necessário executando:

```bash
sudo apt update && sudo apt install -y debootstrap
```

Use o comando debootstrap com a flag --variant=minbase. Essa variação garante uma instalação minimalista ideal para contêineres, contendo apenas o apt e pacotes vitais.

```bash
# Cria o diretório para receber a estrutura
mkdir debian-rootfs

# Baixa a estrutura básica do Debian FHS
sudo debootstrap --variant=minbase bookworm ./debian-rootfs http://deb.debian.org/debian/
```

Para deixar a imagem do contêiner o menor possível, você pode remover arquivos temporários de download armazenados no cache da pasta criada

```bash
sudo rm -rf ./debian-rootfs/var/cache/apt/archives/*.deb
sudo rm -rf ./debian-rootfs/var/lib/apt/lists/*
```

# Parte 2 - **Comando "ps aux"**

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


Então, dentro do shell:

* `/bin` → na verdade é`/jail/bin`
* `/etc` →`/jail/etc

### 1.6 Sair da jail

digite "**exit**"

```bash
exit
```

> #### Qual o Problema esta solução? 👀️
>
> * Precisamos elevar a root no sistema para executar chroot
> * Mesmo dentro do chroot, não tivemos isolamento de processos

# Parte 3 -  Importar a estrutura para o seu motor de contêiner

Agora que a árvore do FHS está criada dentro da pasta ./jail, ou baixada no ./debian-rootfs, você pode compactar e importar essa estrutura diretamente como uma nova imagem de contêiner.

## 3.1. Se estiver utilizando o Docker:
Compacte e envie diretamente para o gerenciador com o comando docker import:
```bash
sudo tar -C debian-rootfs -c . | docker import - debian-custom:minimal
```

## 3.2. Se estiver utilizando ferramentas OCI mais baixas (como runc ou podman):
Você pode testar rodando diretamente a partir do diretório usando o Podman:
```bash
podman run --rm --rootfs ./debian-rootfs /bin/sh -c 'cat /etc/os-release'
```
