---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Configuração da Conexão
{: #connection-configuration}

As conexões com o banco de dados do {{site.data.keyword.composeForScyllaDB_full}} são gerenciadas por 3 portais HAProxy. Cada portal tem 64 MB de memória.

Failover no lado do cliente é responsabilidade do designer de aplicativo. A maioria dos drivers Scylla reconhece o cluster, em que é possível fornecer uma sequência de conexões, todas as três sequências de conexões, os mapas de conexão ou alguma combinação deles para manipular múltiplas sequências de conexões e failovers normais automaticamente. A não ser que esteja trabalhando com um driver que manipule completamente erros de failover será necessário:

* Trabalhar com um driver que aceite múltiplos terminais em sua configuração de conexão.
* Assegurar que as suas chamadas de aplicativos leiam ou gravem adequadamente na reação do banco de dados a erros. As opções incluem:
  + Criar log do erro e reconectar.
  + Reconectar e tentar novamente a operação que causou o erro.
  + Enfileirar a operação com falha e planejar a reconexão.

## Criptografia em Trânsito

Todos os portais HAProxy do {{site.data.keyword.composeForScyllaDB}} têm o TLS/SSL ativado e suportam o TLS versão 1.2. Os certificados para o serviço são certificados Let's Encrypt. O driver Certian e a interface da linha de comandos, cqlsh, precisam de uma cópia da cadeia de certificados para se conectar ao serviço. Para obter mais informações, consulte [Usando certificados](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)

## Limites de Conexão

As conexões com o banco de dados têm um limite de 1.000 conexões por portal. Exceder o limite de conexão tornará seu serviço não responsivo. É possível gerenciar conexões de seu aplicativo por meio do recurso de definição do conjunto de conexões do driver.

## Tempos limite de proxy

Os portais HAProxy gerenciam o ciclo de vida de conexões entre o servidor e o cliente. Nós as detalhamos aqui como uma referência para gravadores de driver e outros que têm interesse na atividade de baixo nível de conexões com o banco de dados do {{site.data.keyword.composeForScyllaDB}}.

Configurando | Value | Aplica
----------|-----------|-----------
cliente | 5m | Quando se espera que o cliente reconheça ou envie dados.
conectar | 4s | Quando uma conexão está sendo feita entre o proxy e o servidor.
server | 5m | Quando se espera que o servidor reconheça ou envie dados.
túnel | 1 dia | Quando uma conexão bidirecional é estabelecida entre um cliente e um servidor e a conexão permanece inativa em ambas as direções.
cliente-fin | 1h | Quando se espera que o cliente reconheça ou envie dados enquanto uma direção já está encerrada.
check | 5s | Como uma verificação de tempo limite secundário quando uma conexão está sendo feita entre o proxy e o servidor.

{: caption="Tabela 1. Tempos limite de Scylla HAProxy" caption-side="top"}
