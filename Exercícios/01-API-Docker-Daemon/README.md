# Exercício: Utilização da API do Docker Daemon para Gerenciamento de Containers

## 1. Introdução

O Docker é uma plataforma amplamente utilizada para criar, implantar e gerenciar containers de forma automatizada. O Docker Daemon fornece uma API RESTful que permite interagir com containers, imagens, volumes e redes por meio de requisições HTTP. Essa API pode ser acessada diretamente pelo socket UNIX do Docker, possibilitando automação e integração com outras ferramentas.

Neste exercício, você aprenderá a interagir com a API do Docker Daemon utilizando o socket `/var/run/docker.sock`, criando e gerenciando containers programaticamente.

---

## 2. Objetivo

O objetivo deste exercício é:

- Compreender como funciona a API do Docker Daemon.
- Aprender a enviar requisições HTTP diretamente ao Docker usando sockets UNIX.
- Criar um container via API sem utilizar o comando `docker run`.
- Aplicar conceitos de automação para gerenciamento de containers Docker.

---

## 3. Desenvolvimento

### **Passo 1: Verificar se o Docker está em execução**

Antes de utilizar a API do Docker, certifique-se de que o Docker Daemon está rodando:

```bash
systemctl status docker
```

Se o serviço não estiver ativo, inicie-o com:

```bash
sudo systemctl start docker
```

### **Passo 2: Testar acesso ao socket do Docker**

A API do Docker pode ser acessada via `/var/run/docker.sock`. Para verificar o acesso, execute:

```bash
ls -l /var/run/docker.sock
```

Se necessário, conceda permissão ao usuário atual:

```bash
sudo chmod 666 /var/run/docker.sock
```

### **Passo 3: Listar os containers usando `curl`**

Podemos testar a API listando os containers ativos:

```bash
curl --unix-socket /var/run/docker.sock http://localhost/containers/json

curl --unix-socket /var/run/docker.sock http://localhost/containers/json | jq '.[].Names'
```

Isso retornará um JSON com a lista de containers em execução.

### **Passo 4: Criar um container usando Python e sockets UNIX**

Agora, crie um arquivo Python chamado `create_container.py` e adicione o seguinte código:

```python
import socket
import json

# Criar um socket UNIX
sock = socket.socket(socket.AF_UNIX, socket.SOCK_STREAM)
sock.connect("/var/run/docker.sock")

# Dados da requisição
request_data = {
    "Image": "nginx",
    "Cmd": ["echo", "Hello, Docker!"],
    "Tty": True
}
request_json = json.dumps(request_data)

# Construir a requisição HTTP
request = f"POST /containers/create HTTP/1.1\r\n"
request += "Host: localhost\r\n"
request += "Content-Type: application/json\r\n"
request += f"Content-Length: {len(request_json)}\r\n"
request += "Connection: close\r\n\r\n"
request += request_json

# Enviar a requisição pelo socket
sock.sendall(request.encode())

# Receber a resposta
data = sock.recv(4096)
sock.close()

# Exibir a resposta do Docker Daemon
print(data.decode())
```

### **Passo 5: Executar o script**

Salve o arquivo e execute:

```bash
python3 create_container.py
```

Se o comando for bem-sucedido, você verá a resposta do Docker com o ID do container criado.

### **Passo 6: Listar os containers novamente**

Confira se o container foi criado executando:

```bash
curl --unix-socket /var/run/docker.sock http://localhost/containers/json | jq
```

(Use `jq` para formatar a saída JSON.)

### **Passo 7: Iniciar e parar um container via API**

Para iniciar o container criado:

```bash
curl --unix-socket /var/run/docker.sock -X POST http://localhost/containers/<CONTAINER_ID>/start
```

Para parar o container:

```bash
curl --unix-socket /var/run/docker.sock -X POST http://localhost/containers/<CONTAINER_ID>/stop
```

Substitua `<CONTAINER_ID>` pelo ID retornado no passo anterior.

---

## 4. Principais Opções da API do Docker Daemon

A API do Docker Daemon oferece diversas opções para gerenciamento de containers, imagens, volumes e redes. Algumas das principais incluem:

### **Containers**
- Listar containers: `GET /containers/json`
- Criar um container: `POST /containers/create`
- Iniciar um container: `POST /containers/{id}/start`
- Parar um container: `POST /containers/{id}/stop`
- Remover um container: `DELETE /containers/{id}`
- Obter logs do container: `GET /containers/{id}/logs`

### **Imagens**
- Listar imagens: `GET /images/json`
- Criar uma imagem (pull): `POST /images/create`
- Remover uma imagem: `DELETE /images/{name}`

### **Volumes**
- Listar volumes: `GET /volumes`
- Criar um volume: `POST /volumes/create`
- Remover um volume: `DELETE /volumes/{name}`

### **Redes**
- Listar redes: `GET /networks`
- Criar uma rede: `POST /networks/create`
- Remover uma rede: `DELETE /networks/{id}`

Estas opções permitem o controle total do ambiente Docker via API, possibilitando a automação e integração com outras ferramentas.

---

## 5. Conclusão

Neste exercício, você aprendeu a interagir diretamente com a API do Docker Daemon utilizando sockets UNIX. Além disso, conheceu as principais opções disponíveis na API para o gerenciamento de containers, imagens, volumes e redes. Essa abordagem permite um controle mais refinado sobre containers e facilita a automação de processos sem depender diretamente da CLI do Docker.

Agora, você pode explorar outras operações da API, como remoção de containers, gerenciamento de volumes e redes, tornando seu ambiente Docker ainda mais dinâmico e flexível! 🚀

