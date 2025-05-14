# ✅ Checklist Mínimo de Build Seguro com Docker

Este documento serve como uma checklist de boas práticas para construir imagens Docker seguras, enxutas e funcionais, com foco em projetos como Laravel, Node.js, Python, entre outros.

---

## 📆 1. Escolha da Imagem Base

* [ ] Usar imagem **oficial e atualizada**
* [ ] Usar imagem base **leve**, como `alpine`, se apropriado
* [ ] Verificar qual é o **usuário padrão** (ex: `www-data`, `node`, `nginx`, `root`)

**Exemplo:**

```dockerfile
FROM php:8.2-fpm-alpine AS base
```

---

## 🛠️ 2. Estágio de Build (Builder)

* [ ] Separar build em estágios (`AS builder`)
* [ ] Instalar dependências com limpeza de cache

**Exemplo:**

```dockerfile
FROM php:8.2-cli AS builder
RUN apk add --no-cache libzip-dev \
    && docker-php-ext-install zip
```

---

## 📂 3. Instalação ou Geração do Projeto

* [ ] Clonar ou gerar projeto com Composer, npm, etc

**Exemplo:**

```dockerfile
RUN php composer.phar create-project laravel/laravel app
```

---

## 🚚 4. Estágio Final (Imagem Prod)

* [ ] Usar imagem final enxuta
* [ ] Definir `WORKDIR` e limpar pastas desnecessárias

**Exemplo:**

```dockerfile
FROM php:8.2-fpm-alpine
WORKDIR /var/www
RUN rm -rf /var/www/html
```

---

## 👤 5. Permissões e Usuário

* [ ] Copiar arquivos com permissão correta
* [ ] Ajustar dono com `chown`
* [ ] (Opcional) Rodar com usuário não root (`USER`)

**Exemplo:**

```dockerfile
COPY --from=builder /var/www/app .
RUN chown -R www-data:www-data /var/www
USER www-data
```

---

## 📡 6. Exposição e Comando Final

* [ ] `EXPOSE` apenas do que for necessário
* [ ] Definir `CMD` claro e direto

**Exemplo:**

```dockerfile
EXPOSE 9000
CMD ["php-fpm"]
```

---

## 🔎 Verificações Pós-Build

```bash
# Verificar processos
docker exec -it meu_container ps aux

# Verificar donos dos arquivos
docker exec -it meu_container ls -l /var/www

# Testar escrita
docker exec -it meu_container touch /var/www/storage/test.log
```

---

> Mantenha este checklist como referência sempre que iniciar um novo projeto com Docker. Adapte conforme a linguagem ou framework.
