# App: Pado

The `pado` app provides a Hazelcast Portable class generator and CSV file import tools for Hazelcast.

## Installing Pado App

The `pado` app currently comes in the form of bundle. To install the pado bundle, Run the `install_bundle` command as follows:

```console
install_bundle -download bundle-imdg-3.12.4-app-pado.tar.gz
```

## Building Pado

```console
cd_app pado
cd bin_sh
./build_app
```

## Pado CSV `data` Directory

The Pado CSV `data` directory structure includes the `import` directory where you place the CSV files to import and the `schema` directory in which you provide schema files that define how to parse the CSV files. Pado automatically moves the successfully imported files from the `import` directory to the `processed` directory. It moves the unsuccessful ones in the `error` directory.

```console
data
├── error
├── import
├── processed
└── schema
```

## Running Pado CSV Importer

The Pado CSV importer facility automatically generates schema files, generates and compiles `VersionedPortable` classes, and imports CSV file contents into Hazelcast in the form of `VersionedPortable` objects. The imported data can then be viewed using the `desktop` app. These steps are shown in sequence below.

1. Place CSV files in the `data/import/` directory.
2. Generate schema files using the CSV files in `data/import/`.
3. Generate `VersionedPortable` source code.
4. Compile and create a `VersionedPortable` jar file.
5. Deploy the generated jar file to a Hazelcast cluster and add the Portable factory class in hazelcast.xml.
6. Start a Hazelcast cluster.
7. Import CSV files.
8. View imported data using the `desktop` app.

## NW Demo

For our demo, let's import the NW sample data included in the Pado distribution into Hazelcast. To import data in CSV files, you need to generate schema files. Pado provides the `generate_schema` command that auto-generates schema files based on CSV file contents. Once you have schema files, then you can generate Hazelcast `VersionedPortable` classes by executing the `generate_versioned_portable` command.

1. Change directory to the pado directory and copy the NW CSV files to the import directory. 

```console
cd_app pado
cd pado_<version>

# Copy CSV files into data/import/
cp -r data/nw/import data/
```

2. Generate schema files.

First, edit `bin_sh/setenv.sh` file and set the correct path to `JAVA_HOME`.

```console
vi bin_sh/setenv.sh
```

Generate schema files for the `nw` data

```console
# Generate schema files. The following command generates schema files in the
# data/schema/generated directory.
cd bin_sh/hazelcast
./generate_schema

# Move the generated schema files to data/schema.
mv ../../data/schema/generated/* ../../data/schema/
```

3. Generate `VersionedPortable` source code. The following command reads schema files located in data/schema/ and generates the corresponding `VersionedPortable Java source code.

```console
# Generate VersionedPortable classes with the factory ID of 30000 and the
# start class ID of 30000.
./generate_versioned_portable  -fid 30000 -cid 30000
```

4. Compile and create jar file.

```console
./compile_generated_code
```

5. Deploy the generated jar file to Hazelcast cluster and add the Portable factory class ID in hazelcast.xml.

```console
# Copy the jar file to the hazelcast-addon workspace plugins directory
cp ../../../pado_<vesion>/dropins/generated.jar $HAZELCAST_ADDON_WORKSPACE/plugins/

# Add the Portable factory class ID in hazelcast.xml
switch_cluster myhz

# In hazelcast.xml, add the serialization configuration outputted by
# the generate_versioned_portable command in step 3.
vi etc/hazelcast.xml
             <serialization>
                 <portable-factories>
                     <portable-factory factory-id="30000">
                          org.hazelcast.data.PortableFactoryImpl
                     </portable-factory>
                 </portable-factories>
             </serialization>
```

6. Start Hazelcast cluster

```console
start_cluster
```

7. Import CSV files.

```console
cd_app pado
cd pado_<version>/bin_sh/hazelcast
./import_csv
```

8. View imported data using the `desktop` app.

```console
# If you haven't installed the desktop app then install and build it.
create_app -app desktop
cd_app desktop
cd bin_sh
./build_app

# Change directory to hazelcast-desktop
cd ../hazelcast-desktop_<verson>

# Add serialization configuration in etc/pado.properties
vi etc/pado.properties

hazelcast.client.config.serialization.portable.factories=1:org.hazelcast.demo.nw.data.PortableFactoryImpl,\
10000:org.hazelcast.addon.hql.impl.PortableFactoryImpl,\
30000:org.hazelcast.data.PortableFactoryImpl

# Run desktop
cd bin_sh
./desktop
```

## Dataset Examples

The following links provide Pado instructions for ingesting downloadable datasets.

- [UCI Machine Learning Repository](UCI-ML.md)

## About Pado

Pado is authored by Dae Song Park (email:dspark@netcrest.com) to bring linear scalability to IMDG for storing Big Data. His architecture achieves this by logically federating data grids and providing an abstract API layer that not only hides the complexity of the underlying IMDG API but introduces new Big Data capabilities that IMDG products are not meant to deliver. He coined the terms **grids within grid** and **grid of grids** to illustrate his architecture which spans in-memory data across a massive number of clusters with a universal namespace similar to URL for easy data access.

The current implementation of Pado supports Pivotal GemFire 8.x. Unfortunately, the transformation of GemFire to Apache Geode is requiring a major overhaul to Pado. 

Instead of adapting the Geode code base, the author of Pado is now working towards supporting Hazelcast as its next upgrade. It is envisioned that the same grid federation architecture will apply to Hazelcast with additional native support for popular open source plugins such as Kafka and BI tools.

The `hazelcast-addon` project was inspired by Pado and borrows many architecture and script ideas from Pado.
