# **Expondo Portas e Trabalhando com Nginx no Docker**

Nesta seção, será abordado o uso de containers que expõem portas, como o **Nginx**, e como configurar o acesso ao container a partir do host. Além disso, veremos o uso do **`-p`** para publicação de portas e o **`-d`** para desanexar o terminal.

---

## **Comando: `docker run nginx`**

### **O que acontece?**
- O comando `docker run nginx` cria e inicia um container baseado na imagem **Nginx**.
- O Nginx é um **servidor web** amplamente utilizado para hospedar sites, balancear carga e atuar como proxy reverso.

### **Porta Exposta**:
- A imagem **Nginx** expõe a **porta 80** dentro do container. Isso significa que o processo do Nginx está ativo e ouvindo na porta 80 **dentro do container**.
- No entanto, isso não significa que a porta 80 do container está acessível diretamente pelo **host**.

### **Por que não consigo acessar pelo `localhost:80`?**
- A porta 80 está ativa **dentro do container**, mas ela **não está mapeada** para a porta do host.
- O host (sua máquina) e o container são ambientes isolados. Uma porta ativa no container não é automaticamente acessível pelo host.

### **Container x Container**:
- Outros containers na mesma rede do Docker conseguem acessar a porta 80 do container Nginx diretamente.
- Por exemplo, usando a rede padrão do Docker, outro container poderia acessar o Nginx pelo **IP interno** do container e porta 80.

---

## **Como acessar a porta do Nginx?**

Para tornar a porta do container acessível no host, utilizamos o mapeamento de portas com o argumento **`-p`**.

### **Comando: `docker run -p 8080:80 nginx`**

### **Sintaxe e Significado**:
```bash
docker run -p 8080:80 nginx
```
- **`-p 8080:80`**:
  - Mapeia a porta **80** do container para a porta **8080** do host.
  - A sintaxe é: `hostPort:containerPort`.
- **`nginx`**: A imagem utilizada.

### **Comportamento**:
- A partir desse comando, é possível acessar o servidor Nginx em `http://localhost:8080`.
- A porta 80 dentro do container é redirecionada para a porta 8080 no host.

### **Exemplo**:
```bash
docker run -p 8080:80 nginx
```
**Saída (parcial):**
```
/docker-entrypoint.sh: Configuration complete; ready for start up.
```
- Agora, o Nginx está acessível no host em `http://localhost:8080`.

---

## **Executando em Background com `-d`**

### **O que é `-d`?**
A flag **`-d`** significa **detach** (desanexar). Ela permite que o container rode em segundo plano, liberando o terminal.

### **Sintaxe**:
```bash
docker run -d -p 8080:80 nginx
```

### **Comportamento**:
- O container é iniciado e continua rodando em background.
- O terminal é liberado imediatamente, e você pode continuar usando o terminal para outros comandos.

**Exemplo**:
```bash
docker run -d -p 8080:80 nginx
```
**Saída:**
```
abc1234567890abcdef
```
- O ID do container é exibido.
- O Nginx continua rodando em segundo plano.

### **Verificando o Container**:
- Use `docker ps` para listar containers em execução:
  ```bash
  docker ps
  ```
  **Saída:**
  ```
  CONTAINER ID   IMAGE    COMMAND                  STATUS        PORTS                  NAMES
  abc123456789   nginx    "/docker-entrypoint.s..." Up 10 secs    0.0.0.0:8080->80/tcp   awesome_nginx
  ```

---

## **Resumo Rápido**:
- **`docker run nginx`**: Inicia um container com Nginx, expondo a porta 80 **dentro do container**.
- **Mapear portas**: Use `-p hostPort:containerPort` para tornar portas do container acessíveis no host.
  - Exemplo: `docker run -p 8080:80 nginx`.
- **Rodar em background**: Use `-d` para executar o container em segundo plano.
- **`docker ps`**: Lista containers em execução.
- **Isolamento**: A porta exposta no container não é acessível no host até ser mapeada com `-p`.

Com isso, você consegue iniciar containers, expor portas e executar processos em background no Docker.

