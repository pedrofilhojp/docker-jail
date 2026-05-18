# Projeto — Monitoramento e Auto-Recovery de Containers com Health Check

## Tema do Projeto

Desenvolvimento de um sistema automatizado de monitoramento de containers Docker capaz de verificar a saúde de aplicações em execução e realizar ações automáticas de recuperação em caso de falhas.

---

# Objetivo do Projeto (Versão Melhorada)

O objetivo deste projeto é desenvolver uma solução baseada em containers capaz de monitorar continuamente a saúde de outros containers Docker e executar ações automáticas de recuperação quando forem identificadas falhas ou indisponibilidades.

A aplicação deverá atuar como um sistema de supervisão, realizando health checks periódicos nos containers cadastrados e validando se as aplicações continuam operando corretamente.

Caso algum problema seja identificado, o sistema deverá:

* detectar a falha;
* registrar o evento;
* reiniciar automaticamente o container afetado;
* monitorar o retorno da aplicação;
* registrar histórico das ocorrências.

O projeto busca explorar conceitos fundamentais relacionados a:

* health checks;
* alta disponibilidade;
* automação operacional;
* observabilidade;
* gerenciamento de containers;
* auto-healing;
* tolerância a falhas;
* supervisão de serviços.

Além disso, os alunos irão compreender conceitos utilizados em plataformas modernas como:

* **Kubernetes**;
* **Docker Swarm**;
* **systemd**;
* **Monit**;
* **supervisord**.

---

# Competências Esperadas

Ao final do projeto, espera-se que os alunos sejam capazes de:

* compreender conceitos de health check;
* monitorar containers Docker;
* automatizar recuperação de serviços;
* utilizar a API Docker;
* implementar mecanismos de supervisão;
* compreender tolerância a falhas;
* trabalhar com automação operacional;
* registrar logs e eventos;
* implementar observabilidade básica.

---

# Requisitos Mínimos

O projeto deverá obrigatoriamente:

1. Monitorar containers Docker;
2. Permitir cadastro de containers monitorados;
3. Realizar health checks periódicos;
4. Detectar indisponibilidade do serviço;
5. Reiniciar automaticamente containers com falha;
6. Registrar logs das ocorrências;
7. Monitorar continuamente o retorno do serviço;
8. Funcionar em ambiente Linux com Docker;
9. Possuir configuração simples;
10. Exibir status atual dos containers monitorados.

---

# Exemplos de Estratégias de Health Check

Os alunos poderão implementar verificações como:

* teste HTTP;
* conexão TCP;
* verificação de processo;
* teste de porta;
* execução de comando interno;
* validação de resposta da aplicação;
* consumo de CPU/memória;
* status do Docker HealthCheck.

---

# Exemplo de Configuração

<pre class="overflow-visible! px-0!" data-start="2761" data-end="2988"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>containers:</span><br/><span>  - name: app-fin</span><br/><span>    healthcheck:</span><br/><span>      type: http</span><br/><span>      url: http://app-fin:8080/health</span><br/><span>      interval: 30</span><br/><br/><span>  - name: nginx</span><br/><span>    healthcheck:</span><br/><span>      type: tcp</span><br/><span>      host: nginx</span><br/><span>      port: 80</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Fluxo Esperado

## 1. Monitoramento periódico

A aplicação realiza:

* consultas;
* testes;
* verificações de status.

---

## 2. Falha detectada

Exemplo:

<pre class="overflow-visible! px-0!" data-start="3152" data-end="3218"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>Health check falhou para container app-fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 3. Recuperação automática

Fluxo:

<pre class="overflow-visible! px-0!" data-start="3262" data-end="3379"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>Parando container</span><br/><span>Reiniciando container</span><br/><span>Aguardando inicialização</span><br/><span>Executando novo health check</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 4. Retorno do serviço

Exemplo:

<pre class="overflow-visible! px-0!" data-start="3421" data-end="3485"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>Container app-fin recuperado com sucesso</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Implementação

## Integração com Docker API

Uma das principais sugestões é utilizar o socket Docker:

<pre class="overflow-visible! px-0!" data-start="3609" data-end="3653"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

permitindo comunicação direta com o Docker Engine.

---

# Exemplo de montagem do Docker Socket

<pre class="overflow-visible! px-0!" data-start="3752" data-end="3830"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>volumes:</span><br/><span>  - /var/run/docker.sock:/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Possibilidades Utilizando Docker Socket

Os alunos poderão:

* listar containers;
* obter status;
* reiniciar containers;
* consultar health status;
* acompanhar eventos do Docker;
* verificar logs.

---

# Sugestão de Arquitetura

<pre class="overflow-visible! px-0!" data-start="4070" data-end="4641"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>+-----------------------------+</span><br/><span>| Container Health Monitor    |</span><br/><span>|-----------------------------|</span><br/><span>| Health Check Engine         |</span><br/><span>| Auto Recovery               |</span><br/><span>| Docker API Client           |</span><br/><span>+--------------+--------------+</span><br/><span>               |</span><br/><span>               | docker.sock</span><br/><span>               v</span><br/><span>+-----------------------------+</span><br/><span>| Docker Engine               |</span><br/><span>+-----------------------------+</span><br/><span>               |</span><br/><span>      +--------+--------+</span><br/><span>      |                 |</span><br/><span>+-----------+     +-----------+</span><br/><span>| App 1     |     | App 2     |</span><br/><span>+-----------+     +-----------+</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Estratégias de Detecção

## 1. HTTP Health Check

Exemplo:

<pre class="overflow-visible! px-0!" data-start="4709" data-end="4744"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>GET /health</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 2. TCP Check

Validar:

* porta aberta;
* resposta TCP.

---

## 3. Docker Native HealthCheck

Consultar:

<pre class="overflow-visible! px-0!" data-start="4859" data-end="4897"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker inspect</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 4. Process Check

Verificar:

* PID ativo;
* processo principal.

---

# Linguagens Sugeridas

* Python;
* Go;
* Node.js;
* Bash.

---

# Bibliotecas Úteis

## Python

* `docker`
* `requests`
* `schedule`

## Go

* Docker SDK
* HTTP client

---

# Requisitos Opcionais (Bônus)

Os grupos poderão implementar:

* dashboard web;
* métricas Prometheus;
* integração Grafana;
* notificações:
  * Telegram;
  * Discord;
  * Email;
* múltiplos tipos de health checks;
* rollback automático;
* retry inteligente;
* limite de reinicializações;
* análise de logs;
* persistência SQLite;
* cluster distribuído;
* monitoramento multi-host;
* escalabilidade automática.

---

# Sugestões Técnicas Avançadas

## Auto-Healing Inteligente

Evitar:

* loop infinito de restart;
* restart excessivo.

---

## Circuit Breaker

Bloquear temporariamente:

* containers instáveis.

---

## Graceful Recovery

Aguardar:

* tempo de inicialização;
* warm-up da aplicação.

---

# Sugestão de Estrutura do Projeto

<pre class="overflow-visible! px-0!" data-start="5892" data-end="6043"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>project/</span><br/><span>├── monitor/</span><br/><span>├── healthchecks/</span><br/><span>├── recovery/</span><br/><span>├── logs/</span><br/><span>├── config/</span><br/><span>├── database/</span><br/><span>├── docker/</span><br/><span>├── main.py</span><br/><span>└── README.md</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Modelo de Avaliação


| Critério                     | Peso |
| ------------------------------- | ------ |
| Funcionamento do health check | 25%  |
| Recuperação automática     | 25%  |
| Integração com Docker API   | 15%  |
| Confiabilidade da solução   | 10%  |
| Organização do código      | 10%  |
| Documentação                | 10%  |
| Funcionalidades extras        | 5%   |

---

# Forma de Entrega

## Cada grupo deverá entregar:

### 1. Código-fonte

Disponibilizado em repositório Git.

---

### 2. README.md contendo

* arquitetura da solução;
* tipos de health checks;
* funcionamento da recuperação;
* instruções de uso;
* limitações conhecidas.

---

### 3. Relatório técnico

Deve conter:

* funcionamento dos health checks;
* estratégia de recuperação;
* integração com Docker;
* problemas encontrados;
* aprendizados obtidos.

---

### 4. Vídeo demonstrativo

Entre 5 e 15 minutos contendo:

* falha simulada;
* detecção automática;
* restart do container;
* recuperação do serviço.

---

# Sugestões de Avaliação Prática

Durante a apresentação, o professor poderá solicitar:

* derrubar manualmente um serviço;
* demonstrar a recuperação automática;
* exibir logs do monitoramento;
* explicar o algoritmo de health check;
* demonstrar múltiplos containers;
* mostrar integração com Docker API.

---

# Perguntas Sobre o Projeto

1. O que é um health check?
2. Qual a diferença entre disponibilidade e funcionamento correto?
3. Como o Docker implementa health checks nativamente?
4. Qual a função do:

<pre class="overflow-visible! px-0!" data-start="7473" data-end="7517"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

5. Quais riscos de segurança existem ao utilizar docker.sock?
6. O que é auto-healing?
7. Como evitar loops infinitos de reinicialização?
8. Qual a diferença entre monitoramento passivo e ativo?
9. Como validar se uma aplicação realmente está saudável?
10. Qual a diferença entre restart policy e health check?
11. Como plataformas como **Kubernetes** realizam auto-recovery?
12. O que acontece se o container reiniciar mas a aplicação continuar falhando?
13. Como implementar notificações de falha?
14. Qual a importância de logs em sistemas de monitoramento?
15. Como detectar falhas intermitentes?
16. Como monitorar containers distribuídos em múltiplos hosts?
17. Como implementar alta disponibilidade para o próprio monitor?
18. Qual a relação entre observabilidade e health checks?

---

# Sugestões de Expansão do Projeto

Os grupos mais avançados poderão evoluir o projeto para:

* criar sistema semelhante ao auto-healing do **Kubernetes**;
* implementar monitoramento distribuído;
* suportar múltiplos hosts Docker;
* criar dashboard observabilidade;
* integrar Prometheus e Grafana;
* implementar inteligência para análise de falhas;
* adicionar machine learning para previsão de falhas;
* criar sistema completo de gerenciamento de containers.

Isso permitirá aos alunos compreender profundamente como plataformas modernas realizam monitoramento, tolerância a falhas e recuperação automática em ambientes distribuídos e containerizados.
