# **Introdução aos Comandos Básicos do Docker**

Neste documento, serão apresentados os principais comandos para iniciar e visualizar containers no Docker. Esses comandos são essenciais para quem está começando a utilizar essa tecnologia.

---

## **Comando: `docker ps`**

### **Função**:
O comando `docker ps` é utilizado para listar os containers **em execução** no momento.

### **Sintaxe**:
```bash
docker ps
```

### **Saída do Comando**:
Ao executar `docker ps`, é exibida uma tabela com as seguintes informações:
- **CONTAINER ID**: Identificador único do container.
- **IMAGE**: Nome da imagem usada pelo container.
- **COMMAND**: O comando inicial (entrypoint) executado no container.
- **CREATED**: Tempo desde a criação do container.
- **STATUS**: Estado atual do container (em execução, parado, etc.).
- **PORTS**: Portas expostas pelo container.
- **NAMES**: Nome atribuído ao container.

### **Exemplo**:
```bash
docker ps
```
**Saída:**
```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS          PORTS          NAMES
abc123456789   nginx:latest  "nginx -g..." 2 minutes ago    Up 2 minutes    80/tcp         nginx_server
```

---

## **Comando: `docker run hello-world`**

### **Função**:
O comando `docker run` cria um container a partir de uma imagem especificada e o executa.

- O `hello-world` é uma imagem simples usada como exemplo inicial.
- Se a imagem não existir localmente, o Docker irá procurá-la no **Docker Hub** e fará o download ("pull").

### **Importante**:
Se **nenhuma versão** da imagem for especificada, o Docker utilizará automaticamente a versão **latest** (a última versão disponível no repositório).

### **Exemplo**:
```bash
docker run hello-world
```

### **Processo Executado**:
1. O Docker verifica se a imagem `hello-world` existe **localmente**.
2. Se não existir, faz o **download** (pull) da imagem do Docker Hub.
3. Cria um container com base nessa imagem.
4. Executa o processo definido no **ENTRYPOINT** ou **COMMAND** da imagem.
5. O container finaliza após executar o comando (não há um processo que mantenha o container ativo).

### **Saída do Exemplo**:
```bash
Hello from Docker!
This message shows that your installation appears to be working correctly.
...
```

---

## **Explicação sobre o Digest**

### **O que é um Digest?**
Quando baixamos uma imagem Docker, ela é identificada de forma única por um **Digest**.

- O Digest é um **hash SHA256** que garante a integridade da imagem.
- Isso significa que duas imagens com o mesmo Digest são idênticas, independentemente do nome ou tag.

### **Visualização**:
Ao baixar uma imagem, o Docker pode exibir o Digest:
```bash
docker pull hello-world
```
Saída:
```
Digest: sha256:4f0...3e6
```
O Digest assegura que você tenha a mesma imagem, mesmo que ela seja renomeada.

---

## **Como o Docker Executa Imagens**

Uma imagem Docker pode conter tanto um **ENTRYPOINT** quanto um **COMMAND**, cada um com propósitos distintos na definição do processo inicial a ser executado ao criar um container.

### **Diferenças entre ENTRYPOINT e COMMAND**:

1. **ENTRYPOINT**:
   - Define o executável principal (ponto de entrada) do container.
   - Não pode ser facilmente sobrescrito ao executar o `docker run`, exceto se usado com a flag `--entrypoint`.
   - Geralmente utilizado para transformar a imagem em uma aplicação mais autônoma e configurável.

   **Exemplo**:
   ```Dockerfile
   FROM ubuntu:latest
   ENTRYPOINT ["/bin/echo"]
   CMD ["Default Message"]
   ```
   - Ao executar:
     ```bash
     docker run my-image "Hello, ENTRYPOINT!"
     ```
     **Saída:**
     ```
     Hello, ENTRYPOINT!
     ```
     O `ENTRYPOINT` é fixo e não muda, mas aceita argumentos adicionais.

2. **COMMAND (CMD)**:
   - Define o comando padrão a ser executado pelo container, mas pode ser sobrescrito diretamente com argumentos no `docker run`.
   - Se usado em conjunto com o **ENTRYPOINT**, o `CMD` fornece argumentos padrão para o executável definido no `ENTRYPOINT`.

   **Exemplo**:
   ```Dockerfile
   FROM ubuntu:latest
   CMD ["/bin/echo", "Default Message"]
   ```
   - Ao executar:
     ```bash
     docker run my-image "Hello, CMD!"
     ```
     **Saída:**
     ```
     Hello, CMD!
     ```
     O `CMD` é facilmente sobrescrito.

### **Resumo**:
- **ENTRYPOINT**: Define o executável principal do container e é menos flexível.
- **COMMAND (CMD)**: Define o comando padrão, mas pode ser sobrescrito facilmente.
- **Uso conjunto**: O ENTRYPOINT é ideal quando queremos definir um executável principal que sempre será executado, enquanto o CMD pode ser usado para fornecer valores padrão ou argumentos adicionais para o ENTRYPOINT.

### **Quando usar ENTRYPOINT**:
- Quando a imagem representa um serviço ou aplicação principal.
- Exemplo: Um servidor web onde o executável principal (como `nginx` ou `httpd`) deve ser sempre iniciado.
  ```Dockerfile
  FROM nginx:latest
  ENTRYPOINT ["nginx"]
  CMD ["-g", "daemon off;"]
  ```
  Aqui, o ENTRYPOINT é fixo, mas argumentos extras podem ser passados via CMD.

### **Quando usar COMMAND (CMD)**:
- Quando o comportamento padrão pode ser facilmente alterado.
- Exemplo: Usar uma imagem base onde você executa comandos variados:
  ```Dockerfile
  FROM ubuntu:latest
  CMD ["bash"]
  ```
  Neste caso, ao rodar `docker run my-image ls`, o CMD é sobrescrito pelo `ls`.

### **Uso conjunto**:
- Defina ENTRYPOINT para o executável principal.
- Use CMD para definir argumentos padrão ou valores adicionais, permitindo maior flexibilidade.

- O **ENTRYPOINT** é o ponto de entrada principal.
- O **COMMAND** pode ser sobrescrito quando rodamos o container.

**Exemplo**:
```Dockerfile
FROM ubuntu:latest
ENTRYPOINT ["echo"]
CMD ["Hello, World!"]
```
Ao rodar essa imagem:
```bash
docker run meu-ubuntu
```
**Saída:**
```
Hello, World!
```
O processo finaliza após a execução do comando, pois não há nada que mantenha o container ativo.

---

## **Comando: `docker ps -a`**

### **Função**:
O comando `docker ps -a` lista **todos os containers**, incluindo aqueles que estão **parados**.

### **Sintaxe**:
```bash
docker ps -a
```

### **Diferença entre `docker ps` e `docker ps -a`**:
- **`docker ps`**: Mostra apenas os containers em execução.
- **`docker ps -a`**: Mostra todos os containers, incluindo parados e finalizados.

### **Saída do Comando**:
```bash
CONTAINER ID   IMAGE         COMMAND      CREATED         STATUS                     PORTS          NAMES
abc123456789   hello-world   "/hello"     5 minutes ago   Exited (0) 5 minutes ago                 peaceful_nobel
xyz987654321   nginx:latest  "nginx -g..." 1 hour ago      Up 1 hour                  80/tcp         nginx_server
```

### **Campos da Saída**:
- **STATUS**: Indica o estado do container:
  - **Exited**: O container finalizou com sucesso (exit code 0).
  - **Up**: O container está em execução.
  - Outros status (e.g., erro, reiniciado).
- **NAMES**: O Docker atribui um nome aleatório ao container se não for especificado.

### **Exemplo Prático**:
1. Execute um container `hello-world`:
   ```bash
docker run hello-world
   ```
2. Liste os containers (incluindo finalizados):
   ```bash
docker ps -a
   ```
   **Saída:**
   ```
CONTAINER ID   IMAGE         COMMAND      CREATED          STATUS                     PORTS          NAMES
abc123456789   hello-world   "/hello"     2 minutes ago    Exited (0) 2 minutes ago                 quirky_pasteur
   ```
---

## **Resumo Rápido**:
- **`docker ps`**: Lista apenas os containers em execução.
- **`docker run hello-world`**: Executa uma imagem Docker. Se a imagem não for encontrada localmente, é baixada do Docker Hub.
- **Imagens `latest`**: Quando nenhuma tag é especificada, o Docker assume a versão mais recente.
- **`docker ps -a`**: Lista todos os containers, inclusive os parados.
- **Digest**: Identificador único que garante a integridade de uma imagem.
- **ENTRYPOINT**: Processo inicial executado ao iniciar um container.

---

Com esses conceitos, já é possível iniciar containers, verificar o status e entender como o Docker executa imagens.
