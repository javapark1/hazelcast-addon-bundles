# IMDG Cluster: v3.11.1

As part of the Rolling Upgrade lab of Hazelcast Operations Training, this cluster has been preconfigured to run with Hazelcast Enterprise 3.11.1 which must be installed separately.

## Hazelcast Enterprise 3.11.1

You can download version 3.11.1 from the following link:

[Hazelcast Enterprise Downloads](https://hazelcast.com/download/customer/)

## Creating Workspace

Following the lab instructions, run the `create_workspace` command to create the `ws-3.11.1` workspace. You will be prompt for the IMDG path which must be the Hazelcast Enterprise 3.11.1 home path as shown in the example below.

```console
create_workspace -name ws-3.11.1

Please answer the prompts that appear below. If you are not able to complete
the prompts at this time then use the '-quiet' option to bypass the prompts.
You can complete the requested values later in the generated 'setenv.sh' file
You can abort this command at any time by entering 'Ctrl-C'.

Enter Java home path.
[/Library/Java/JavaVirtualMachines/jdk1.8.0_212.jdk/Contents/Home]:

Enter Hazelcast (IMDG or Jet) Enterprise home directory path. Choose one
from the defaults listed below or enter another.
   /Users/dpark/Hazelcast/hazelcast-enterprise-3.12.4
   /Users/dpark/Hazelcast/hazelcast-jet-enterprise-3.2
[/Users/dpark/Hazelcast/hazelcast-enterprise-3.12.4]:
/Users/dpark/Hazelcast/hazelcast-enterprise-3.11.1
Enter workspace name.
[ws-3.11.1]:

Enter default cluster name.
[myhz]:

Enable VM? Enter 'true' or 'false' [false]: 
```


## Installing Bundle

Switch workspace to `ws-3.11.1` and install this bundle.

```console
switch_workspace ws-3.11.1
install_bundle -download bundle-imdg-cluster-v3.11.1.tar.gz
```

## Starting Cluster

Upon successful bundle installation, switch cluster and start the cluster.

```console
switch_cluster v3.11.1
start_cluster
```
