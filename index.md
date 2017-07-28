---

copyright:
  years: 2016,2017
lastupdated: "2017-07-13"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Getting started with Compose for ScyllaDB
{: #getting-started-with-compose-for-scylladb}

ScyllaDB is an in-place replacement for the Cassandra wide-column distributed database. ScyllaDB is written in C++, rather than Cassandra's Java, for better resource usage that can result in 10 times better performance in benchmarks. In addition to retaining compatibility with Cassandra tool and data files, ScyllaDB adds self-tuning capabilities. {{site.data.keyword.composeForScyllaDB_full}} extends the capabilities of ScyllaDB by managing it for you, offering an easy, auto-scaling deployment system that delivers high availability and redundancy, plus automated backups.
{:shortdesc}

**Note:** {{site.data.keyword.composeForScyllaDB_full}} does not give access to the Compose UI. See [Compose on Bluemix Support](https://help.compose.com/docs/bluemix-compose-support) for more details.

## Creating a Compose for ScyllaDB service instance

[Create a {{site.data.keyword.composeForScyllaDB}} instance](https://console.ng.bluemix.net/catalog/services/compose-for-scylladb/).

When you create an instance of the service, choose both a name for your service and a credential name. Leave the service unbound; you can connect an application to your service later by using the credentials that are provided when the service is provisioned. The various credential values are listed in the "Available credentials" section.

When you provision your {{site.data.keyword.composeForScyllaDB}} instance you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your {{site.data.keyword.composeForScyllaDB}} instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [Compose Enterprise documentation](../ComposeEnterprise/index.html) for more details.

## Managing Compose for ScyllaDB

You can manage your service from the service dashboard. Here you can find information about your Bluemix Compose database and how to connect to it. You can also manage your backups from the dashboard. For more information, see [Dashboard Overview](./dashboard-overview.html) and [Backups](./managing-backups.html).

## Connecting to Compose for ScyllaDB

You can connect to your service using the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

## Connecting a Bluemix application to Compose for ScyllaDB

To connect a Bluemix application to your service, use the credentials that are created along with the service. You can find information on how to connect a Bluemix application to a {{site.data.keyword.composeForScyllaDB}} service in [Connecting a Bluemix Application](./connecting-bluemix-app.html).

## Connecting to Compose for ScyllaDB from outside Bluemix

If you want to connect to {{site.data.keyword.composeForScyllaDB}} from outside Bluemix, you can use the provided connection strings or command line. You can find information on how to connect in [Connecting an external application](./connecting-external.html).