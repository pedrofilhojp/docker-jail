# Projeto — Atualização Automática de Containers Baseada em Novas Versões de Imagens Docker

## Tema do Projeto

Desenvolvimento de um sistema automatizado capaz de monitorar imagens Docker com tag `latest`, detectar novas versões publicadas em um registry e atualizar automaticamente os containers em execução.

---

# Objetivo do Projeto (Versão Melhorada)

O objetivo deste projeto é desenvolver uma solução automatizada capaz de monitorar periodicamente imagens Docker publicadas em registries de containers e identificar alterações em imagens que utilizam a tag:

<pre class="overflow-visible! px-0!" data-start="568" data-end="598"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Quando uma nova versão da imagem for detectada no registry, o sistema deverá:

* identificar a atualização;
* baixar a nova imagem;
* interromper o container antigo;
* remover o container desatualizado;
* criar um novo container utilizando a imagem atualizada;
* restaurar a execução da aplicação automaticamente.

O projeto busca explorar conceitos fundamentais relacionados a:

* gerenciamento de containers;
* automação de infraestrutura;
* versionamento de imagens;
* registries Docker;
* CI/CD;
* rolling updates;
* atualização contínua;
* observabilidade e automação operacional.

Os alunos deverão compreender como plataformas modernas realizam atualização automatizada de workloads containerizados, explorando conceitos semelhantes aos utilizados por ferramentas como:

* **Watchtower**;
* **Kubernetes**;
* **Docker Swarm**;
* **Argo CD**.

---

# Competências Esperadas

Ao final do projeto, espera-se que os alunos sejam capazes de:

* compreender o funcionamento de registries Docker;
* monitorar alterações em imagens;
* automatizar atualização de containers;
* utilizar a API do Docker;
* trabalhar com automação operacional;
* compreender estratégias de atualização contínua;
* manipular imagens Docker;
* automatizar processos de deploy;
* implementar mecanismos de monitoramento.

---

# Requisitos Mínimos

O projeto deverá obrigatoriamente:

1. Monitorar periodicamente uma imagem Docker;
2. Trabalhar com imagens utilizando tag`latest`;
3. Verificar se houve alteração da imagem no registry;
4. Fazer download automático da nova imagem;
5. Identificar containers utilizando a imagem antiga;
6. Parar o container atual;
7. Remover o container antigo;
8. Criar novo container utilizando a nova imagem;
9. Restaurar a execução automaticamente;
10. Funcionar em ambiente Linux com Docker.

---

# Exemplo de Funcionamento Esperado

## Cenário Inicial

Container em execução:

<pre class="overflow-visible! px-0!" data-start="2583" data-end="2644"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker run </span><span class="ͼ12">-d</span><span> </span><span class="ͼ12">--name</span><span> app nginx:latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Fluxo esperado

### 1. Verificação periódica

A aplicação verifica:

* digest da imagem;
* ID da imagem;
* data de atualização;
* manifesto remoto.

---

### 2. Nova versão detectada

Caso o registry possua nova versão:

<pre class="overflow-visible! px-0!" data-start="2873" data-end="2936"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>Nova imagem detectada para nginx:latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

### 3. Atualização automática

Fluxo:

<pre class="overflow-visible! px-0!" data-start="2981" data-end="3126"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>Parando container antigo</span><br/><span>Removendo container</span><br/><span>Baixando nova imagem</span><br/><span>Criando novo container</span><br/><span>Container atualizado com sucesso</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Implementação

## Estratégias possíveis

Os alunos poderão utilizar:

### 1. Docker CLI

Exemplo:

<pre class="overflow-visible! px-0!" data-start="3245" data-end="3318"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker pull nginx:latest</span><br/><span>docker inspect</span><br/><span>docker </span><span class="ͼ10">ps</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

### 2. API Docker (Recomendado)

Utilizar:

<pre class="overflow-visible! px-0!" data-start="3368" data-end="3412"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

permitindo acesso à API Docker diretamente do container de gerenciamento.

---

# Exemplo de montagem do Docker Socket

<pre class="overflow-visible! px-0!" data-start="3534" data-end="3612"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>volumes:</span><br/><span>  - /var/run/docker.sock:/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Arquitetura

<pre class="overflow-visible! px-0!" data-start="3646" data-end="4249"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>+-----------------------------+</span><br/><span>| Container Auto-Updater      |</span><br/><span>|-----------------------------|</span><br/><span>| Monitor de imagens          |</span><br/><span>| API Docker                  |</span><br/><span>| Scheduler                   |</span><br/><span>+--------------+--------------+</span><br/><span>               |</span><br/><span>               | docker.sock</span><br/><span>               v</span><br/><span>+-----------------------------+</span><br/><span>| Docker Engine               |</span><br/><span>+-----------------------------+</span><br/><span>               |</span><br/><span>      +--------+--------+</span><br/><span>      |                 |</span><br/><span>+-----------+     +-----------+</span><br/><span>| Container |     | Registry  |</span><br/><span>| Atual     |     | DockerHub |</span><br/><span>+-----------+     +-----------+</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Estratégias de Verificação

## 1. Comparação de digest

Verificar:

* SHA256 local;
* SHA256 remoto.

---

## 2. Docker pull periódico

Executar:

<pre class="overflow-visible! px-0!" data-start="4403" data-end="4452"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker pull imagem:latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

e verificar se houve alteração.

---

## 3. Consulta ao Registry API

Consultar:

* manifestos;
* tags;
* timestamps;
* digest remoto.

---

# Sugestões Técnicas

## Linguagens sugeridas

* Python;
* Go;
* Bash;
* Node.js.

---

# Bibliotecas Úteis

## Python

* `docker`
* `requests`
* `schedule`

## Go

* Docker SDK
* Cron libraries

---

# Requisitos Opcionais (Bônus)

Os grupos poderão implementar:

* rolling update;
* health check antes da troca;
* rollback automático;
* dashboard web;
* logs persistentes;
* atualização paralela;
* suporte a múltiplos containers;
* notificações:
  * email;
  * Telegram;
  * Discord;
* métricas Prometheus;
* integração Grafana;
* suporte OCI Registry;
* atualização baseada em semver;
* política de atualização;
* múltiplos registries.

---

# Possíveis Estratégias Avançadas

## Rolling Update

Atualizar:

* sem downtime;
* mantendo disponibilidade.

---

## Blue/Green Deployment

Criar novo container antes de remover o antigo.

---

## Canary Update

Atualização parcial gradual.

---

# Sugestão de Estrutura do Projeto

<pre class="overflow-visible! px-0!" data-start="5522" data-end="5666"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>project/</span><br/><span>├── updater/</span><br/><span>├── scheduler/</span><br/><span>├── logs/</span><br/><span>├── config/</span><br/><span>├── docker/</span><br/><span>├── main.py</span><br/><span>├── docker-compose.yml</span><br/><span>└── README.md</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Modelo de Avaliação


| Critério                             | Peso |
| --------------------------------------- | ------ |
| Detecção correta de atualização   | 25%  |
| Atualização automática funcionando | 25%  |
| Integração com Docker API           | 15%  |
| Confiabilidade da automação         | 10%  |
| Organização do código              | 10%  |
| Documentação                        | 10%  |
| Funcionalidades extras                | 5%   |

---

# Forma de Entrega

## Cada grupo deverá entregar:

### 1. Código-fonte

Disponibilizado em repositório Git.

---

### 2. README.md contendo

* arquitetura da solução;
* funcionamento da detecção;
* estratégias de atualização;
* instruções de execução;
* limitações conhecidas.

---

### 3. Relatório técnico

Deve conter:

* funcionamento dos registries Docker;
* conceito de digest;
* política de atualização;
* integração com Docker;
* dificuldades encontradas;
* aprendizados obtidos.

---

### 4. Vídeo demonstrativo

Entre 5 e 15 minutos contendo:

* detecção da nova imagem;
* atualização automática;
* substituição do container;
* logs do processo.

---

# Sugestões de Avaliação Prática

Durante a apresentação, o professor poderá solicitar:

* atualização de uma imagem em tempo real;
* demonstração do monitoramento;
* explicação do digest da imagem;
* troca automática do container;
* demonstração do rollback;
* explicação da integração com Docker API.

---

# Perguntas Sobre o Projeto

1. O que é uma imagem Docker?
2. O que representa a tag:

<pre class="overflow-visible! px-0!" data-start="7036" data-end="7066"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

3. O que é digest de imagem?
4. Como detectar que uma imagem foi atualizada?
5. Qual a diferença entre tag e digest?
6. Como funciona um Docker Registry?
7. Quais riscos existem ao utilizar:

<pre class="overflow-visible! px-0!" data-start="7263" data-end="7293"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>latest</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

em produção?

8. Como o Docker armazena imagens localmente?
9. Como funciona o download incremental de layers?
10. O que acontece com containers em execução quando a imagem é atualizada?
11. Como implementar rollback automático?
12. Como evitar downtime durante atualização?
13. Qual a função do:

<pre class="overflow-visible! px-0!" data-start="7597" data-end="7641"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

14. Quais riscos de segurança existem ao expor docker.sock?
15. Como plataformas como **Kubernetes** realizam rolling updates?
16. O que são blue/green deployments?
17. Como validar se o novo container está saudável antes da troca?
18. Qual a diferença entre atualização automática e CI/CD?

---

# Sugestões de Expansão do Projeto

Os grupos mais avançados poderão evoluir o projeto para:

* criar ferramenta semelhante ao **Watchtower**;
* implementar dashboard web;
* suportar múltiplos hosts Docker;
* integrar com Kubernetes;
* implementar políticas inteligentes de atualização;
* adicionar observabilidade;
* criar sistema de rollback avançado;
* integrar notificações e auditoria.

Isso permitirá aos alunos compreender profundamente como plataformas modernas realizam atualização automatizada, gerenciamento contínuo e orquestração de containers em ambientes cloud-native.
