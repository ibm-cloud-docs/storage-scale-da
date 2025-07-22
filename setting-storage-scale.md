---

copyright:
  years: 2025
lastupdated: "2025-07-22"

keywords: architecture overview, cluster access, storage cluster
account-plan: paid
subcollection: storage-scale-da

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}
{:step: data-tutorial-type='step'}

# Setting up an IBM Storage Scale cluster
{: #using-storage-cluster}

This section describes the process and procedure to setup an IBM Storage Scale cluster.

## Architecture overview
{: #storage-cluster-architecture-overview}

Mention brief about architecture overview.

## Create SSH key
{: #storage-ssh-key-creation-before}

Complete the following steps to create your SSH key:

1. Generate an SSH key on your system by running the following command:

    ```pre
    ssh-keygen -t rsa
    ```
    {: pre}

2. Copy and save all the content from `.ssh/id_rsa.pub`.

## Add SSH key to the VPC infrastructure
{: #storage-ssh-key-adding}

1. Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/){: external} by using your unique credentials.
2. From the dashboard, click **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > Infrastructure > Compute > SSH keys**.
3. Click **Create**.
4. Enter the SSH key name (for example, `po-ibm-ssh-key`), select the preferred resource group, add tags, and select the region.
5. Copy and paste the public key into the _Public key_ field (the contents that you saved from `.ssh/id_rsa.pub`).
6. Click **Add SSH** key.

## Create API key
{: #storage-api-key}

Complete the following steps to create your API key:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > API keys**.
2. Click **Create**.
3. Enter a name and description for your API key.
4. Click **Create**.
5. Click Show to display the API key. Copy the key and save it for later, or click **Download**.

## Using OpenLDAP with IBM Storage Scale
{: #using-openladap-scale}

If you want to know more about OpenLDAP with IBM Storage Scale, see [About OpenLDAP with IBM Storage Scale](/docs/storage-scale-da?topic=storage-scale-da-about-openldap).

During deployment, you enable OpenLDAP with your IBM Storage Scale cluster by setting the `enable_ldap`, `ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_user_password`, and `ldap_instance` deployment input values.

If you want to know more about integrating OpenLDAP with your IBM Storage Scale cluster, see [Integrating OpenLDAP with your IBM Storage Scale cluster](/docs/storage-scale-da?topic=storage-scale-da-integrating-openldap).

## Create DNS zones and DNS custom resolver
{: #dns-zones-custom-resolvers}

 If you leave the `dns_instance_id` deployment input value as null, the deployment process creates a new DNS service instance ID in the respective DNS zone. Alternatively, provide an existing [IBM Cloud® DNS Service instance ID](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-dns-custom-resolvers) for the `dns_instance_id` deployment input value.

If you leave the `dns_custom_resolver_id` deployment input value as null, the deployment process creates a new VPC and enables a new custom resolver for your cluster. Alternatively, to create custom DNS resolvers with an existing VPC, provide the resolver ID for the `dns_custom_resolver_id` deployment input value.

## Using IBM Key Protect instances and GKLM to manage data encryption
{: #key-protect-encryption}

The Storage Scale cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. For more information, see [Enabling encryption by using GKLM](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions&interface=ui#enable-encryption-gklm) and [Enabling encryption by using IBM KeyProtect](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions#enable-encryption-keyprotect).

## Create and configure an Storage cluster from the IBM Cloud catalog
{: #storage-cluster-creation}

Complete the following steps to create and configure an Storage cluster from the {{site.data.keyword.cloud_notm}} catalog:

1. In the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external}, search for _Storage Scale_, and then select IBM Storage Scale.

2. Click on **Review deployment options** on right-side and then provide the required input values: `ibm_customer_number`, `storage_gui_username`, `storage_gui_password`, `existing_resource_group`, `remote_allowed_ips`, `ssh_keys`, and `zones`.

3. After you confirm with the license agreement, you can use the default values for other parameters and click **Deploy**. The Storage cluster is created and completed within 40 minutes with the default configuration.

## Accessing the Storage cluster
{: #storage-cluster-access}

To access your Storage cluster, complete the following steps:

1. Go to **Schematics** > **Choose your workspace** > **Apply plan** > **View log**.

2. Copy the `ssh-command` to access your cluster.

    * `ssh -J ubuntu@ip-jumphost vpcuser@ip-storagehost`

    * The `ip-jumphost` is `public`, while the `ip-storagehost`is not.

    * `-J flag`: Connects to the jump-host and establishes a TCP forwarding to the ultimate destination (storage host).
