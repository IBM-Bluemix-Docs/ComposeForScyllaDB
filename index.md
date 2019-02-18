---

copyright:
  years: 2016,2018
lastupdated: "2018-03-27"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# About {{site.data.keyword.composeForScyllaDB}}
{: #about}

ScyllaDB is an in-place replacement for the Cassandra wide-column distributed database. ScyllaDB is written in C++, rather than Cassandra's Java, for better resource usage that can result in 10 times better performance in benchmarks. In addition to retaining compatibility with Cassandra tool and data files, ScyllaDB adds self-tuning capabilities. {{site.data.keyword.composeForScyllaDB_full}} extends the capabilities of ScyllaDB by managing it for you, offering an easy, auto-scaling deployment system that delivers high availability and redundancy, plus automated backups.
{:shortdesc}

## Creating a {{site.data.keyword.composeForScyllaDB}} service instance

You can create a {{site.data.keyword.composeForScyllaDB}} service from the [{{site.data.keyword.composeForScyllaDB}} page](https://{DomainName}/catalog/services/compose-for-scylladb/) in the {{site.data.keyword.cloud_notm}} catalog.

Choose a service name, and a region, organization and space to provision the service in. You can use the **Select a database version** field to choose from the available database versions.

When you provision your {{site.data.keyword.composeForScyllaDB}} instance you can choose the *Standard* or *Enterprise* plans. With the *Enterprise* plan, you can provision your {{site.data.keyword.composeForScyllaDB}} instance into an available {{site.data.keyword.composeEnterprise}} cluster. {{site.data.keyword.composeEnterprise}} provides the security and isolation required by enterprise compliance and uses dedicated networking to ensure the performance of the deployed databases. See the [{{site.data.keyword.composeEnterprise}} documentation](/docs/services/ComposeEnterprise/index.html) for more details.

## Managing {{site.data.keyword.composeForScyllaDB}}

You can manage your service from the service dashboard. Here you can find information about your {{site.data.keyword.cloud_notm}} Compose database and how to connect to it. You can also:

- manage your backups
- allocate more resources for your service 
- use whitelists to restrict access to your databases. 

For more information, see [Settings](/docs/services/ComposeForScyllaDB?topic=compose-for-scylladb-dashboard-settings).

{{site.data.keyword.composeForScyllaDB}} relies on Cloud Foundry roles to manage access to the service. Only users with the Developer role can see or use the service dashboard. For more information on Cloud Foundry roles, see the [Cloud Foundry access](/docs/iam?topic=iam-cfaccess) and the [Managing Cloud Foundry access](https://{DomainName}/docs/iam/mngcf.html#mngcf) pages.
{: tip}

## Connecting to {{site.data.keyword.composeForScyllaDB}}

You can connect to your service using the credentials that are created along with the service, or with the connection strings and command line that are provided in the *Overview* tab of your service dashboard.

## Connecting an {{site.data.keyword.cloud_notm}} application to {{site.data.keyword.composeForScyllaDB}}

To connect an {{site.data.keyword.cloud_notm}} application to your service, use the credentials that are created along with the service. You can find information on how to connect an {{site.data.keyword.cloud_notm}} application to a {{site.data.keyword.composeForScyllaDB}} service in [Connecting an {{site.data.keyword.cloud_notm}} Application](/docs/services/ComposeForScyllaDB?topic=compose-for-scylladb-ibmcloud-cf-app).

## Connecting to {{site.data.keyword.composeForScyllaDB}} from outside {{site.data.keyword.cloud_notm}}

If you want to connect to {{site.data.keyword.composeForScyllaDB}} from outside {{site.data.keyword.cloud_notm}}, you can use the provided connection strings or command line. You can find information on how to connect in [Connecting an external application](/docs/services/ComposeForScyllaDB?topic=compose-for-scylladb-external-app).
