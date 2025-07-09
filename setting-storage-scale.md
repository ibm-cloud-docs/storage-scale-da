---

copyright:
  years: 2025
lastupdated: "2025-07-09"

keywords: architecture overview, cluster access, hpc cluster
content-type: tutorial
services: virtual-servers, vpc, loadbalancer-service
account-plan: paid
completion-time: 60m
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

---

# Setting up an IBM Storage Scale cluster
{: #using-hpc-cluster}

This section describes the process and procedure to setup an IBM Storage Scale cluster.

## Architecture overview
{: #hpc-cluster-architecture-overview}

<<Architecture overiew in brief>>

## Create SSH key
{: #hpc-ssh-key-creation-before}
{: step}

Complete the following steps to create your SSH key:

1. Generate an SSH key on your system by running the following command:

    ```pre
    ssh-keygen -t rsa
    ```
    {: pre}

2. Copy and save all the content from `.ssh/id_rsa.pub`.

## Add SSH key to the VPC infrastructure
{: #hpc-ssh-key-adding}
{: step}

1. Log in to the [{{site.data.keyword.cloud}} console](https://cloud.ibm.com/){: external} by using your unique credentials.
2. From the dashboard, click **Menu icon ![Menu icon](../icons/icon_hamburger.svg) > Infrastructure > Compute > SSH keys**.
3. Click Create.
4. Enter the SSH key name (for example, `po-ibm-ssh-key`), select the default resource group, add tags, and select the region.
5. Copy and paste the public key into the _Public key_ field (the contents that you saved from `.ssh/id_rsa.pub`).
6. Click Add SSH key.

## Create API key
{: #hpc-api-key}
{: step}

Complete the following steps to create your API key:

1. In the {{site.data.keyword.cloud_notm}} console, go to **Manage > Access (IAM) > API keys**.
2. Click **Create**.
3. Enter a name and description for your API key.
4. Click Create.
5. Then click Show to display the API key, **Copy** to copy and save it for later, or click Download.

## Create and configure an HPC cluster from the IBM Cloud catalog
{: #hpc-cluster-creation}
{: step}

Complete the following steps to create and configure an HPC cluster from the {{site.data.keyword.cloud_notm}} catalog:

1. In the {{site.data.keyword.cloud_notm}} catalog, search for _Storage Scale_, and then select IBM Storage Scale.

2. In the **Set the deployment values** section, supply the required values: << update the required values>>

3. After you confirm with the license agreement, you can use the default values for other parameters and click Install. The HPC cluster is created and completed within 40 minutes with the default configuration.

## Accessing the HPC cluster
{: #hpc-cluster-access}
{: step}

To access your HPC cluster, complete the following steps:

1. Go to Schematics > Choose the name for your workspace > Plan applied > View log.

2. Copy `ssh-command` to access your cluster.

    * `ssh -J root@ip-jumphost lsfadmin@ip-managementhost`

    * The `ip-jumphost` is `public`, while the `ip-managementhost`is not.

    * `-J flag`: Connects to the jump-host and establishes a TCP forwarding to the ultimate destination (management host).

## Auto scaling
{: #hpc-cluster-auto-scaling}
{: step}

You have a minimum number of worker nodes (`worker_node_min_count`). This is the number of worker nodes that are provisioned at the time the cluster is created. However, you can use a maximum number of worker nodes that should be added to the IBM Storage Scale cluster defined by `worker_node_max_count`. This is to limit the number of systems that can be added to IBM Storage Scale cluster when the auto scaling configuration is used. This property can be used to manage the cost associated with IBM Storage Scale cluster instance.

The following example shows `worker_node_min_count=2` and `worker_node_max_count=10`.

1. To check the two worker nodes, run the following command:

    ```pre
    bhosts -w
    ```
    {: pre}

    Example output:

    ![Two worker nodes](images/original_workernodes.png){: caption="Two worker nodes"}

2. To try the auto scaling function, run a job that requires more than two nodes. For example, this job requires five jobs to sleep for 10 seconds:

    ```pre
    bsub -n 5 -R "span[ptile=1]" sleep 10
    ```
    {: pre}

3. The job is submitted.

4. After a minute, check the nodes by running the following command:

    ```pre
    bhosts -w
    ```
    {: pre}

    You can see that now five nodes were added to your cluster:

    ![Two worker nodes](images/autoscaling.png){: caption="Five worker nodes added"}

5. The difference of nodes that are created by the auto scaling function are destroyed automatically after 10 minutes of not being used.

## Using OpenLDAP with IBM Storage Scale
{: #using-openladap-scale}
{: step}

If you want to know more about OpenLDAP with IBM Storage Scale, see [About OpenLDAP with IBM Storage Scale](/docs/storage-scale-da?topic=storage-scale-da-about-openldap).

During deployment, you enable OpenLDAP with your IBM Storage Scale cluster by setting the `enable_ldap`, `ldap_basedns`, `ldap_server`, `ldap_server_cert`, `ldap_admin_password`, `ldap_user_name`, `ldap_instance` deployment input values.

If you want to know more about integrating OpenLDAP with your IBM Storage Scale cluster, see [Integrating OpenLDAP with your IBM Storage Scale cluster](/docs/storage-scale-da?topic=storage-scale-da-integrating-openldap).

## Create DNS zones and DNS custom resolver
{: #dns-zones-custom-resolvers}
{: step}

 If you leave the `dns_instance_id` deployment input value as null, the deployment process creates a new DNS service instance ID in the respective DNS zone. Alternatively, provide an existing [IBM Cloud® DNS Service instance ID](/docs/hpc-ibm-spectrumlsf?topic=hpc-ibm-spectrumlsf-dns-custom-resolvers) for the `dns_instance_id` deployment input value.

If you leave the `dns_custom_resolver_id` deployment input value as null, the deployment process creates a new VPC and enables a new custom resolver for your cluster. Alternatively, to create custom DNS resolvers with an existing VPC, provide the resolver ID for the `dns_custom_resolver_id` deployment input value.

## Using IBM Key Protect instances and GKLM to manage data encryption
{: #key-protect-encryption}
{: step}

The Storage Scale cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. For more information, see [Enabling encryption by using GKLM](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions&interface=ui#enable-encryption-gklm) and [Enabling encryption by using IBM KeyProtect](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions#enable-encryption-keyprotect).
