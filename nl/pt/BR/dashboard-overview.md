---

Copyright:
  years: 2017,2018
lastupdated: "2017-12-11"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visão geral do serviço

A página _Visão geral_ mostra informações sobre seu banco de dados do Compose do {{site.data.keyword.cloud}}. A visão geral inclui informações essenciais de identificação e o uso do recurso atual. Você também localizará uma seção para sequências de conexões que podem ser usadas com ferramentas ou fazer uso de ferramentas para se conectar a seu banco de dados.

## Detalhes da implementação

O painel _Detalhes da implementação_ mostra detalhes de seu serviço.

![Deployment Details](./images/scylla-deployment-details.png "A view of the Deployment Details panel")

### Tipo

O tipo de banco de dados que é oferecido pelo serviço e a versão do banco de dados que seu serviço usa. Se uma versão de banco de dados mais recente estiver disponível, uma notificação será exibida, junto a um link para a seção [Fazer upgrade da versão](/docs/services/ComposeForScyllaDB/dashboard-settings.html#upgrade-version) de seu painel de serviço.

### Nome

Um identificador interno para o serviço.

### Uso

O tamanho de seu banco de dados e a quantia de armazenamento fornecido por seu plano de serviço.

## Tarefas atuais

Fazer mudanças administrativas em seu serviço (como ajuste de escala ou execução de um backup manual) inicia uma tarefa. Enquanto uma tarefa estiver em execução, o painel _Tarefas atuais_ aparecerá na página _Visão geral_, mostrando o nome da tarefa e uma barra de progresso. Quando a tarefa for concluída, o painel _Tarefas atuais_ não aparecerá mais na página _Visão geral_.

## Sequências de conexões

Você localizará cada Sequência de conexões para seu serviço em uma guia diferente no painel _Sequências de conexões_.

### HTTPS

Uma sequência de conexões formatadas por URI que pode ser usada por algumas bibliotecas do cliente e contém todas as informações necessárias para outras bibliotecas se conectarem. É possível descobrir como usar a Sequência de conexões para conectar-se em [Conectando um aplicativo externo](./connecting-external.html).

### Linha de comandos

A **Linha de comandos** é um comando pré-formatado que chamará `cqlsh` com os parâmetros corretos. O comando exibido inclui as sinalizações necessárias (`--ssl` e `--cqlversion`).  Para usá-lo, você precisará ter as ferramentas do cliente PostgreSQL no sistema local. É possível descobrir mais sobre como fazer isso em [Conectando um aplicativo externo](./connecting-external.html).

### Mapas
Estes mapas de conversão de endereço podem ser usados em aplicativos que requerem alta disponibilidade e podem usar a autodescoberta para localizar os nós no cluster. Eles convertem os endereços de portal externo para os endereços de cluster interno para drivers de cliente que usam esse recurso.

### Socks e Nodetool
Para ativar a administração de nodetool do Scylla, o serviço é fornecido com uma cápsula SSH configurada como um proxy SOCKS. É necessário incluir uma chave SSH na implementação para usar o proxy. É possível descobrir mais em [Usando o nodetool](./scylla-nodetool.html).


## API de administração de instância

É possível gerenciar o serviço {{site.data.keyword.composeForScyllaDB}} por meio da API do {{site.data.keyword.cloud_notm}} Compose.

### Terminal de base

O terminal base é composto pela região na qual o serviço reside e pelo ID da instância de serviço. Ela estará no início de cada terminal.

### ID de implementação

O ID de implementação é necessário para a maioria das chamadas e identifica a instância de implementação específica.

### Referência

Para obter mais documentação e referência para usar a API Compose do {{site.data.keyword.cloud_notm}}, ao longo de todos os serviços Compose do {{site.data.keyword.cloud_notm}}, leia [A API Compose do {{site.data.keyword.cloud_notm}}](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
