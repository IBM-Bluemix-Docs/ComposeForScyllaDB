---

copyright:
  years: 2016,2018
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Sobre o {{site.data.keyword.composeForScyllaDB}}
{: #about-compose-for-scylladb}

ScyllaDB é uma substituição no local para o banco de dados distribuído de coluna ampla do Cassandra. O ScyllaDB é escrito em C++, em vez de Java do Cassandra, para um melhor uso dos recursos, o que pode resultar em um desempenho 10 vezes melhor em avaliações de desempenho. Além disso, para retenção de compatibilidade com a ferramenta Cassandra e os arquivos de dados, o ScyllaDB inclui recursos de autoajuste. O {{site.data.keyword.composeForScyllaDB_full}} amplia os recursos do ScyllaDB gerenciando-o para você, oferecendo um sistema de implementação fácil e de escala automática que oferece alta
disponibilidade e redundância, além de backups automatizados.
{:shortdesc}

## Criando uma instância de serviço do {{site.data.keyword.composeForScyllaDB}}

É possível criar um serviço do {{site.data.keyword.composeForScyllaDB}} por meio da página [{{site.data.keyword.composeForScyllaDB}}](https://console.{DomainName}/catalog/services/compose-for-scylladb/) no catálogo do {{site.data.keyword.cloud_notm}}.

Escolha um nome do serviço e uma região, organização e espaço no qual provisionar o serviço. É possível usar o campo **Selecionar uma versão do banco de dados** para escolher nas versões do banco de dados disponíveis.

Quando você provisiona sua instância do {{site.data.keyword.composeForScyllaDB}}, é possível escolher os planos *Padrão* ou *Corporativo*. Com o plano *Corporativo*, é possível provisionar sua instância do {{site.data.keyword.composeForScyllaDB}} em um cluster disponível do {{site.data.keyword.composeEnterprise}}. O {{site.data.keyword.composeEnterprise}} fornece a segurança e o isolamento requeridos pela conformidade corporativa e usa rede dedicada para assegurar o desempenho dos bancos de dados implementados. Veja a [Documentação do Compose Enterprise](../ComposeEnterprise/index.html) para obter mais detalhes.

## Gerenciando {{site.data.keyword.composeForScyllaDB}}

É possível gerenciar seu serviço no painel de serviço. Aqui é possível localizar informações sobre o banco de dados do {{site.data.keyword.cloud_notm}} Compose e como conectar-se a ele. Também é possível:

- gerenciar seus backups
- alocar mais recursos para seu serviço 
- use listas de desbloqueio para restringir o acesso a seus bancos de dados. 

Para obter mais informações, veja [Configurações](./dashboard-settings.html).

## Conectando-se ao {{site.data.keyword.composeForScyllaDB}}

É possível se conectar a seu serviço usando as credenciais que são criadas junto com o serviço ou com as sequências de conexões e linha de comandos que são fornecidas na guia *Visão geral* de seu painel de serviço.

## Conectando um aplicativo {{site.data.keyword.cloud_notm}} ao {{site.data.keyword.composeForScyllaDB}}

Para conectar um aplicativo {{site.data.keyword.cloud_notm}} a seu serviço, use as credenciais que são criadas com o serviço. É possível localizar informações sobre como conectar um aplicativo {{site.data.keyword.cloud_notm}} a um serviço {{site.data.keyword.composeForScyllaDB}} em [Conectando um aplicativo {{site.data.keyword.cloud_notm}}](./connecting-bluemix-app.html).

## Conectando-se ao {{site.data.keyword.composeForScyllaDB}} de fora do {{site.data.keyword.cloud_notm}}

Se você deseja se conectar ao {{site.data.keyword.composeForScyllaDB}} de fora do {{site.data.keyword.cloud_notm}}, é possível usar as sequências de conexões fornecidas ou a linha de comandos. É possível localizar informações sobre como conectar-se em [Conectando um aplicativo externo](./connecting-external.html).
