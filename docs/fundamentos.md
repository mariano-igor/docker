## O que são Containers?

Containers são uma unidade padrão de software que empacota o código e todas as dependências necessárias para rodar uma aplicação de forma consistente em diferentes ambientes. Isso garante que a aplicação possa ser executada rapidamente e de maneira confiável, desde o ambiente de desenvolvimento até a produção.

## Como Funcionam os Containers?

Para entender o funcionamento dos containers, precisamos discutir alguns conceitos do sistema operacional (S.O.). Um S.O. tem diversos processos e cada tarefa é um processo rodando. Containers utilizam alguns recursos do S.O. para isolar esses processos, garantindo que possam ser executados de forma independente. Esses recursos são principalmente os **Namespaces**, **Cgroups** e **OFS (Overlay File System)**.

### Namespaces

**Namespaces** são responsáveis por isolar os processos. Quando temos um processo pai com um namespace X, todos os processos filhos iniciam com o mesmo namespace do pai, formando um grupo isolado. Essa mudança na forma de trabalhar com processos é um dos principais fundamentos dos containers. Cada container tem seu próprio conjunto de processos, onde um processo é o pai e os demais são seus filhos. Se o processo pai for finalizado, o container é destruído.

Namespaces em containers controlam diferentes aspectos, como:
- **PID**: Isola os processos e seus IDs.
- **User**: Isola usuários dentro do container.
- **Network**: Isola as configurações de rede.
- **Filesystem**: Isola o sistema de arquivos.

### Cgroups

**Cgroups** (Control Groups) são usados para controlar os recursos computacionais disponíveis para um container, como CPU, memória, I/O, etc. Não basta apenas isolar processos; é importante também isolar o uso de recursos para que um container não interfira no desempenho de outros processos ou containers na mesma máquina.

Exemplo de Cgroups:
- **memory**: Limitar o uso de memória do container, por exemplo, `500MB`.
- **cpu_shares**: Controlar a quantidade de CPU disponível, como `512` unidades de processamento.

### Overlay File System (OFS)

O **OFS (Overlay File System)** é um sistema de arquivos em camadas. Isso significa que as alterações em um container são salvas como camadas adicionais, sem a necessidade de replicar toda a base. Por exemplo, se temos um container com:
- **MyApp:v1**: 70MB
- **Dep 1**: 200MB
- **Dep 2**: 250MB

Se precisarmos alterar o **MyApp**, a nova imagem precisa conter apenas **MyApp:v2**. As demais dependências são mantidas, sem precisar duplicá-las. O OFS trabalha com camadas (layers), guardando apenas as diferenças entre elas. Esse conceito de camadas é fundamental para que os containers sejam leves, uma vez que apenas as partes necessárias são copiadas ou modificadas.

Diferente da virtualização, onde cada máquina virtual precisa de um S.O. completo, os containers compartilham o kernel do S.O. do host, utilizando apenas as bibliotecas necessárias. Isso os torna muito mais leves e rápidos.

## Imagens Docker

**Imagens** são a base dos containers. As pessoas costumam associar imagens a snapshots, ou seja, uma captura de como o ambiente está em um determinado momento. No entanto, as imagens Docker são construídas a partir de camadas reutilizáveis.

Uma imagem inicial pode ser uma **scratch**, que é uma imagem vazia, como um disco formatado. Ao construir uma imagem, somente as camadas que não estão disponíveis no S.O. são baixadas. Por exemplo, ao utilizar uma imagem **Ubuntu**, o sistema terá apenas as camadas que não estão no host, como **bash** ou **sshd**.

As imagens são constituídas por camadas de dependências, formando uma espécie de **árvore de dependências**. Se criarmos uma nova imagem para outra aplicação que também utiliza **Ubuntu** e **bash**, podemos reaproveitar essas camadas. Caso ocorra um problema com a camada **Ubuntu**, podemos atualizá-la e rebuildar a imagem, sem afetar as outras partes da árvore de dependências.

Existem duas formas principais de criar imagens Docker:
1. **Dockerfile**: Um arquivo declarativo que define como uma imagem será construída. Utilizamos comandos como:
   - **FROM**: Para especificar uma imagem base.
   - **RUN**: Para executar comandos durante a construção da imagem.
   - **EXPOSE**: Para expor portas específicas.

   O **Dockerfile** é usado para construir imagens de maneira programática e consistente.

2. **Commit de um Container**: Podemos fazer um **commit** das alterações realizadas na camada de leitura/escrita de um container em execução, criando uma nova imagem com as modificações feitas durante a execução.

## Persistência de Dados e Volumes

Os containers são leves e rápidos porque suas imagens são imutáveis, ou seja, o estado inicial da imagem não muda. No entanto, containers possuem uma camada de **leitura e escrita (Read/Write)**, onde podemos fazer alterações enquanto o container está em execução. Essas mudanças não afetam a imagem, e se o container for destruído, todas as alterações são perdidas.

Para persistir dados, utilizamos **volumes**. Volumes são uma maneira de compartilhar pastas entre o host e o container. Dessa forma, tudo o que é escrito no container é mantido no host e não se perde ao destruir o container.

## Docker Host e Docker Client

O **Docker Host** é responsável por unir **namespaces**, **cgroups** e **OFS** e rodar uma **daemon** que permite a execução dos containers. Para se comunicar com o Docker Host, utilizamos um **Docker Client**, que é um programa que envia comandos para a daemon do Docker Host.

O Docker Host possui um cache de imagens. Assim, quando um `pull` de uma imagem é feito, a imagem é armazenada em cache, evitando a necessidade de novos downloads quando se cria outros containers.

O Docker Host também gerencia:
- **Volumes**: Para permitir a persistência de dados.
- **Rede**: Para permitir a comunicação entre containers. Como containers são processos independentes, eles precisam de um meio para se comunicar. O Docker fornece o gerenciamento de redes necessário para isso.

## Image Registry

As imagens Docker são armazenadas em um **Image Registry**, que é um repositório onde as imagens podem ser distribuídas. O mais comum é o **Docker Hub**. Podemos:
- Fazer um **pull** para baixar uma imagem do registry.
- Fazer um **push** para enviar uma imagem construída localmente.

## Conclusão

O Docker combina tecnologias como **namespaces**, **cgroups** e **OFS** para criar containers leves e rápidos. O uso de **imagens em camadas** permite a reutilização de dependências, enquanto o **Dockerfile** e o **commit de containers** possibilitam a criação e atualização de imagens. Para persistir dados, utilizamos **volumes**, e para armazenar e compartilhar imagens, utilizamos um **Image Registry**.

Essa abordagem permite o desenvolvimento e a implantação consistentes de aplicações, trazendo flexibilidade e eficiência para ambientes de TI modernos.

