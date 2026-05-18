# Projeto — Load Balancer Dinâmico com Auto-Discovery de Containers no Docker

## Tema do Projeto

Desenvolvimento de um sistema de balanceamento de carga dinâmico utilizando **NGINX** com descoberta automática de containers Docker através de labels (tags) e integração com a API do Docker via socket Unix.

---

# Objetivo do Projeto (Versão Melhorada)

O objetivo deste projeto é desenvolver uma solução de load balancing baseada em containers, utilizando o **NGINX** como proxy reverso e balanceador de carga, capaz de descobrir automaticamente aplicações Docker em execução através de labels configuradas nos containers.

A solução deverá monitorar continuamente o ambiente Docker e identificar containers elegíveis para balanceamento utilizando labels como:

<pre class="overflow-visible! px-0!" data-start="819" data-end="878"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>lb.enable=true</span><br/><span>lb.port=9000</span><br/><span>app=fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Com base nessas informações, o sistema deverá:

* identificar automaticamente novos containers;
* descobrir IP e porta dos serviços;
* gerar dinamicamente a configuração do NGINX;
* atualizar o balanceamento automaticamente;
* remover containers indisponíveis;
* realizar reload do NGINX sem interrupção do serviço.

O projeto também deverá explorar a integração direta com a API do Docker através do socket Unix:

<pre class="overflow-visible! px-0!" data-start="1294" data-end="1338"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

permitindo que o próprio container do load balancer consulte eventos, containers ativos, labels e redes Docker em tempo real.

O projeto busca proporcionar aos alunos uma compreensão prática sobre:

* service discovery;
* reverse proxy;
* integração com API Docker;
* automação de infraestrutura;
* microsserviços;
* comunicação entre containers;
* balanceamento de carga dinâmico;
* arquiteturas modernas cloud-native.

Além disso, os alunos irão explorar conceitos semelhantes aos utilizados por ferramentas como:

* **Traefik**;
* **HAProxy**;
* **NGINX**;
* **Kubernetes**;
* **Docker Swarm**.

---

# Competências Esperadas

Ao final do projeto, espera-se que os alunos sejam capazes de:

* compreender o funcionamento de load balancers;
* configurar e automatizar o NGINX;
* utilizar labels em containers Docker;
* consumir a API do Docker;
* utilizar o socket Docker Unix;
* implementar service discovery;
* automatizar geração de configuração;
* trabalhar com redes Docker;
* compreender arquiteturas distribuídas;
* implementar reload dinâmico de serviços.

---

# Requisitos Mínimos

O projeto deverá obrigatoriamente:

1. Executar o NGINX em container;
2. Descobrir containers automaticamente;
3. Filtrar containers através de labels;
4. Consumir informações da API Docker;
5. Identificar IP e porta do container;
6. Gerar configuração dinâmica do NGINX;
7. Implementar balanceamento de carga;
8. Atualizar configuração automaticamente;
9. Recarregar o NGINX dinamicamente;
10. Remover containers indisponíveis do balanceamento;
11. Funcionar em ambiente Linux com Docker.

---

# Exemplo de Labels Esperadas

Os containers das aplicações poderão possuir:

<pre class="overflow-visible! px-0!" data-start="3130" data-end="3210"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>labels:</span><br/><span>  - lb.enable=true</span><br/><span>  - lb.port=9000</span><br/><span>  - app=fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Exemplo de Funcionamento Esperado

## Containers ativos

<pre class="overflow-visible! px-0!" data-start="3276" data-end="3329"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>fin-app-1</span><br/><span>fin-app-2</span><br/><span>fin-app-3</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Todos contendo:

<pre class="overflow-visible! px-0!" data-start="3348" data-end="3407"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>lb.enable=true</span><br/><span>lb.port=9000</span><br/><span>app=fin</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## Configuração gerada automaticamente

<pre class="overflow-visible! px-0!" data-start="3454" data-end="3677"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>upstream fin_backend {</span><br/><span>    server 172.18.0.2:9000;</span><br/><span>    server 172.18.0.3:9000;</span><br/><span>    server 172.18.0.4:9000;</span><br/><span>}</span><br/><br/><span>server {</span><br/><span>    listen 80;</span><br/><br/><span>    location / {</span><br/><span>        proxy_pass http://fin_backend;</span><br/><span>    }</span><br/><span>}</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Sugestão de Implementação — API Docker via Docker Socket

## Uso do socket Docker

Uma das principais sugestões deste projeto é utilizar o socket Unix do Docker:

<pre class="overflow-visible! px-0!" data-start="3849" data-end="3893"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

Esse socket permite que aplicações consultem diretamente a API interna do Docker sem necessidade de CLI externa.

---

# Exemplo de montagem do socket no container

<pre class="overflow-visible! px-0!" data-start="4060" data-end="4138"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>volumes:</span><br/><span>  - /var/run/docker.sock:/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div></div></div></div></pre>

g

# Possibilidades Utilizando Docker Socket

Os alunos poderão:

* listar containers ativos;
* obter labels;
* identificar redes;
* descobrir IPs;
* monitorar eventos do Docker;
* detectar start/stop de containers;
* atualizar automaticamente o balanceamento.

---

# Estratégias de Descoberta de Containers

## 1. Polling periódico

Consultar periodicamente:

<pre class="overflow-visible! px-0!" data-start="4965" data-end="5013"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>docker </span><span class="ͼ10">ps</span><br/><span>docker inspect</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 2. Docker Events API (Recomendado)

Monitorar eventos em tempo real:

* container start;
* stop;
* destroy;
* die.

# Sugestão de Estrutura do Projeto

<pre class="overflow-visible! px-0!" data-start="5354" data-end="5531"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>project/</span><br/><span>├── nginx/</span><br/><span>│   ├── nginx.conf.template</span><br/><span>│   └── generated/</span><br/><span>├── discovery/</span><br/><span>├── scripts/</span><br/><span>├── logs/</span><br/><span>├── docker-compose.yml</span><br/><span>├── main.py</span><br/><span>└── README.md</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

# Fluxo de Funcionamento

## 1. Monitorar Docker

A aplicação monitora:

* containers ativos;
* eventos do Docker;
* labels.

---

## 2. Filtrar containers elegíveis

Selecionar:

<pre class="overflow-visible! px-0!" data-start="5716" data-end="5754"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>lb.enable=true</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

---

## 3. Obter informações

Extrair:

* IP;
* porta;
* nome da aplicação;
* status.

---

## 4. Gerar configuração NGINX

Criar:

* upstreams;
* rotas;
* regras.

---

## 5. Recarregar NGINX

Aplicar:

<pre class="overflow-visible! px-0!" data-start="5957" data-end="5996"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute inset-x-4 top-12 bottom-4"><div class="pointer-events-none sticky z-40 shrink-0 z-1!"><div class="sticky bg-token-border-light"></div></div></div><div class="relative"><div class=""><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>nginx </span><span class="ͼ12">-s</span><span> reload</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

# Sugestões de Discussões Técnicas

Os grupos poderão analisar:

* riscos de segurança do docker.sock;
* isolamento entre containers;
* privilégios excessivos;
* alternativas ao socket Docker;
* impacto de reload dinâmico;
* service discovery em larga escala.

---

# Requisitos Opcionais (Bônus)

Os grupos poderão implementar:

* HTTPS automático;
* health check;
* múltiplos upstreams;
* dashboard web;
* métricas Prometheus;
* integração Grafana;
* failover automático;
* sticky session;
* cache reverso;
* autenticação;
* suporte a múltiplos hosts Docker;
* suporte OCI;
* integração com **Kubernetes**.

---

# Modelo de Avaliação


| Critério                         | Peso |
| ----------------------------------- | ------ |
| Auto-discovery funcionando        | 25%  |
| Integração com API Docker       | 20%  |
| Configuração dinâmica do NGINX | 20%  |
| Balanceamento funcionando         | 15%  |
| Organização do código          | 10%  |
| Documentação                    | 5%   |
| Funcionalidades extras            | 5%   |

---

# Forma de Entrega

## Cada grupo deverá entregar:

### 1. Código-fonte

Disponibilizado em repositório Git.

---

### 2. README.md contendo

* arquitetura da solução;
* instruções de instalação;
* labels suportadas;
* explicação do Docker Socket;
* exemplos de execução.

---

### 3. Relatório técnico

Deve conter:

* funcionamento do NGINX;
* integração com Docker API;
* uso do docker.sock;
* estratégias de auto-discovery;
* dificuldades encontradas;
* aprendizados obtidos.

---

### 4. Vídeo demonstrativo

Entre 5 e 15 minutos contendo:

* descoberta automática;
* atualização dinâmica;
* subida/remoção de containers;
* funcionamento do balanceamento.

# Perguntas Sobre o Projeto

1. O que é service discovery?
2. Qual a função de um load balancer?
3. Como o **NGINX** realiza balanceamento?
4. O que são labels no Docker?
5. Como a API do Docker funciona?
6. Qual a função do arquivo:

<pre class="overflow-visible! px-0!" data-start="8061" data-end="8106"><div class="relative w-full mt-4 mb-1"><div class=""><div class="relative"><div class="h-full min-h-0 min-w-0"><div class="h-full min-h-0 min-w-0"><div class="border border-token-border-light border-radius-3xl corner-superellipse/1.1 rounded-3xl"><div class="h-full w-full border-radius-3xl bg-token-bg-elevated-secondary corner-superellipse/1.1 overflow-clip rounded-3xl lxnfua_clipPathFallback"><div class="pointer-events-none absolute end-1.5 top-1 z-2 md:end-2 md:top-1"></div><div class="relative"><div class="pe-11 pt-3"><div class="relative z-0 flex max-w-full"><div id="code-block-viewer" dir="ltr" class="q9tKkq_viewer cm-editor z-10 light:cm-light dark:cm-light flex h-full w-full flex-col items-stretch ͼs ͼ16"><div class="cm-scroller"><pre class="cm-content q9tKkq_readonly m-0"><code><span>/var/run/docker.sock</span></code></pre></div></div></div></div></div></div></div></div></div><div class=""><div class=""></div></div></div></div></div></pre>

7. Quais riscos de segurança existem ao expor o docker.sock para containers?
8. Como o Docker fornece IP para containers?
9. O que é upstream no NGINX?
10. Como evitar downtime durante reload do NGINX?
11. Como ferramentas como **Traefik** utilizam auto-discovery?
12. Como implementar health checks automáticos?
13. Como detectar containers indisponíveis?
14. Qual a diferença entre reverse proxy e ingress controller?
15. Como seria possível expandir o projeto para múltiplos hosts?
16. Qual a relação deste projeto com plataformas como **Kubernetes**?
17. Como o balanceamento impacta escalabilidade e disponibilidade?
18. Como reduzir privilégios ao utilizar Docker Socket?

---

# Sugestões de Expansão do Projeto

Os grupos mais avançados poderão evoluir o projeto para:

* criar um mini ingress controller;
* implementar métricas observabilidade;
* suportar autoscaling;
* integrar com Kubernetes;
* implementar certificados automáticos;
* desenvolver um proxy reverso semelhante ao **Traefik**;
* criar painel administrativo em tempo real;
* suportar ambientes distribuídos.

Isso permitirá aos alunos compreender profundamente como arquiteturas modernas realizam descoberta automática de serviços, balanceamento dinâmico e automação de infraestrutura em ambientes containerizados.
