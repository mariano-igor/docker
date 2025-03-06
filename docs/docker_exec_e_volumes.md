# Executando Comandos e Gerenciando Volumes no Docker

O comando `docker exec` permite executar comandos dentro de um container que já está em execução.

## Exemplo:
```sh
docker exec -it nginx bash
```
Esse comando permite entrar no **bash** do container `nginx` e interagir com ele diretamente.

Dentro do container, o diretório onde está o HTML da mensagem padrão exibida na porta 80 do Nginx é:
```sh
cd /usr/share/nginx/html/
```

### Instalando o Vim dentro do Container
Se você tentar rodar `vim` dentro do container, pode se deparar com o seguinte erro:
```sh
bash: vim: command not found
```
Isso acontece porque o **Vim** não vem instalado por padrão na imagem do Nginx. Como essa imagem é baseada no Ubuntu, podemos instalar com:
```sh
apt-get install vim
```

No entanto, antes disso, precisamos rodar:
```sh
apt-get update
```
Isso é necessário porque a imagem do Nginx remove o cache do `apt-get` para manter a imagem leve. O `update` baixa as listas de pacotes disponíveis para instalação.

Agora, podemos abrir e editar o arquivo dentro do container:
```sh
vim index.html
```

### Usando o Vim
- O **Vim** inicia em modo de leitura. Para começar a editar, pressione `i` (modo Insert).
- Para salvar as alterações:
  1. Pressione `ESC` para sair do modo Insert.
  2. Digite `:w` e pressione `Enter`.

## Persistência de Dados no Docker

Containers são **imutáveis** por padrão. As alterações feitas dentro de um container **desaparecem** quando ele é removido. Para persistir mudanças, precisamos usar **Bind Mounts** ou **Volumes**.

### Usando Bind Mounts
O Bind Mount permite mapear um diretório do seu **host** (máquina local) para dentro do container.

#### Exemplo com `-v` (antigo)
```sh
docker run -d --name nginx -p 8080:80 -v ~/Projects/fullcycle2/docker/html/:/usr/share/nginx/html nginx
```

#### Exemplo com `--mount` (recomendado)
```sh
docker run -d --name nginx -p 8080:80 \
  --mount type=bind,source="${PWD}"/html,target=/usr/share/nginx/html nginx
```
**Diferença entre `-v` e `--mount`**:
- O `-v` **cria automaticamente** a pasta caso ela não exista.
- O `--mount` **gera erro** se a pasta do `source` não existir:
  ```sh
  Error response from daemon: invalid mount config...
  ```

### Usando Volumes
Volumes são uma solução mais avançada e gerenciada pelo próprio Docker.

#### Criando um Volume:
```sh
docker volume create meuvolume
```

#### Inspecionando um Volume:
```sh
docker volume inspect meuvolume
```

#### Usando Volumes em Containers:
```sh
docker run --name nginx -d --mount type=volume,source=meuvolume,target=/app nginx
```
Esse comando monta o volume `meuvolume` dentro do container, permitindo que múltiplos containers compartilhem esse volume.

Outra forma de escrever esse comando usando `-v`:
```sh
docker run --name nginx3 -d -v meuvolume:/app nginx
```

### Limpando Volumes Não Utilizados
Com o tempo, os volumes podem acumular espaço desnecessário. Para remover volumes que não estão sendo usados por nenhum container:
```sh
docker volume prune
```
Esse comando remove **todos os volumes não utilizados**, liberando espaço em disco.

---
Essa documentação serve como um guia rápido para manipulação de arquivos dentro de containers e gerenciamento de volumes no Docker. 🚀

