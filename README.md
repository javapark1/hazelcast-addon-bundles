# hazelcast-addon-bundles

This repo contains readily runnable hazelcast-addon bundles. It is an experimental repo and subject to change.

A hazelcast-addon bundle is a tarball that includes all the necessary files to activate a Hazelcast environment in which you can start an IMDG or Jet cluster and/or run Hazelcast apps. You can find README.md that provides detailed installation, build, and execution instructions in the cluster and app directories.

## Bundle Naming Conventions

A bundle may contain configuration files pertaining to cluster, app or both. 

### Cluster

A cluster bundle contains only cluster files. It may contain more than one cluster.

```console
bundle-imdg|jet-<version>-cluster-<cluster-name>.tar.gz
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

## Version Compatibility

The version number in the bundle name indicates that the bundle has been created in the workspace which was configured to run that particular version of IMDG or Jet. Note that in most cases but not always, the bundles are major version number compatible. For example, a bundle created in a 3.12.4 workspace should run in a 3.13.1 workspace, but may not in a 4.0 workspace.

## Installing Bundles

To install bundles, run the `install_bundle` command included in `hazelcast-addon`. Before installing a bundle, make sure you are in the workspace configured with the compatible IMDG or Jet version. Always execute the `switch_workspace` command first to enter the workspace of your choice before installing a bundle.

### Preview Bundle

Before you install a bundle, you can preview the contents of the bundle by specifying the `preview` option as shown in the following example.

```console
install_bundle bundle-imdg-3.12.4-cluster-app-openssl-perf_test_openssl.tar.gz -preview
```

### Bundle Installation Example

The following example shows typical bundle installation steps. Each bundle includes `README.md` with instructions in the cluster and/or app directories.

```console
# Switch to my-jet-ws which has been configured with Jet 3.2
switch_workspace my-jet-ws

# Install the trade cluster in my-jet-ws
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
