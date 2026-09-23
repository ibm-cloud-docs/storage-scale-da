---

copyright:
  years: 2026
lastupdated: "2026-09-23"

keywords:

subcollection: storage-scale-da

---

{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:external: target="_blank" .external}
{:pre: .pre}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:beta: .beta}
{:row-headers: .row-headers}
{:table: .aria-labeledby="caption"}

# Master Key Rotation for Key Protect Encryption
{: #enable-encryption-keyprotect}

Master key rotation is applicable only when the following criteria are met:

* The cluster must be created with Key Protect encryption enabled.
* The certificate and Key Protect files must be stored only on **storage node‑1** which is located in the `/opt/key_protect` directory.
* Any missing changes in the files will impact the key rotation.

## Key features
{: #key-features}

Following are the master key features:

* The master key does not have an expiration date.
* The master key depends on the client certificate.
* The master key can be re-created using the existing client certificate.
* If the key is changed, the encryption policy must be updated accordingly.
* Newly created objects will be encrypted using the new master key.
* The existing objects remain accessible since the old master key is still available on the Key Protect server.

## Locating the existing certificate
{: #locate-existing-cert}

You must locate and verify the existing certificate available on **storage node‑1** using the following commands:

1. Navigate to the certificate directory.

    ```text
    cd /opt/key_protect/
    ```
    {: codeblock}

2. List the certificate files.

    ```text
    ls -la cert.p12
    ```
    {: codeblock}

3. Verify the certificate details.

    ```text
    file cert.p12
    ```
    {: codeblock}

### Verifying certificate validity
{: #validate}

To verify the certificate validity, run the following command:

```text
# Check certificate expiration and details
openssl pkcs12 -in cert.p12 -nokeys -passin pass:your_password | \
openssl x509 -text -noout | grep -E "(Subject:|Not Before:|Not After:)"
```
{: codeblock}

## Creating the master key using an existing certificate
{: #create-mk-existing-cert}

The `mmkmipkm createkey` command creates a master key using the certificate for Key Management Interoperability Protocol (KMIP) integration.

```text
$ mmkmipkm createkey \
    --host REGION.kms.cloud.ibm.com \
        --kmipport 5696 --keystore cert.p12 --keypass cert.pwd \
        --label RESOURCE_PREFIX
```
{: codeblock}

|Commands|	Description|
|-------------|------------|
|`host`| In the Key Protect host value, replace REGION with your Key Protect server region. Example: us-south.kms.cloud.ibm.com |
|`kmipport`| Port for integration between the node and the key protect server |
|`keystore`| Certificate path |
|`keypass`| File containing the password for the certificate |
|`label`| Resource prefix |
{: caption='Creating Master Key - Commands'}

Applying the above commands will provide you a master key and the same key can be found on the key protect server under the KMIP keys.

## Retrieving the existing encryption policy
{: #retrieve-encrypt-policy}

The `mmlspolicy` command displays the current encryption policies for file sets or filesystems.

### Viewing policies
{: #policies}

```text
# Display policies of a filesystem
$ mmlspolicy FILE_SYSTEM -L
```
{: codeblock}

Here, FILE_SYSTEM – Name of your filesystem.

### Example output
{: #output}

```text
$ mmlspolicy storage -L
RULE 'EncPolicyGenerator Rule' ENCRYPTION 'RULE1' IS
ALGO 'DEFAULTNISTSP800131AFAST'
KEYS ('ba94cadd-a42c-4ea4-8d0a-9c5177325a56: KP')
RULE 'Encrypt all files' SET ENCRYPTION 'RULE1' WHERE NAME LIKE '%'
```
{: codeblock}

## Updating the encryption policy
{: #update-policy}

After retrieving the encryption policy, create a new policy file to define the encryption rules with the new master key.

```text
$ cat > /tmp/new_encryption_policy.pol << 'EOF'
RULE 'EncPolicyGenerator Rule' ENCRYPTION 'RULE1' IS
ALGO 'DEFAULTNISTSP800131AFAST'
KEYS ('NEW_MASTER_KEY: KP')
RULE 'Encrypt all files' SET ENCRYPTION 'RULE1' WHERE NAME LIKE '%'
```
{: codeblock}

## Applying the encryption policy
{: #apply-policy}

The `mmchpolicy` command applies and activates the updated encryption policies.

### Applying the policy to filesystem
{: #apply-policy}

```text
mmchpolicy FILE_SYSTEM /tmp/new_encryption_policy.pol
```
{: codeblock}

### Example output
{: #example-output}

```text
mmchpolicy fs new_encryption_policy.pol
Validated policy 'new_encryption_policy.pol': Parsed 2 policy rules.
Policy 'new_encryption_policy.pol' installed and broadcast to all nodes.
```
{: codeblock}

## Verifying policy application
{: #verify-policy}

1. Check the policy status using the command:

    ```text
    mmlspolicy FILE_SYSTEM -L
    ```
    {: codeblock}

2. Create a file under the filesystem using the command:

    ```text
    touch test.txt
    ```
    {: codeblock}

3. Check the encryption status of files using the command:

    ```text
    mmlsattr -n gpfs.Encryption test.txt
    ```
    {: codeblock}

The file should be encrypted with the new master key.
{: note}

## KMS Security Group Configuration
{: #kms-config}

If you are using an existing or newly created KMS instance, configure an outbound security group rule with the destination set to 0.0.0.0/0 for both the **Storage** and **Compute** security groups.

This allows the Compute and Storage nodes to communicate with the KMS endpoint, helping to prevent the `rkm_no_access (KP-eu-de.kms.cloud.ibm.com)` error.
