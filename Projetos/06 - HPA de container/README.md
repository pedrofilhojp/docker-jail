# Projeto — Autoscaling Horizontal de Containers no Docker (HPA Simplificado)

## Tema do Projeto

Desenvolvimento de um sistema de autoscaling horizontal para containers Docker, inspirado no funcionamento do HPA (Horizontal Pod Autoscaler) do **Kubernetes**.

---

# Objetivo do Projeto (Versão Melhorada)

O objetivo deste projeto é desenvolver uma solução automatizada capaz de monitorar o consumo de recursos de containers Docker e realizar escalabilidade horizontal automática baseada em métricas de utilização de CPU e memória.

A aplicação deverá atuar de forma semelhante ao HPA (Horizontal Pod Autoscaler) do **Kubernetes**, monitorando continuamente containers específicos e criando ou removendo réplicas automaticamente de acordo com regras definidas de consumo.

Os containers que poderão sofrer escalabilidade deverão ser identificados através de labels (tags) Docker, como por exemplo:

<pre class="overflow-visible! px-0!" data-start="946" data-end="1068"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>autoscale.enable=true</span><br/><span>autoscale.min=1</span><br/><span>autoscale.max=5</span><br/><span>autoscale.cpu=70</span><br/><span>autoscale.memory=80</span><br/><span>app=fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Com base nessas informações, o sistema deverá:

* monitorar continuamente containers elegíveis;
* obter métricas de CPU e memória;
* detectar sobrecarga;
* criar novas réplicas automaticamente;
* remover réplicas quando houver baixa utilização;
* manter limites mínimos e máximos de instâncias;
* registrar eventos de scaling.

O projeto busca proporcionar aos alunos uma compreensão prática sobre:

* autoscaling horizontal;
* orquestração de containers;
* monitoramento de recursos;
* observabilidade;
* automação operacional;
* balanceamento de carga;
* tolerância a falhas;
* elasticidade computacional.

Além disso, os alunos irão explorar conceitos semelhantes aos utilizados por plataformas modernas como:

* **Kubernetes**;
* **Docker Swarm**;
* **Nomad**;
* **OpenShift**.

---

# Competências Esperadas

Ao final do projeto, espera-se que os alunos sejam capazes de:

* compreender conceitos de autoscaling horizontal;
* monitorar consumo de CPU e memória;
* consumir métricas de containers Docker;
* automatizar criação de containers;
* implementar políticas de scaling;
* compreender elasticidade computacional;
* trabalhar com automação de infraestrutura;
* implementar monitoramento contínuo;
* utilizar APIs do Docker.

---

# Requisitos Mínimos

O projeto deverá obrigatoriamente:

1. Monitorar containers Docker;
2. Selecionar containers através de labels;
3. Obter métricas de CPU;
4. Obter métricas de memória;
5. Implementar scaling horizontal automático;
6. Criar novas réplicas automaticamente;
7. Remover réplicas automaticamente;
8. Respeitar limites mínimo e máximo;
9. Registrar eventos de scaling;
10. Funcionar em ambiente Linux com Docker.

---

# Exemplo de Labels Esperadas

Os containers elegíveis poderão possuir:

<pre class="overflow-visible! px-0!" data-start="2911" data-end="3065"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>labels:</span><br/><span>  - autoscale.enable=true</span><br/><span>  - autoscale.min=1</span><br/><span>  - autoscale.max=5</span><br/><span>  - autoscale.cpu=70</span><br/><span>  - autoscale.memory=80</span><br/><span>  - app=fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Exemplo de Funcionamento Esperado

## Cenário Inicial

Container:

<pre class="overflow-visible! px-0!" data-start="3140" data-end="3173"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>fin-app-1</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

CPU:

<pre class="overflow-visible! px-0!" data-start="3180" data-end="3207"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>85%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Threshold:

<pre class="overflow-visible! px-0!" data-start="3220" data-end="3247"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>70%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Resultado esperado

O sistema deverá:

* detectar sobrecarga;
* criar nova réplica:

<pre class="overflow-visible! px-0!" data-start="3340" data-end="3373"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>fin-app-2</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Cenário posterior

Carga reduzida:

<pre class="overflow-visible! px-0!" data-start="3418" data-end="3445"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>25%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

O sistema poderá:

* remover réplicas excedentes;
* retornar à quantidade mínima configurada.

---

# Sugestão de Implementação

## Integração com Docker API

Uma das principais sugestões é utilizar o Docker Socket:

<pre class="overflow-visible! px-0!" data-start="3663" data-end="3707"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

permitindo comunicação direta com o Docker Engine.

---

# Exemplo de montagem do Docker Socket

<pre class="overflow-visible! px-0!" data-start="3806" data-end="3884"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>volumes:</span><br/><span>  - /var/run/docker.sock:/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Arquitetura

<pre class="overflow-visible! px-0!" data-start="3918" data-end="4567"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>+----------------------------------+</span><br/><span>| Container HPA Controller         |</span><br/><span>|----------------------------------|</span><br/><span>| Metrics Collector                |</span><br/><span>| Scaling Engine                   |</span><br/><span>| Docker API Client                |</span><br/><span>+----------------+-----------------+</span><br/><span>                 |</span><br/><span>                 | docker.sock</span><br/><span>                 v</span><br/><span>+----------------------------------+</span><br/><span>| Docker Engine                    |</span><br/><span>+----------------------------------+</span><br/><span>                 |</span><br/><span>      +----------+----------+</span><br/><span>      |                     |</span><br/><span>+------------+       +------------+</span><br/><span>| fin-app-1  |       | fin-app-2  |</span><br/><span>+------------+       +------------+</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Implementação — Coleta de CPU e Memória

## Opção 1 — Docker Stats (Mais simples)

Os alunos poderão utilizar:

<pre class="overflow-visible! px-0!" data-start="4700" data-end="4736"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker stats</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Exemplo:

<pre class="overflow-visible! px-0!" data-start="4747" data-end="4795"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker stats </span><span class="ͼ12">--no-stream</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Opção 2 — Docker API (Recomendado)

Consultar métricas diretamente via API Docker utilizando:

<pre class="overflow-visible! px-0!" data-start="4899" data-end="4945"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/containers/{id}/stats</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Exemplo de Informações Disponíveis

A API poderá fornecer:

* uso de CPU;
* limite de CPU;
* uso de memória;
* limite de memória;
* IO;
* rede;
* PIDs.

---

# Exemplo Simplificado de Métrica

<pre class="overflow-visible! px-0!" data-start="5146" data-end="5248"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>{</span><br/><span>  "memory_usage": </span><span class="ͼz">"512MB"</span><span>,</span><br/><span>  "memory_limit": </span><span class="ͼz">"1GB"</span><span>,</span><br/><span>  "cpu_percent": </span><span class="ͼz">"82%"</span><br/><span>}</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Estratégias de Scaling

## Scale Out

Criar novos containers quando:

<pre class="overflow-visible! px-0!" data-start="5326" data-end="5359"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>CPU > 70%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

ou:

<pre class="overflow-visible! px-0!" data-start="5365" data-end="5398"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>MEM > 80%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Scale In

Remover containers quando:

<pre class="overflow-visible! px-0!" data-start="5445" data-end="5478"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>CPU < 20%</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

durante determinado período.

---

# Sugestões Técnicas Importantes

## Evitar Scaling Excessivo

Os alunos deverão implementar:

* cooldown;
* janela de observação;
* média móvel;
* limite de frequência.

---

# Sugestões de Balanceamento

O projeto poderá ser integrado com:

* **NGINX**;
* **HAProxy**;
* projeto anterior de auto-discovery.

---

# Linguagens Sugeridas

* Python;
* Go;
* Node.js.

---

# Bibliotecas Úteis

## Python

* `docker`
* `psutil`
* `schedule`

## Go

* Docker SDK
* Prometheus client

---

# Requisitos Opcionais (Bônus)

Os grupos poderão implementar:

* integração com load balancer;
* dashboard web;
* métricas Prometheus;
* integração Grafana;
* autoscaling baseado em requisições;
* cooldown inteligente;
* suporte multi-host;
* persistência SQLite;
* notificações;
* escalabilidade preditiva;
* suporte a GPU;
* algoritmos avançados de scaling.

---

# Possíveis Estratégias Avançadas

## Média móvel

Evitar scaling baseado em picos instantâneos.

---

## Histerese

Evitar:

* scale in/out contínuo.

---

## Warm-Up

Aguardar:

* inicialização da nova réplica antes de considerar saudável.

---

# Sugestão de Estrutura do Projeto

<pre class="overflow-visible! px-0!" data-start="6700" data-end="6848"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>project/</span><br/><span>├── autoscaler/</span><br/><span>├── metrics/</span><br/><span>├── scaling/</span><br/><span>├── logs/</span><br/><span>├── config/</span><br/><span>├── database/</span><br/><span>├── docker/</span><br/><span>├── main.py</span><br/><span>└── README.md</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Modelo de Avaliação


| Critério                    | Peso |
| ------------------------------ | ------ |
| Coleta correta de métricas  | 20%  |
| Funcionamento do autoscaling | 25%  |
| Integração com Docker API  | 20%  |
| Estratégia de scaling       | 10%  |
| Organização do código     | 10%  |
| Documentação               | 10%  |
| Funcionalidades extras       | 5%   |

---

# Forma de Entrega

## Cada grupo deverá entregar:

### 1. Código-fonte

Disponibilizado em repositório Git.

---

### 2. README.md contendo

* arquitetura da solução;
* política de scaling;
* labels suportadas;
* estratégia de coleta de métricas;
* instruções de execução.

---

### 3. Relatório técnico

Deve conter:

* funcionamento do HPA;
* métricas utilizadas;
* política de scaling;
* integração com Docker;
* dificuldades encontradas;
* aprendizados obtidos.

---

### 4. Vídeo demonstrativo

Entre 5 e 15 minutos contendo:

* aumento de carga;
* criação automática de réplicas;
* redução de carga;
* remoção automática de réplicas;
* exibição das métricas.

---

# Sugestões de Avaliação Prática

Durante a apresentação, o professor poderá solicitar:

* geração artificial de carga;
* demonstração do scale out;
* demonstração do scale in;
* exibição das métricas;
* explicação da política de scaling;
* integração com balanceador;
* demonstração dos thresholds.

---

# Perguntas Sobre o Projeto

1. O que é autoscaling horizontal?
2. Qual a diferença entre scaling horizontal e vertical?
3. Como o HPA do **Kubernetes** funciona?
4. Como obter métricas de CPU e memória no Docker?
5. Qual a função do:

<pre class="overflow-visible! px-0!" data-start="8382" data-end="8426"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

6. Quais riscos existem ao utilizar docker.sock?
7. Como evitar scaling excessivo?
8. O que é cooldown em autoscaling?
9. O que é histerese em sistemas de scaling?
10. Como balanceadores de carga trabalham junto com autoscaling?
11. Como detectar containers saudáveis?
12. O que acontece se novas réplicas forem criadas rapidamente demais?
13. Como implementar scale in com segurança?
14. Qual a importância da observabilidade em autoscaling?
15. Como plataformas modernas evitam flapping de scaling?
16. Como seria possível implementar autoscaling preditivo?
17. Qual a relação entre este projeto e cloud computing?
18. Como o autoscaling impacta custos e disponibilidade?

---

# Sugestões de Expansão do Projeto

Os grupos mais avançados poderão evoluir o projeto para:

* criar sistema semelhante ao HPA do **Kubernetes**;
* implementar integração com Prometheus;
* criar dashboard observabilidade;
* integrar com load balancer dinâmico;
* implementar autoscaling distribuído;
* adicionar IA para previsão de carga;
* suportar múltiplos hosts Docker;
* criar mini orquestrador de containers.

Isso permitirá aos alunos compreender profundamente como plataformas modernas realizam elasticidade automática, gerenciamento de carga e orquestração de aplicações containerizadas em ambientes distribuídos.
