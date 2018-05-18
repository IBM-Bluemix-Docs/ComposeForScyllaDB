---

copyright:
  years: 2018
lastupdated: "2018-05-17"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using cqlsh
{: #using-cqlsh}

You can use `cqlsh` to connect directly to {{site.data.keyword.composeForScyllaDB_full}}.

As of writing, `cqlsh` is a Python 2.7 program and does not run with Python 3.
{: .tip}

As `cqlsh` is a Python application, you can run `pip install cqlsh` to install the latest version of the command.

You can also get `cqlsh` from a full distribution of Cassandra by downloading it from [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/), which offers binary, source, and instructions on how to install Cassandra on Debian (apt) and Red Hat (RPM) Linux distributions. Download a binary release and unpack the file into a directory. Then `cd` into that directory and you'll find `cqlsh` in the `bin` directory.

How you get this tool onto your local device depends on your local operating system. The easiest is to install the latest Cassandra release using an appropriate method for your operating system (the latest versions still support version 2.1.8, which is what Scylla is), and then use the built-in `cqlsh` command. 

On a Mac, for example, you can use `homebrew` to install Cassandra by typing `brew install cassandra`.

## cqlshrc files

When the `cqlsh` command runs it loads `$HOME/.cassandra/cqlshrc` by default, which can set many parameters that are not settable in the command line. You can edit the default file to set parameters, and any changes you make apply to all sessions unless they are overridden from the command line.

If you are running multiple instances of Scylla in different environments, you can create a separate `cqlshrc` file and use it by adding `--cqlshrc=[filename]` to your command.


For an example of a full cqlshrc file, see the [Apache sample of cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). 

### cqlshrc for {{site.data.keyword.composeForScyllaDB}}

A minimal `cqlshrc` file for connecting to your service looks like this:

```
[authentication]
username = scylla
password = [password]

[connection]
hostname = [hostname.composedb.com]
port = [portnumber]
factory = cqlshlib.ssl.ssl_transport_factory

[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

Enter the information provided in your service's _Connection Strings_ and save the file as `example.cqlshrc`.

You can now log in to your Scylla database by running `cqlsh --ssl --cqlshrc=example.cqlshrc` from the command line. Note that even though `cqlshrc` has an option for specifying `ssl=true` it appears to be ignored so you must use the `--ssl` flag.

The ssl version cannot be set in the `cqlshrc` file, so you have to either set the environment to use TLSv1.2  or append `SSL_VERSION=TLSv1_2` to the front of the `cqlsh` connection command.

## TLSv1.2

`cqlsh` does not default to TLSv1.2 when negotiating an encrypted connection. To successfully connect to a Compose Scylla deployment, the TLS version must be specified in the environment that is running `cqlsh`. Set SSL_VERSION in the environment using:
`export SSL_VERSION=TLSv1_2` or place `SSL_VERSION=TLSv1_2` before the `cqlsh` connection command.

## Connecting Without Validating

Validation is enabled by default, but you can connect to Scylla with TLS/SSL without validating the remote host. This is not recommended for production systems. Validation is controlled by the environment variable `SSL_VALIDATE`, or by SSL settings in the cqlshrc file. Set `SSL_VALIDATE` to false (`export SSL_VALIDATE=false`) in your environment to disable validation.

Alternatively, you can edit your cqlshrc file to disable validation.

```
[ssl]  
validate = false
```

If you want to disable validation for a single `cqlsh` command, include `SSL_VALIDATE=false` before the `cqlsh` connection command. 

You still have to tell `cqlsh` to use TLSv1.2, even when validation is not enabled. If you haven't set the ssl version in the environment and don't want to validate, the connection should look like this example:

```
SSL_VERSION=TLSv1_2 SSL_VALIDATE=false cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Obtaining and Using the Certificate

If you want to use cqlsh and verify the remote host, you need to obtain the certificate, save it somewhere accessible, and then provide the path to the certificate to cqlsh.

Set the environment variable  `SSL_CERTFILE` to the path and file name of your saved certificate to enable it for all subsequent invocations of `cqlsh`. 

Alternatively, you can add the following to your cqlshrc file.

```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

If you only want to use a particular certificate for a single `cqlsh` command place `SSL_CERTFILE = ~/path_to_your/lechain.pem` at the start of your cqlsh connection command. For example:

```
SSL_VERSION=TLSv1_2 SSL_CERTFILE='~/path_to_your/lechain.pem' cqlsh --ssl portal1204-7.sally-scylla.composedb.com 24981 -u scylla -p [password] --cqlversion=3.3.1
```

## Getting started with cqlsh

If you type `HELP` you can see that the shell has a lot of capability. All of the commands even have `TAB` completion too. For example:

1. Type `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. You should see the choices for the replication class.
2. Choose `SimpleStrategy` here because the cluster won't be spanning multiple data centers.
3. Press `<TAB><TAB>` again and enter 3 for the `replication_factor` value. Then close the brace with `}` and finish the statement with `;<enter>`.

You have created your first KEYSPACE and defaulted it to replicating your data to all three nodes in your cluster.

```sql
scylla@cqlsh> CREATE KEYSPACE example_keyspace WITH replication = {'class': 'SimpleStrategy', 'replication_factor': 3 };
scylla@cqlsh> use example_keyspace ;
scylla@cqlsh:example_keyspace> create table names (
                    ... name_id uuid,
                    ... last_name text,
                    ... first_name text,
                    ... PRIMARY KEY(name_id)
                    ... );
scylla@cqlsh:example_keyspace> 
```

Now you can use the keyspace:

```sql 
USE my_new_keyspace;
```

And your shell now shows that your command prompt is using your keyspace by default. Every table has to have a keyspace.

Type the following `CREATE TABLE` command in the shell to create a table to populate with examples.

```sql
CREATE TABLE my_new_table (
my_table_id uuid,
last_name text,
first_name text,
PRIMARY KEY(my_table_id)
);
```
