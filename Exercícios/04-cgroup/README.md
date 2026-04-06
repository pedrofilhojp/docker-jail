# ⚙️ 1. Limitando recursos com CGroups
## 1.1 O que é cgroup?
Cgroup (abreviação de Control Groups) é uma funcionalidade do kernel do Linux que permite limitar recursos do sistema (como CPU, memória, disco e rede) para grupos de processos. Ele é muito usado em ambientes de contêiner (como Docker) e sistemas que precisam garantir que nenhum processo ultrapasse certos limites de recurso. 
Imagine que você tem diferentes "caixas" virtuais dentro do seu sistema, e você pode colocar grupos de processos nessas caixas e definir limites para o quanto cada caixa pode "comer" dos recursos do seu computador.

## 1.2. Como funciona o cgroup?
O cgroup organiza processos em grupos hierárquicos, onde cada grupo pode ter políticas específicas de uso de recursos. Ele funciona com subsystems (ou controladores), cada um responsável por um tipo de recurso.

Exemplos de controladores:
- cpu – limita ou compartilha o **tempo** de CPU.
- memory – restringe o uso de memória.
- blkio – controla o uso de I/O em disco.
- cpuset – Restringe os processos em um cgroup a rodarem em um conjunto específico de CPUs
- net_cls – classifica pacotes de rede para controle de QoS.
- pids - Limita o número de processos que podem ser criados dentro de um cgroup.

Você pode verificar todos os controladores executando o comando:
```bash
cat /proc/cgroups  | awk '{print $1}'
```

 ### 1.2.1. Hierarquia: 
 Os cgroups são organizados em uma estrutura de árvore, chamada hierarquia. 
 Cada hierarquia é associada a um ou mais subsistemas. Isso permite que 
 você organize seus grupos de processos de forma lógica e aplique 
 controles de recursos de maneira granular. Um processo pode pertencer 
 a vários cgroups, desde que cada um esteja em uma hierarquia diferente.

 ### 1.2.3. Cgroups (os grupos em si):
 Dentro de uma hierarquia, você cria os cgroups. Cada cgroup é um 
 diretório no sistema de arquivos virtual cgroup 
 (geralmente montado em **/sys/fs/cgroup**). Dentro desses diretórios, 
 você encontra arquivos que permitem configurar os limites e as 
 estatísticas de uso dos recursos para os processos pertencentes a esse grupo.

Caso não exista nada no diretório "**/sys/fs/cgroup**", você pode montá-lo desta forma:
```bash
sudo mount -t cgroup2 none /sys/fs/cgroup
```

### Em resumo, o fluxo geral é

1. Montar uma ou mais hierarquias de cgroup, associando a cada uma os subsistemas que você deseja controlar;
2. Criar diretórios (os cgroups propriamente ditos) dentro dessas hierarquias;
3. Configurar os limites de recursos desejados escrevendo valores nos arquivos de configuração dentro dos diretórios dos cgroups;
4. Mover os PIDs dos processos que você quer controlar para o arquivo tasks dentro do diretório do cgroup.

## 1.3. Exemplos de uso (na prática)
Vamos utilizar o programa "stress", com ele é possível controlar alocação de 
cpu e memória de forma contralada.

### 1.3.1. Preparando o ambiente
Abra 2 terminais com usuário root, em 1 deles você deixará o programa 
"**htop**" aberto para monitoramento do sistema.
No outro, você utilizará os comandos a seguir para manipulação do cgroup.

Antes de iniciar, execute o **stress** para observar o consumo de recurso 
do sistema

```bash
stress --cpu 2  --vm 1 --vm-bytes 1G --timeout 20s
```

### 1.3.2. Criar um cgroup
Antes, você precisa instalar o conjunto de ferramentas para manipulação do cgroup
```bash
apt install cgroup-tools
```
Criando a hierarquia de diretório cgroup
```bash
sudo cgcreate -g cpu:/meu_grupo
ou
sudo mkdir /sys/fs/cgroup/meu_grupo
```

### 1.3.3. Configurar limites (cpu e memória):

CPU (50% de um núcleo)
```bash
cgset -r cpu.max="50000 100000" meu_grupo 
ou
echo "50000 100000" | sudo tee /sys/fs/cgroup/meu_grupo/cpu.max
```

Memória (limite de 1GB)
```bash
cgset -r memory.max="50000 100000" meu_grupo
ou
echo "100M" | sudo tee /sys/fs/cgroup/meu_grupo/memory.max
```
### 1.3.4. Incluir o processo do terminal ao cgroup:
Desta forma, tudo que executar no terminal, terá os recursos 
limitados de acordo com as configurações do nosso cgroup
```bash
echo $$ | sudo tee /sys/fs/cgroup/meu_grupo/cgroup.procs
```
Obs. O arquivo "**cgroup.procs**" armazena os PIDs dos processos pertencentes 
ao cgroup. O mesmo processo não pode está em 2 cgroups distintos. Logo,
se desejar retirar o processo deste terminal deste cgroup, 
basta adicioná-lo em outro por meio do programa "**cgclassify**" ou manualmente.

### 1.3.5. Executar o stress:

Execute o stress e observe o consumo de recurso no outro terminal pelo htop.

```bash
stress --cpu 2  --vm 1 --vm-bytes 1G --timeout 20s
```
 
 ## 1.4. Reflexão
 Até o momento, contruímos herarquia de cgroups de forma manual utilizando
 programas que lidam a "**libcgroup**". Esta não é uma boa estratégia 
 e será descontinuada, pois pode haver conclitos com cgroups atuais 
 do sistema e apenas ainda está disponível para cobrir casos específicos 
 que o systemd não é aplicado.
 
[Fonte de dados - Redhat Docs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/resource_management_guide/chap-introduction_to_control_groups#sec-What_are_Control_Groups)

# 2. Criando Slices do Cgroup no SystemD

## 2.1. O que é um slice no systemd?
No systemd, um slice é uma unidade especial usada para organizar, agrupar e gerenciar recursos (CPU, memória, I/O) de vários serviços e processos em cgroups.

Um slice é como uma "fatia" de recursos do sistema, onde você pode colocar vários serviços/processos e aplicar limites coletivos a eles.

## 2.2. Como funciona um slice?
O systemd cria automaticamente alguns slices básicos, tipo:
- system.slice: onde ficam os serviços normais (sshd, nginx, etc).
- user.slice: onde ficam sessões de usuários (ex.: GNOME, Terminal).
- init.scope: o processo init e seus filhos diretos.

Cada slice corresponde a um cgroup no sistema.

Você pode criar novos slices personalizados, e qualquer serviço (ou conjunto de serviços) pode ser colocado dentro de um slice.

Dá para definir políticas de uso de recursos no nível do slice (e isso afeta todos os serviços dentro dele).

## 2.3. Para que serve um slice?
1. Agrupar serviços: organizar processos que fazem parte de uma mesma aplicação ou função.

2. Aplicar limites de recursos: controlar CPU, memória, disco, rede para grupos inteiros.

3. Priorizar ou depriorizar processos: dar mais ou menos acesso a recursos para certos conjuntos.

4. Proteger o sistema: evitar que um grupo de processos cause um colapso consumindo tudo (tipo um ataque DoS interno).

## 2.4. Exemplo prático: Criando um slice personalizado
Crie um novo slice:
```bash
sudo nano /etc/systemd/system/minhaapp.slice
```
Conteúdo básico:

```bash
[Unit]
Description=Slice para Minha Aplicação

[Slice]
CPUQuota=50%
MemoryMax=500M
```
- CPUQuota=50%: o grupo inteiro pode usar no máximo metade da CPU.
- MemoryMax=500M: o grupo inteiro pode usar no máximo 500 MB de RAM.

Recarregue e ative:

```bash
sudo systemctl daemon-reload
sudo systemctl start minhaapp.slice
```

Colocar um serviço dentro do slice
Se você quiser que um serviço rode dentro do seu slice, no .service dele adicione:

```bash
[Service]
Slice=minhaapp.slice
```
Exemplo (meuapp.service):

```bash
[Unit]
Description=Meu App

[Service]
ExecStart=/usr/bin/sleep infinity
Slice=minhaapp.slice

[Install]
WantedBy=multi-user.target
```
Assim, o serviço meuapp.service será limitado pelas regras do 
"**minhaapp.slice**"

## 🔍 2.5. Como visualizar os slices ativos?
Listar slices:

```bash
systemctl -t slice
```

Monitorar consumo por slice (em tempo real):
```bash
systemd-cgtop
```

Inspecionar um slice:
```bash
systemctl status minhaapp.slice
```
[Fonte de dados - Redhat Docs](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/7/html/resource_management_guide/sec-obtaining_information_about_control_groups#sec-Listing_Units)