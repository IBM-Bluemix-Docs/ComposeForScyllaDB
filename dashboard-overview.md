---

Copyright:
  years: 2017,2018
lastupdated: "2018-05-07"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Service Overview

The _Overview_ page shows you information about your {{site.data.keyword.cloud}} Compose database. The overview includes essential identifying information and current resource usage. You'll also find a section for connection strings that you can use to connect to your database.

## Deployment Details

The _Deployment Details_ panel shows details of your service.

![Deployment Details](./images/scylla-deployment-details.png "A view of the Deployment Details panel")

### Type

The type of database that is offered by the service, and the database version that your service uses. If a more recent database version is available, a notification is displayed, together with a link to the [Upgrade version](/docs/services/ComposeForScyllaDB/dashboard-settings.html#upgrade-version) section of your service dashboard.

### ID

An internal identifier for the service.

### Usage

The size of your database and the amount of storage that is provided by your service plan.

## Current Jobs

Making administrative changes to your service (such as scaling, or taking a manual backup) starts a job. While a job is running, the _Current Jobs_ panel appears on the _Overview_ page, showing the job name and a progress bar. When the job is complete, the _Current Jobs_ panel no longer appears on the _Overview_ page.

## Connection Strings

You can find each Connection String for your service in a different tab in the _Connection Strings_ panel.

### HTTPS

A URI-formatted connection string that can be used by some client libraries, and which contains all the information that is needed for other libraries to connect. You can find out how to use the Connection String to connect in [Connecting an external application](./connecting-external.html).

### Command Line

The **Command Line** is a preformatted command that invokes `cqlsh` with the correct parameters. The displayed command includes the required flags (`--ssl` and `--cqlversion`). To use it, you need to install the PostgreSQL client tools on the local system. For more information, see [Connecting an external application](./connecting-external.html).

### Maps
These address translation maps can be used in applications that require high-availability and can use auto-discovery to find nodes in the cluster. They translate the external portal addresses to the internal cluster addresses for client drivers that use this feature.

### Socks and Nodetool
To enable nodetool administration of Scylla, the service comes with an SSH capsule configured as a SOCKS proxy. You need to add an SSH key to your deployment to use the proxy. You can find out more in [Using Nodetool](./scylla-nodetool.html).


## Instance Administration API

You can manage your {{site.data.keyword.composeForScyllaDB}} service through the {{site.data.keyword.cloud_notm}} Compose API.

### Foundation Endpoint

The foundation endpoint is composed of the region the service is deployed in and the service instance ID. It is at the start of every endpoint.

### Deployment ID

The deployment ID is necessary for most calls, and identifies the specific deployment instance.

### Reference

For more documentation and reference for using the {{site.data.keyword.cloud_notm}} Compose API, across all {{site.data.keyword.cloud_notm}} Compose services, read [The {{site.data.keyword.cloud_notm}} Compose API](https://www.compose.com/articles/the-ibm-cloud-compose-api/).
