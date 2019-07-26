---
copyright:
  years: 2016,2018
lastupdated: "2018-06-18"

keywords: scylla, compose

subcollection: compose-for-scylladb

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}
{:tip: .tip}

# Connection Architecture
{: #connection-architecture}

{{site.data.keyword.composeForScyllaDB_full}} database connections are managed by 3 HAProxy portals. Each portal has 64 MB of memory.

Failover at the client side is the responsibility of the application designer. Most Scylla drivers are cluster-aware, so you can use one or more connection strings, the connection maps, or a combination of both to automatically handle connections and graceful failover.

Unless you're working with a driver that completely handles failover errors, you need to take steps to handle them yourself.

* Work with a driver that accepts multiple endpoints in its connection configuration.
* Ensure that your application's calls to read or write to the database handle errors. Options include:
  + Logging the error and reconnecting.
  + Reconnecting, and retrying the operation that caused the error.
  + Queuing the failed operation and scheduling reconnection.

## Encryption in Transit

All {{site.data.keyword.composeForScyllaDB}} HAProxy portals are TLS/SSL-enabled and support TLS version 1.2. The certificates for the service are Let's Encrypt certificates. Some drivers, and the command line interface, `cqlsh`, need a copy of the certificate chain to connect to your service. For more information, see [Using Certificates](/docs/services/ComposeForScyllaDB?topic=compose-for-scylladb-scylla-certificates)

## Connection Limits

Database connections have a limit of 1000 connections per portal. Exceeding the connection limit makes your service unresponsive. You can manage connections from your application through your driver's connection pooling feature.

## Proxy Timeouts

The HAProxy portals manage the lifecycle of connections between the server and client. They're detailed here as a reference for driver writers and others who have an interest in the low-level activity of {{site.data.keyword.composeForScyllaDB}} database connections.

Setting | Value | Details
----------|-----------|-----------
`client` | 5 m | Applies when the client is expected to acknowledge or send data.
`connect` | 4 s | Applies when a connection is being made between the proxy and the server.
`server` | 5 m | Applies when the server is expected to acknowledge or send data.
`tunnel` | 1 day | Applies when a bidirectional connection is established between a client and a server, and the connection remains inactive in both directions.
`client-fin` | 1 h | Applies when the client is expected to acknowledge or send data while one direction is already shut down.
`check` | 5 s | Used as a secondary timeout check when a connection is being made between the proxy and the server.

{: caption="Table 1. Scylla HAProxy timeouts" caption-side="top"}
