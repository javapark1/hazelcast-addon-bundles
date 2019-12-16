# IMDG Cluster: openssl

As part of the TLS/SSL lab of Hazelcast Operations Training, the `openssl` cluster has been preconfigured to enable OpenSSL/BoringSSL.

## Private Key and Trusted Certificate

This distribution contains the following private key and trusted certificate files to be expired on June 19, 2020.

```console
etc/ssl
├── lab.crt
└── lab.key
```

They are created for the domain name "demo.com" and therefore you must set the domain name for your machine. This can be done by entering the following line in the `/etc/hosts` file, which requires root acccess.

```console
# On Unix
sudo vi /etc/hosts

# On Windows
notepad %windir%\system32\drivers\etc\hosts

# Example. Replace the IP address with your machine's IP address "mymachine" to your machine's host name.
192.168.1.6 mymachine server1.demo.com server2.demo.com server3.demo.com
```

## Building `openssl` Cluster

If the bundle does not include openssl binaries in the lib directory then you must run the `build_app` script to download the binaries.

```console
switch_cluster openssl
cd bin_sh
./build_app
```

## Starting Cluster

```console
# Add members. Bundles do not include members.
add_member
add_member

# Start cluster
start_cluster

# See SSL messages in the log file
show_log
```

## Starting Client

```console
cd_app perf_test_openssl
cd bin_sh
./test_ingestion -run

# See SSL outputs
```

## Tearing Down

```console
stop_cluster
```
