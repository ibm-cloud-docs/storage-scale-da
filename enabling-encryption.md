---

copyright:
  years: 2025
lastupdated: "2025-08-07"

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

# Enabling encryption
{: #enable-encryptions}

The {{site.data.keyword.scale_short}} cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. You can enable encryption features during deployment. The encryption provides highly available key servers for cryptographic operations on the GPFS file system.
{: shortdesc}

## Enabling encryption by using GKLM
{: #enable-encryption-gklm}

The following diagram is an example of a {{site.data.keyword.scale_short}} deployment with encryption enabled. The deployment includes key servers that use the GKLM application.

![Architecture diagram](images/Scale-With-SGKLM5.png){: caption="{{site.data.keyword.scale_short}} deployment with encryption" caption-side="bottom"}

By setting the `scale_encryption_type` deployment value to `gklm`, the key servers are automatically deployed and configured for encryption during the deployment.

### Before you begin
{: #before-you-begin-gklm}

Before you begin, review the following tasks:

1. To begin the deployment for the Scale cluster, refer to [Before you begin deploying](/docs/storage-scale-da?topic=storage-scale-da-before-begin-deploy) topic.
2. For more information on file system encryption using [GKLM](https://www.ibm.com/docs/en/storage-scale/5.2.3?topic=environment-simplified-setup-using-sklm-self-signed-certificate){: external}.

A minimum of two key servers are added to the cluster to avoid a single point of failure.
{: note}

### Configuring encryption deployment values
{: #configure-encryption-values-gklm}

To enable encryption on a Scale cluster, the following variables need to be defined in your workspace.

| Encryption variable | Description | Example value |
| ------------------- | ----------- | ------------- |
|`scale_encryption_enabled` | To enable the encryption for the filesystem. Select true or false. | `gklm` |
|`scale_encryption_type` | To enable filesystem encryption, specify either 'key_protect' or 'gklm'. If neither is specified, the default value will be 'null' and encryption is disabled. | `gklm` |
|`gklm_instances` | Specify the list of virtual server instances to be provisioned as ldap nodes in the cluster. Each object in the list defines the instance profile (machine type), the count (number of instances), the image (OS image to use), and an optional filesystem mount path. This configuration allows you to customize the compute tier of the cluster based on your performance and workload requirements. For more details, refer [Instance Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles&interface=ui). | [{ profile = "bx2-2x8" count  = 2 image  = "hpcc-scale-gklm4202-v2-5-3" }] |
| `scale_encryption_admin_password` | The password for administrative operations in KeyProtect or GKLM must be between 8 and 20 characters long. It must include at least three alphabetic characters (one uppercase and one lowercase), two numbers, and one special character from the set (~@_+:). The password should not contain the username. For more information, see [GKLM password policy](https://www.ibm.com/docs/en/gklm/5.x){: external}. | `xxxxxxx` |
{: caption="Encryption variables and example values" caption-side="bottom"}

After a successful cluster creation, the following resources are automatically configured to encrypt the file system:

* The key servers are deployed along with the Scale cluster.
* The admin password is updated for the GKLM application.
* An SSL certificate is created on the key server.
* Replication occurs between the primary and clone key servers.
* The key servers are added to each cluster (storage and compute).
* Tenants and clients are created on each cluster.
* The master key is created for encrypting the file system.
* An encryption policy is created and applied to the file system.

The storage and compute clusters have access to the encrypted file system, and any files that are created on the file system are encrypted.

### Verifying encryption on the file system
{: #verify-encryption-gklm}

1. Log in to any of the clusters (storage or compute nodes) by running the following SSH command:

    ```ssh
    ssh -J ubuntu@<replace this with your bastion_node IP address> vpcuser@<replace this with your storage_node IP address>
    ```
    {: codeblock}

2. List the key server that was added to the cluster by running the following command:

    ```text
    mmkeyserv server show
    ```
    {: codeblock}

3. List the key components, for example tenant or client, by running the following commands:

    ```text
    mmkeyserv tenant show
    ```
    {: codeblock}

    ```text
    mmkeyserv client show
    ```
    {: codeblock}

4. Validate the policy that was applied to the cluster by running the following command:

    ```text
    mmlspolicy FILESYSTEM_NAME -L
    ```
    {: codeblock}

5. Check the encrypted file by running the following command:

    ```text
    mmlsattr -n gpfs.Encryption FILE_NAME
    ```
    {: codeblock}


To learn more about encryption for {{site.data.keyword.scale_short}} or different encryption use cases, see [Encryption](https://www.ibm.com/docs/en/storage-scale/5.1.8?topic=administering-encryption){: external}.

### Accessing the GKLM dashboard
{: #access-gklm}

1. Open a new command line terminal.
2. Run the following commands from the local machine:

    ```pre
    ssh -L 9443:localhost:9443 -J ubuntu@<bastion_host_IP> vpcuser@<first_GKLM_server>
    ```

3. Open the browser on the local system, and run https://localhost:9443/.
4. Enter the username **SKLMAdmin** and the password that you set for `scale_encryption_admin_password` when you create your workspace to access the GKLM GUI.

## Enabling encryption by using IBM KeyProtect
{: #enable-encryption-keyprotect}

The Scale cluster file system can be encrypted using IBM® Key Protect. The IBM Key Protect helps you to provision and store encrypted keys for applications across IBM Cloud services, so that you can see and manage data encryption and the entire key lifecycle from one central location.

By setting the `scale_encryption_type` deployment value to `key_protect`, the key servers are automatically deployed and configured for encryption during the deployment.

### Before you begin
{: #before-you-begin-keyprotect}

Before you begin, review the following tasks:

1. To begin the deployment for the Scale cluster, refer to [Before you begin deploying](/docs/storage-scale-da?topic=storage-scale-da-before-begin-deploy) topic.
2. For more information on file system encryption using [IBM Key Protect](/docs/key-protect?topic=key-protect-getting-started-tutorial).

### Configuring encryption deployment values
{: #configure-encryption-values-keyprotect}

To enable encryption on a Scale cluster, the following variables need to be defined in your workspace.

| Encryption variable | Description | Example value |
| ------------------- | ----------- | ------------- |
|`scale_encryption_type` | To enable filesystem encryption, specify either `key_protect` or `gklm`. If neither is specified, the default value will be 'null' and encryption is disabled. | `key_protect` |
| `scale_encryption_admin_password` | The password for administrative operations in KeyProtect or GKLM must be between 8 and 20 characters long. It must include at least three alphabetic characters (one uppercase and one lowercase), two numbers, and one special character from the set (~@_+:). The password should not contain the username. For more information, see [GKLM password policy](https://www.ibm.com/docs/en/gklm/4.2.1). | `xxxxxxx` |
|`key_protect_instance_id`| An existing Key Protect instance used for filesystem encryption.| Null |
{: caption="Encryption variables and example values" caption-side="bottom"}

### Verifying encryption on the file system
{: #verify-encryption-keyprotect}

1. Log in to any of the clusters (storage or compute nodes) by running the following SSH command and switch to the `root` user:

    ```ssh
    ssh -J ubuntu@<replace this with your bastion_node IP address> vpcuser@<replace this with your storage_node IP address>
    sudo -i
    ```
    {: codeblock}

2. Validate the policy that was applied to the cluster by running the following command:

    ```text
    mmlspolicy FILESYSTEM_NAME -L
    ```
    {: codeblock}

3. Check the encrypted file by running the following command:

    ```text
    mmlsattr -n gpfs.Encryption FILE_NAME
    ```
    {: codeblock}

## Key Protect Instance
{: #keyprotect-instance}

IBM Storage Scale now supports both new and existing Key Protect instances for filesystem encryption.

* If the `key_protect_instance_id` is not set, a new Key Protect instance will be created automatically through our automation.

* If the `key_protect_instance_id` is provided, the existing Key Protect instance will be used for file system encryption and automation will create the key.

The existing Key Protect instance must be in the same region.
{: note}
