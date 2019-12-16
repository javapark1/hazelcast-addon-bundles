# IMDG Cluster: ssl

As part of the TLS/SSL lab of Hazelcast Operations Training, the `ssl` cluster has been preconfigured to enable SSL. It contains scripts to create both private and trust keystores that contain both member and client keys and certificates.

## Creating Keystores

To create keystores, follow the instructions in the Hazelcast Operations Training slide deck or the steps in the order shown below. 

### 1. Member Keystore

```console
# Switch cluster and cd into the bin_sh directory.
# All commands must be executed inside the bin_sh directory
# with the './' prefix.
switch_cluster ssl
cd bin_sh

# Create member keystore to store private keys.
./create_member_keystore

# List private keys in the member keystore.
# You should see one (1) key.
./list_member_keystore
```

### 2. Member Trust Keystore

``` console
# Create client’s trust keystore (Export the trusted certificate
# and import it in the trust keystore)
./create_member_trust_keystore

# List the trusted certificates  in the trust keystore you just created.
./list_member_keystore
```

### 3. Client Keystore

```console
# Change directory to the perf_test_ssl app
cd_app perf_test_ssl
cd bin_sh

# Create the client’s private keystore
./create_client_keystore
```

### 4. Client Trust Keystore

```console
# Create client’s trust keystore
# (Export the trusted certificate and import it in the trust keystore)
./create_client_trust_keystore

# List the trusted certificates  in the trust keystore you just created.
# You should see one (1) trusted certificate.
./list_client_trust_keystore

# Import the member’s trusted certificate into the client’s
# trust keystore.
./import_member_trusted_certificate

# List the client’s trust keystore.
# You should now see two (2) trusted certificates (member and client).
./list_client_trust_keystore
```

### 5. Import Client Key to Member Keystore

```console
# Change directory to the cluster’s bin_sh directory 
cd_cluster ssl
cd bin_sh

# Import client’s private key to member’s keystore
./import_client_key

# List the keys in the member’s keystore
# You should see 2 private keys (member and client)
./list_member_keystore

```

### 6. Import Client Trusted Certificate

```console
# Import the client’s trusted certificate into the member’s 
# trust keystore.
./import_client_trusted_certificate

# List client’s trust keystore
# You should see 2 trusted certificates (member and client)
./list_member_trust_keystore
```

## Starting Cluster

```console
# First, add members. Bundles do not include members.
add_member
add_member

# Start cluster.
start_cluster

# See the log file to see SSL messages.
show_log
```

## Running Client

```console
# Change directory to the perf_test’s bin_sh directory
cd_app perf_test_ssl
cd bin_sh

# Run the perf_test ingestion program
./test_ingestion –run

# See SSL outputs
```

## Starting Management Center

Management Center has also been configured with TLS/SSL using the member's keystore. After starting the MC, obtain the HTTPS URL by running `show_mc`. 

```console
start_mc
show_mc
```

## Browser Notes

Note that depending on your browser, `localhost` in URL may not work. Use the host name instead if it fails. The private key has been generated with your host name as one of domain names.

## Tearing Down

```console
stop_cluster
```
