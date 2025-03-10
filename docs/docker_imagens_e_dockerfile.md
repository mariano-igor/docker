## 🔹 O que é o Docker Hub?
O **Docker Hub** é um repositório público onde estão armazenadas todas as imagens que podem ser utilizadas no Docker. Ele permite que os usuários baixem, compartilhem e gerenciem imagens de containers.

## 🔹 O Docker como Container Registry
O **Docker** é, por si só, um Container Registry, mas você pode usar registries privados, como os oferecidos pela **AWS (Amazon ECR), Azure (ACR), Google Cloud (GCR) ou Digital Ocean**.

## 🔹 Como listar as imagens disponíveis localmente
Para visualizar todas as imagens que você tem no computador:
```sh
docker images
```
Isso listará todas as imagens armazenadas localmente.

## 🔹 Relação entre Containers e Imagens
Tudo o que você executa no Docker é baseado em **imagens**. Essas imagens são obtidas de um **container registry** e baixadas para o seu computador quando um container é iniciado.

## 🔹 Como remover uma imagem
Se precisar remover uma imagem específica:
```sh
docker rmi nomeImagem
```
Isso apagará a imagem do seu sistema local.

---

## 📝 O que é um Dockerfile?
Um **Dockerfile** é um arquivo de texto que contém instruções para construir uma imagem Docker personalizada. Ele define o ambiente, dependências e comandos que serão executados no container.

### Exemplo de Dockerfile básico:
```Dockerfile
FROM ubuntu:latest
WORKDIR /app
COPY . .
CMD ["echo", "Hello, Docker!"]
```
Neste exemplo:
- `FROM ubuntu:latest` → Usa a imagem do Ubuntu como base.
- `WORKDIR /app` → Define o diretório de trabalho dentro do container.
- `COPY . .` → Copia todos os arquivos locais para o diretório do container.
- `CMD ["echo", "Hello, Docker!"]` → Executa um comando padrão quando o container é iniciado.

---

## 🔹 Comando para construir uma imagem
```sh
docker build -t imariano/nginx-com-vim:latest .
```
### Explicação passo a passo:
- `docker build` → Comando para construir uma imagem Docker.
- `-t imariano/nginx-com-vim:latest` → Define um nome e tag para a imagem (`latest` significa que é a versão mais recente).
- `.` → Indica que o Dockerfile está no diretório atual.

---

## 🔹 O que é `WORKDIR` no Dockerfile?
O `WORKDIR` define o diretório de trabalho dentro do container. Qualquer comando executado dentro do container será realizado a partir desse diretório.

Exemplo:
```Dockerfile
WORKDIR /usr/src/app
```
Agora, qualquer comando no container será executado dentro de `/usr/src/app`.

---

## 🏗️ Como funcionam as Layers no Docker?
O Docker constrói imagens em **camadas (layers)**. Cada comando no Dockerfile cria uma nova camada.

- Se você modificar apenas uma parte do Dockerfile, o Docker **reutiliza as layers já existentes** e só baixa novamente a parte alterada.
- Isso melhora a performance e reduz o tempo de build.

Exemplo de otimização com cache:
```Dockerfile
FROM node:16
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
CMD ["node", "index.js"]
```
Se você só modificar os arquivos copiados depois de `COPY package.json .`, o `npm install` não será reexecutado, pois o Docker usará o cache das camadas anteriores.

---

## 🔹 Como remover todos os containers?
Se quiser remover **todos os containers criados**, use:
```sh
docker rm $(docker ps -a -q) -f
```
Isso removerá **todos os containers**, independentemente de estarem em execução ou parados.

---

## 🔹 `CMD` vs `ENTRYPOINT`
O `CMD` pode ser substituído por parâmetros ao rodar o container, enquanto o `ENTRYPOINT` não.

### Exemplo:
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT [ "echo", "Hello" ]
CMD [ "World" ]
```
Se rodarmos:
```sh
docker run nome-da-imagem
```
A saída será:
```sh
Hello World
```
Mas se rodarmos:
```sh
docker run nome-da-imagem Docker!
```
A saída será:
```sh
Hello Docker!
```
O `CMD` foi substituído pelo argumento `Docker!`, enquanto o `ENTRYPOINT` se manteve fixo.

---

## 🚀 Conclusão
Este documento cobriu conceitos essenciais sobre o Docker, como:
- **Docker Hub e registries privados**
- **Imagens, containers e como gerenciá-los**
- **Dockerfile, layers e cache**
- **Comandos importantes (`docker build`, `docker images`, `docker rmi`, etc.)**
- **Diferença entre `ENTRYPOINT` e `CMD`**

