---

Copyright:
  Years: 2017
lastupdated: "2017-09-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Visão geral do serviço

A página _Visão geral_ mostra informações sobre seu banco de dados do {{site.data.keyword.cloud}} Compose. A visão geral inclui informações essenciais de identificação e o uso do recurso atual. Você também localizará uma seção para sequências de conexões que podem ser usadas com ferramentas ou fazer uso de ferramentas para se conectar a seu banco de dados.

## Detalhes da implementação

O painel _Detalhes da implementação_ mostra detalhes de seu serviço.

![Deployment Details](./images/scylla-deployment-details.png "A view of the Deployment Details panel")

### Tipo

O tipo de banco de dados que é oferecido pelo serviço e a versão do banco de dados que seu serviço usa.

### Nome

Um identificador interno para o serviço.

### Uso

O tamanho de seu banco de dados e a quantia de armazenamento fornecido por seu plano de serviço.


## Sequências de conexões

Você localizará cada Sequência de conexões para seu serviço em uma guia diferente no painel _Sequências de conexões_.

### HTTPS

Uma sequência de conexões formatadas por URI que pode ser usada por algumas bibliotecas do cliente e contém todas as informações necessárias para outras bibliotecas se conectarem. É possível descobrir como usar a Sequência de conexões para conectar-se em [Conectando um aplicativo externo](./connecting-external.html).

### Linha de comandos

A **Linha de comandos** é um comando pré-formatado que chamará `cqlsh` com os parâmetros corretos. O comando exibido inclui as sinalizações necessárias (`--ssl` e `--cqlversion`). Para usá-lo, você precisará ter as ferramentas do cliente PostgreSQL no sistema local. É possível descobrir mais sobre como fazer isso em [Conectando um aplicativo externo](./connecting-external.html).

### Mapas
Estes mapas de conversão de endereço podem ser usados em aplicativos que requerem alta disponibilidade e podem usar a autodescoberta para localizar os nós no cluster. Eles convertem os endereços de portal externo para os endereços de cluster interno para drivers de cliente que usam esse recurso.

### Certificado SSL

Seu serviço Compose {{site.data.keyword.cloud_notm}} fornece um certificado SSL que é possível usar para conectar-se a seu banco de dados.
