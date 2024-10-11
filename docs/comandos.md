## Comandos Básicos do Docker

### `docker ps`

Exibe todos os containers que estão rodando na máquina no momento. É útil para visualizar quais containers estão em execução, suas IDs, imagens utilizadas, status, portas mapeadas, entre outras informações importantes.

```bash
docker ps
```
- **Exemplo de Saída**:
  ```
  CONTAINER ID   IMAGE          COMMAND             CREATED          STATUS          PORTS                    NAMES
  a1b2c3d4e5f6   ubuntu:latest  "bash"              10 minutes ago   Up 9 minutes                              my_ubuntu
  ```

### `docker ps -a`

Exibe todos os containers que existem na máquina, incluindo os que estão parados. Isso permite visualizar o histórico de containers criados.

```bash
docker ps -a
```
- **Exemplo de Saída**:
  ```
  CONTAINER ID   IMAGE          COMMAND             CREATED          STATUS                      PORTS       NAMES
  a1b2c3d4e5f6   ubuntu:latest  "bash"              20 minutes ago   Exited (0) 5 minutes ago                my_ubuntu
  b2c3d4e5f6g7   nginx:alpine   "nginx -g 'daemon…" 3 hours ago      Up 3 hours                  80/tcp      my_nginx
  ```

## Trabalhando com Containers

### `docker run -it ubuntu bash`

Este comando cria e executa um container baseado na imagem `ubuntu`, abrindo um terminal interativo com o comando `bash`. Vamos entender cada parte do comando:

- **`docker run`**: Cria e inicia um novo container.
- **`-it`**: O flag `-i` significa **interativo**, e o `-t` significa **tty** (alocar um pseudo-terminal). Esses flags permitem que você interaja com o container através do terminal.
- **`ubuntu`**: Indica a imagem a ser utilizada.
- **`bash`**: Indica o comando a ser executado quando o container iniciar. Neste caso, o terminal `bash` do Ubuntu.

```bash
docker run -it ubuntu bash
```
- Ao executar esse comando, você terá acesso a um shell interativo dentro do container, como se fosse um terminal do Ubuntu.

### `docker start nome_container`

Este comando inicia um container que já foi criado, mas está parado. Ele é útil quando você deseja reiniciar um container que foi interrompido, sem a necessidade de criar um novo.

```bash
docker start my_ubuntu
```
- **`my_ubuntu`** é o nome do container que você deseja iniciar novamente. Note que, ao contrário do `docker run`, o comando `start` não cria um novo container.

### `docker stop nome_container`

Este comando para um container em execução de maneira segura, permitindo que todos os processos sejam finalizados corretamente.

```bash
docker stop my_ubuntu
```
- **`my_ubuntu`** é o nome do container que você deseja parar.

### `docker run -it --rm ubuntu bash`

Este comando é semelhante ao comando `docker run -it ubuntu bash`, mas com um adicional importante: o flag `--rm`.

- **`--rm`**: Quando um container é iniciado com essa flag, ele é automaticamente removido ao ser parado. Isso é útil para containers temporários que não precisam ser mantidos após o uso.

```bash
docker run -it --rm ubuntu bash
```
- Quando você sair do terminal do container (`exit`), o container será removido automaticamente.

## Outras Considerações

### Versão das Imagens: `latest` e Digest

Ao utilizar o Docker, se não especificarmos a versão da imagem que queremos utilizar, o Docker assume a última versão estável, identificada como `latest`.
- **`latest`**: É a tag padrão utilizada pelo Docker para indicar a versão mais recente de uma imagem. No entanto, é importante lembrar que nem sempre isso garante estabilidade, pois a imagem mais recente pode conter mudanças que afetem a compatibilidade.
- **Digest**: O **digest** é um identificador criptográfico da imagem (geralmente um hash SHA256) que garante que a imagem utilizada seja exatamente a mesma, sem variações. Diferente da tag `latest`, o digest permite que a versão da imagem seja imutável, garantindo consistência entre ambientes.

### Entrypoint e Command

- **ENTRYPOINT** e **COMMAND** são instruções usadas em Dockerfiles para definir qual programa ou script será executado quando o container iniciar.
  - **ENTRYPOINT**: Define o executável principal do container, ou seja, é o ponto de entrada que sempre será executado. Este comando geralmente é fixo.
  - **COMMAND**: Especifica argumentos que serão passados para o executável definido no ENTRYPOINT ou define o comando padrão, caso não tenha um ENTRYPOINT.

  **Exemplo**:
  ```dockerfile
  FROM ubuntu:latest
  ENTRYPOINT ["echo"]
  CMD ["Hello, Docker!"]
  ```
  - Neste exemplo, `ENTRYPOINT` sempre executará `echo`, e o `CMD` define o argumento padrão, que é `"Hello, Docker!"`. Dessa forma, ao rodar um container dessa imagem, ele imprimirá `"Hello, Docker!"`.

## Conclusão

Esta documentação apresenta comandos essenciais para o uso do Docker no dia a dia, como visualizar containers, criar novos containers interativos, iniciar e parar containers, além de alguns conceitos como **latest**, **digest**, **ENTRYPOINT** e **COMMAND**. Com estes comandos e conceitos, é possível gerenciar containers de forma eficiente e compreender melhor como o Docker funciona. Recomenda-se explorar cada comando em um ambiente de aprendizado para se familiarizar mais com o Docker.

