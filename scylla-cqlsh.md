---

copyright:
  years: 2018
lastupdated: "2018-02-06"
---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:codeblock: .codeblock}
{:pre: .pre}

# Using cqlsh
{: #using-cqlsh}

You can connect directly to {{site.data.keyword.composeForScyllaDB_full}} using `cqlsh`.
There are a number of ways of installing it. As it is a Python application, running `pip install cqlsh` will install the latest version of the command. As of writing, `cqlsh` is a Python 2.7 program and does not run with Python 3.

It is also possible to get `cqlsh` from a full distribution of Cassandra. This can be obtained from [http://cassandra.apache.org/download/](http://cassandra.apache.org/download/) which offers binary, source and instructions on how to install Cassandra on Debian (apt) and Red Hat (RPM) Linux distributions. Download a binary release and unpack the file into a directory. Then `cd` into that directory and you'll find `cqlsh` in the `bin` directory.

How you get this tool onto your local device depends on your local platform. The easiest is to install the latest Cassandra release using an appropriate method for your platform (the latest versions still support version 2.1.8 which is what Scylla is) and use the builtin `cqlsh` command. 

On a Mac, for example, install Cassandra with `homebrew` by typing `brew install cassandra`.

## About cqlshrc files
When it is run the `cqlsh` command loads a file, `$HOME/.cassandra/cqlshrc` by default, which can set many parameters which are not settable in the command line. An [Apache sample of cqlshrc](https://github.com/apache/cassandra/blob/trunk/conf/cqlshrc.sample). You can edit the default file to set parameters. These will apply to all sessions unless overridden on the command line.

If you are running multiple instances of Scylla in different environments, you can create a separate `cqlshrc` file and make `cqlsh` use it by adding `--cqlshrc=[filename]` to your command.

### cqlshrc for {{site.data.keyword.composeForScyllaDB}}
This is an example minimal `cqlshrc` file for connecting to your service:
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

Enter in the information provided from your service's _Connection Strings_ and save this file as example.cqlshrc.
Running `cqlsh --ssl --cqlshrc=example.cqlshrc` from the command-line and you will be logged into your Scylla database. Note that even though `cqlshrc` has an option for specifying `ssl=true` it appears to be ignored so the `--ssl` flag must be used.

## Connecting Without Validating
The simplest way to connect to Scylla with SSL is without validating the remote host. This is not recommended for production. By default, validation is enabled. Validation is controlled by the environment variable `SSL_VALIDATE` or by SSL settings in the cqlshrc file. Set `SSL_VALIDATE` to false in your environment to disable validation. If you want to only disable it for a single `cqlsh` command, place `SSL_VALIDATE=false` before the cqlsh connection command.

Or you may add the following to your cqlshrc file.

```
[ssl]  
validate = false
```

## Obtaining and Using the Certificate
If you want to use cqlsh and verify the remote host you will need to obtain the certificate, save it somewhere accessible, and then provide the path to the certificate to cqlsh. A copy of the certificate lives in the [Using LE Certificates](./scylla-certificates.html) page, as well as details about its contents and their source. Make a certificate file or save a copy locally. 

Set the environment variable  `SSL_CERTFILE` to the path and filename of your saved certificate to enable it for all subsequent invocations of `cqlsh`. If you only want to use a particular certificate for a single `cqlsh` command place `SSL_CERTFILE = ~/path_to_your/lechain.pem` at the start of your cqlsh connection command.

Alternatively, you could add the following to your cqlshrc file.
```
[ssl]
certfile = ~/path_to_your/lechain.pem
validate = true
```

## Using cqlsh
If you type `HELP` you can see that the shell has a lot of capability. What's even nicer is that all of those commands have `TAB` completion too. For example:
1. Type `CREATE KEYSPACE my_new_keyspace <TAB><TAB><TAB>`. You should see the choices for the replication class.
2. You can choose `SimpleStrategy` here because the cluster won't be spanning multiple data centers.
3. Press `<TAB><TAB>` again and enter in 3 for the replication_factor. Then close the brace with `}` and finish the statement with `;<enter>`.

You just created your first KEYSPACE and defaulted it to replicating your data to all three nodes in your cluster.
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