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

# Connection Configuration
{: #connection-configuration}

{{site.data.keyword.composeForScyllaDB_full}} database connections are managed by 3 HAProxy portals. Each portal has 64 MB of memory.

Failover at the client side is the responsibility of the application designer. Most Scylla drivers are cluster-aware, in that you can provide one connection string, all three connection strings, the connection maps, or some combination of these to handle multiple connection strings and graceful failover automatically. Unless you are working with a driver which completely handles failover errors, you should take steps to handle them yourself.

* Work with a driver that accepts multiple endpoints in its connection configuration.
* Ensure that your application's calls to read or write to the database react to errors appropriately. Options include:
  + Logging the error and reconnecting.
  + Reconnecting, and retrying the operation that caused the error.
  + Queuing the failed operation and scheduling reconnection.

## Encryption in Transit

All {{site.data.keyword.composeForScyllaDB}} HAProxy portals are TLS/SSL-enabled and support TLS version 1.2. The certificates for the service are Let's Encrypt certificates. Some drivers, and the command line interface, `cqlsh`, need a copy of the certificate chain to connect to your service, for more information see the [Using Certificates](https://console.{DomainName}/docs/services/ComposeForScyllaDB/scylla-certificates.html)

## Connection Limits

Database connections have a limit of 1000 connections per portal. Exceeding the connection limit will make your service unresponsive. You can manage connections from your application through your driver's connection pooling feature.

## Proxy Timeouts

The HAProxy portals manage the lifecycle of connections between the server and client. They are detailed here as a reference for driver writers and others who have an interest in the low-level activity of {{site.data.keyword.composeForScyllaDB}} database connections.

Setting | Value | Applies
----------|-----------|-----------
`client` | 5 m | When the client is expected to acknowledge or send data.
`connect` | 4 s | When a connection is being made between the proxy and the server.
`server` | 5 m | When the server is expected to acknowledge or send data.
`tunnel` | 1 day | When a bidirectional connection is established between a client and a server, and the connection remains inactive in both directions.
`client-fin` | 1 h | When the client is expected to acknowledge or send data while one direction is already shut down.
`check` | 5 s | As a secondary timeout check when a connection is being made between the proxy and the server.

{: caption="Table 1. Scylla HAProxy timeouts" caption-side="top"}
