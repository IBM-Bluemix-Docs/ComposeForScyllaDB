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

# Arquitetura 
{: #architecture}

## Replicação

Um serviço {{site.data.keyword.composeForScyllaDB_full}} contém um cluster com três réplicas de dados. Todas as réplicas são igualmente importantes; não há nenhuma réplica primária ou principal e cada nó conhece o estado dos outros nós. Você seleciona uma região na qual o serviço é implementado, as três réplicas de dados são distribuídas pelas zonas de disponibilidade da região e, em seguida, seus dados são copiados entre os três. Quando um membro de dados se torna inacessível, seu cluster continua a operar normalmente.

## Criptografia de Disco

Todos os serviços do  {{site.data.keyword.composeForScyllaDB}}  têm criptografia em repouso. Seus dados residem em servidores que possuem criptografia em nível de volume ativada. 

Como o {{site.data.keyword.composeForScyllaDB}} é um serviço gerenciado, os operadores do {{site.data.keyword.cloud_notm}} Compose podem ver os dados. Além da criptografia de disco, é necessário criptografar informações pessoais antes de armazená-las no banco de dados ou usar extensões ou recursos para ativar a criptografia no próprio banco de dados. Embora esses métodos possam afetar a usabilidade ou o desempenho, é uma boa prática assegurar-se de que as informações pessoais sejam protegidas com criptografia.

## Auto-scaling

Os recursos são escalados automaticamente com base no uso total de armazenamento em disco dos bancos de dados do Scylla. A memória baseia-se em uma razão de disco provisionado em 1:10. Esses recursos são empacotados como unidades.

O Auto-scaling foi projetado para responder às tendências de curto a médio prazo de seu banco de dados. Seu serviço é verificado a cada hora. Se ele estiver sendo executado com pouco espaço em disco, mais unidades serão alocadas para a implementação. 

O Auto-scaling não reduz a capacidade de implementações em que o uso de disco/memória foi reduzido. Os recursos provisionados para seus bancos de dados permanecem para suas necessidades futuras ou até você reduzir a capacidade de sua implementação manualmente.
{: .tip}


