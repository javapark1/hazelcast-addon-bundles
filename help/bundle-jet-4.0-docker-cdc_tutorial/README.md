# Jet CDC Tutorial

The `cdc_tutorial` bundle wraps the CDC tutorial available from the Hazelcast site [https://jet-start.sh/docs/tutorials/cdc](https://jet-start.sh/docs/tutorials/cdc). Simply, install and run.

## Installing Bundle

```console
install_bundle -download bundle-jet-4.0-docker-cdc_tutorial.tar.gz
```

## Use Case

This use case introduces the Debezium MySQL connector a Jet job for ingesting database CDC records into a Jet cluster.

![CDC Tutorial Data Flow](/images/jet-cdc-tutorial.png)

## Required Software

- Docker

## Tutorial Link

The following link provides detailed explanations to the data flow and source code.

[https://jet-start.sh/docs/tutorials/cdc](https://jet-start.sh/docs/tutorials/cdc)

## Building App

```console
cd_docker cdc_tutorial; cd bin_sh
./build_app
```

## Starting MySQL Docker Container

If you have your own MySQL running, stop it first before starting the `mysql` container.

```console
cd_docker cdc_tutorial; cd bin_sh

# Start mysql container
./start_mysql
```

## Starting Jet Cluster

First, create a Jet cluster with default settings. Any Jet cluster with default settings will work. For our example, we will use the default cluster, `myjet`.

```console
create_cluster -cluster myjet
```

Start the cluster.

```console
switch_cluster myjet
start_cluster
```

## Submitting Debezium Connector Job

The default Jet cluster has been configured with the start port 6701. Unless the cluster has been configured with the default port of 5701, we need to specify the port number.

```console
jet --addresses localhost:6701 submit target/cdc-tutorial-1.0-SNAPSHOT.jar
```

## Updating Database

```console
# Start MySQL CLI
./start_mysql_cli
```

From the CLI, update the `customers` table.

```console
mysql> use inventory;
mysql> UPDATE customers SET first_name='Anne Marie' WHERE id=1004;
```

## Viewing Data Updates

```console
cd_docker cdc_tutorial; cd bin_sh
./read_cache
```

**Output:**

```console
Currently there are following customers in the cache:
        Customer {id=1002, firstName=George, lastName=Bailey, email=gbailey@foobar.com}
        Customer {id=1004, firstName=Anne Marie, lastName=Kretchmar, email=annek@noanswer.org}
        Customer {id=1003, firstName=Edward, lastName=Walker, email=ed@walker.com}
        Customer {id=1001, firstName=Sally, lastName=Thomas, email=sally.thomas@acme.com}
```

## Tearing Down

```console
cd_docker cdc_tutorial; cd bin_sh

# Stop and prune Docker containers
./cleanup

# Stop cluster
stop_cluster
```
