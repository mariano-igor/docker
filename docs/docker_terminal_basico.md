# **Uso Interativo de Containers com Docker**

Nesta seção, serão explicados os detalhes do comando `docker run -it ubuntu bash`, abordando cada flag utilizada, o comportamento do container e como gerenciar seu ciclo de vida.

---

## **Comando: `docker run -it ubuntu bash`**

### **Sintaxe e Componentes**:
```bash
docker run -it ubuntu bash
```
- **`docker run`**: Cria e inicia um container com base em uma imagem especificada.
- **`-i` (interativo)**: Mantém o **stdin** aberto, permitindo a interação com o container.
- **`-t` (tty)**: Aloca um terminal (pseudo-TTY), permitindo a digitação de comandos.
- **`ubuntu`**: Imagem utilizada para criar o container.
- **`bash`**: Comando a ser executado no container (inicia o shell Bash).

### **Explicação Detalhada**:
1. **`-i` (interativo)**:
   - Mantém o terminal conectado ao container, garantindo que você possa enviar entradas (stdin).
   - Sem essa flag, o terminal seria desconectado automaticamente.

2. **`-t` (tty)**:
   - Permite a digitação de comandos no terminal (aloca um pseudo-terminal).
   - Com isso, o container se comporta como um terminal normal.

3. **Prender o terminal x Liberar o terminal**:
   - **Prender**: O terminal permanece atrelado ao container, permitindo interação em tempo real.
   - **Liberar**: O terminal é desconectado, mas o container continua rodando em segundo plano (depende do processo ativo).

---

## **Executando Comandos no Container**

Quando o container é iniciado com `docker run -it ubuntu bash`, ele entra em um terminal do Ubuntu. Isso permite executar comandos diretamente dentro do container.

**Exemplo**:
```bash
uname -a
```
**Saída**:
```
Linux 123456789abc 5.4.0-42-generic #46-Ubuntu SMP Fri Jul 10 00:24:02 UTC 2020 x86_64 GNU/Linux
```
- Este comando exibe informações sobre o kernel e o sistema operacional do container.

---

## **Status do Container**

O container permanece **em execução** enquanto o processo **`bash`** estiver ativo. Quando você sai do `bash` (com `exit` ou `Ctrl+D`), o processo termina e o container é encerrado.

### **Verificando Containers em Execução**:
```bash
docker ps
```
- Exibe containers que estão em execução.

**Saída**:
```
CONTAINER ID   IMAGE    COMMAND   CREATED        STATUS       PORTS   NAMES
abc123456789   ubuntu   "bash"   10 seconds ago Up 10 secs           ubuntu_shell
```

---

## **Gerenciando o Ciclo de Vida do Container**

1. **`docker stop <nomeContainer>`**:
   - Pausa o container em execução.
   - O container ainda existe e pode ser reiniciado.

   **Exemplo**:
   ```bash
   docker stop ubuntu_shell
   ```

2. **`docker start <nomeContainer>`**:
   - Reinicia um container parado.
   - O container é iniciado a partir do estado em que parou.

   **Exemplo**:
   ```bash
   docker start -ai ubuntu_shell
   ```
   - A flag `-a` reatacha o terminal ao container.

---

## **Importante: Containers São Processos**

A base do funcionamento do Docker é entender que um container é um **processo isolado**. Se o **processo principal** que o mantém ativo for finalizado, o container será encerrado.

**Exemplo**:
- O processo principal nesse caso é o `bash`. Se ele for fechado, o container cai:
   ```bash
   exit
   ```

---

## **Flag `--rm`**

### **Função**:
A flag `--rm` instrui o Docker a **remover o container automaticamente** assim que ele for finalizado.

### **Impacto no `docker ps -a`**:
- Quando `--rm` é utilizado, o container **não aparecerá** na lista de containers parados (`docker ps -a`), pois ele é removido imediatamente após o encerramento.

**Exemplo**:
```bash
docker run --rm -it ubuntu bash
```
- Quando o comando `exit` é executado, o container é removido automaticamente.

**Comportamento Padrão** (sem `--rm`):
- O container permanece na lista do `docker ps -a` até que seja removido manualmente:
   ```bash
   docker rm <nomeContainer>
   ```

---

## **Resumo Rápido**:
- **`-i`**: Mantém o terminal interativo.
- **`-t`**: Aloca um terminal para entrada de comandos.
- **Container = Processo**: Se o processo principal encerrar, o container cai.
- **`docker start`**: Reinicia um container parado.
- **`docker stop`**: Pausa um container em execução.
- **`--rm`**: Remove automaticamente o container após finalização.
- **`docker ps`**: Lista containers em execução.
- **`docker ps -a`**: Lista todos os containers, incluindo parados.

