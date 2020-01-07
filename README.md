# hazelcast-addon-bundles

This repo contains readily runnable hazelcast-addon bundles. It is an experimental repo and subject to change.

A hazelcast-addon bundle is a tarball that includes all the necessary files to activate a Hazelcast environment in which you can start an IMDG or Jet cluster and/or run Hazelcast apps. You can find README.md that provides detailed installation, build, and execution instructions in the cluster and app directories.

To use this repo, you must first install `hazelcast-addon`:

[https://github.com/javapark1/hazelcast-addon#bundles](https://github.com/javapark1/hazelcast-addon#bundles)

## Bundle Naming Conventions

A bundle may contain configuration files pertaining to cluster, app or both. 

### Cluster

A cluster bundle contains only cluster files. It may contain more than one cluster.

```console
bundle-imdg|jet-<version>-cluster-<cluster-name>[-bin].tar.gz
```
#### Cluster Example

```console
bundle-jet-3.2-cluster-trade.tar.gz
```

### App

An app bundle contains only app files. It may contain more than one app.

```console
bundle-imdg|jet-<version>-app-<app-name>.tar.gz
```

#### App Example

```console
bundle-imdg-3.12.4-app-perf_test.tar.gz
```

### Cluster and App Combo

A cluster and app combo cluster contains both cluster and app files. It may contain multiple clusters and apps.

```console
bundle-imdg|jet-<version>-cluster-app-<cluster-name>-<app-name>.tar.gz
``` 

#### Combo Example

```console
bundle-imdg-3.12.4-cluster-app-openssl-perf_test_openssl.tar.gz
```

### Binary Bundles

The names of the bundles that include binary files end with `-bin.tar.gz`. A binary bundle includes a partial or complete set of binary files. If it contains a complete set of binary files then the build step is typically not required. See the `README.md` file included in the bundle for instructions.

:pushpin: Because the binary files tend to become obsolete over time, they may not always work especially when you have upgraded `hazelcast-addon` or Hazelcast products. For this reason, it is recommended that you should instead install and build non-binary bundles where possible. Note that a binary bundle may also include the build script so that you can wipe out the included binaries and build from scratch as needed.

#### Binary Bundle Example

```console
bundle-imdg-4.0-cluster-app-db-perf_test_db-bin.tar.gz
```

## Version Compatibility

The version number in the bundle name indicates that the bundle has been created in the workspace which was configured to run that particular version of IMDG or Jet. Note that in most cases but not always, the bundles are major version number compatible. For example, a bundle created in a 3.12.4 workspace should run in a 3.13.1 workspace, but may not in a 4.0 workspace.

## Installing Bundles

To install bundles, run the `install_bundle` command included in `hazelcast-addon`. Before installing a bundle, make sure you are in the workspace configured with the compatible IMDG or Jet version. Always execute the `switch_workspace` command first to enter the workspace of your choice before installing a bundle.

### Listing Remote Bundles

The bundles in this repo can be listed using the `install_bundle` command. Simply run the command with or without the `-list` option.

```console
# List remote bundles
install_bundle
```

### Downloading Bundle

For remote bundles, you must first download them before you can install them.

```console
install_bundle -download bundle-imdg-3.12.4-cluster-app-openssl-perf_test_openssl.tar.gz
```

### Previewing Bundle

Before you install a bundle, you can preview the contents of the bundle by specifying the `preview` option as shown in the following example. You can also specify the `-download` option to preview a remote bundle.

```console
install_bundle -preview bundle-imdg-3.12.4-cluster-app-openssl-perf_test_openssl.tar.gz
```

### Bundle Installation Example

The following example shows typical bundle installation steps. Each bundle includes `README.md` with instructions in the cluster and/or app directories.

```console
# Switch to myjet-ws which has been configured with Jet 3.2
switch_workspace myjet-ws

# Download bundle
install_bundle -download -preview bundle-jet-3.2-cluster-trade.tar.gz

# Install the downloaded trade cluster in my-jet-ws
install_bundle bundles/bundle-jet-3.2-cluster-trade.tar.gz

# Switch cluster to the 'trade' cluster you just installed.
switch_cluster trade

# View README.md
less README.md

# You must now add members. All bundles come without members.
add_member
add_member

# For the 'trade' bundle, you must first build its app.
cd bin_sh
./build_app

# Upon successful build, start the trade cluster
start_cluster
```
