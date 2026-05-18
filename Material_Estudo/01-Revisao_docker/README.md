# 🐳 Exercício Dirigido – Prática com Docker

## Objetivo Geral
Praticar os principais recursos do Docker, desde comandos básicos até a criação de imagens, troubleshooting, uso de volumes, redes e interação entre containers.

---

## Parte 1 – Comandos básicos do Docker

**Verifique a versão do Docker instalada:**
```bash
docker --version
docker info
```

**Liste as imagens disponíveis:**
```bash
docker images
```

**Liste os containers:**
```bash
docker ps
docker ps -a
```

**Baixe e execute a imagem hello-world:**
```bash
docker run hello-world
```

---

## Parte 2 – Criando e executando containers

**Execute o nginx:**
```bash
docker run -d --name meu-nginx -p 8080:80 nginx
```

**Entre no container:**
```bash
docker exec -it meu-nginx bash
ls /usr/share/nginx/html
```

---

## Parte 3 – Criando sua própria imagem

**Exemplo de Dockerfile:**
```dockerfile
FROM debian:latest
RUN apt-get update && apt-get install -y curl
CMD ["echo", "Minha primeira imagem Docker personalizada!"]
```

**Construa e execute:**
```bash
docker build -t minha-imagem:1.0 .
docker run minha-imagem:1.0
```

---

## Parte 4 – Troubleshooting de containers

**Dockerfile com erro:**
```dockerfile
FROM ubuntu:latest
CMD ["python3", "app.py"]
```

**Analise os erros:**
```bash
docker ps -a
docker logs <id>
docker inspect <id>
```

---

## Parte 5 – Volumes efêmeros e compartilhados

**Execute containers com volume:**
```bash
docker run -it --name c1 -v meu-volume:/dados alpine sh
docker run -it --name c2 -v meu-volume:/dados alpine sh
```

**Teste compartilhamento:**
```bash
echo "Olá Docker" > /dados/arquivo.txt
cat /dados/arquivo.txt
```

---

## Parte 6 – Redes e comunicação entre containers

**Crie rede e containers:**
```bash
docker network create minha-rede
docker run -d --name web --network minha-rede nginx
docker run -it --name client --network minha-rede alpine sh
```

**No cliente:**
```bash
apk add curl
curl http://web
```

---

## Parte 7 – Desafio final

Monte um mini-ambiente com:
- nginx servindo página customizada com volume persistente
- alpine acessando a página via rede customizada

---

## Resultado Esperado
O aluno terá praticado: comandos básicos, criação de imagens, troubleshooting, volumes, redes e montagem de um mini-ambiente multi-container.
