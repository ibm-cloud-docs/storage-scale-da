---

copyright:
  years: 2025
lastupdated: "2025-08-25"

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
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Enabling cluster export services
{: #config-ces-integration-ldap-authentication}

Cluster Export Services (CES) is a key component of the {{site.data.keyword.scale_full_notm}} architecture, which is designed to enable access to data stored in the Scale cluster and Object Storage (Storage Scale) system. CES plays a critical role in providing efficient and versatile data access to meet the diverse needs of modern enterprises. This flexibility allows organizations to support a wide range of applications and its use cases.

Enabling colocation designates the subset of Storage server as protocol nodes. If disabled, protocol nodes are created on dedicated virtual servers or bare metal, depending on the specified protocol server profile.

The colocation feature avoids the need to provision extra virtual servers and improves the performance. It is also supported on Bare Metal servers.

![Architecture diagram.](images/scale-ces.png){: caption="CES node on Storage Scale" caption-side="bottom"}

## Before you begin
{: #beforeyoubegin-config-ces}

Before you begin, review the following information:

1. To begin with the Scale cluster deployment, see [Before you begin deploying](/docs/storage-scale-da?topic=storage-scale-da-before-begin-deploy) topic.

2. For more information on cluster export service see, [how CES works](/docs/storage-scale-da?topic=storage-scale-da-config-ces-integration-ldap-authentication&interface=ui#verify-ces) topic.

## Configuring CES deployment
{: #procedureconfig-ces-deploy}

To enable the CES feature on a Scale cluster, the following variables need to be defined in your workspace.

|CES Variable|	Description|	Example value|
|-------------|------------|--------------|
|`protocol_subnets_cidr`| Provide the CIDR block required for the creation of the protocol private subnet. Single CIDR block is required. If using a hybrid environment, modify the CIDR block to avoid conflicts with any on-premises CIDR blocks. Ensure the selected CIDR block size can accommodate the maximum number of protocol nodes expected in your cluster. For more information on CIDR block size selection, see [Choosing IP ranges for your VPC](/docs/vpc?topic=vpc-choosing-ip-ranges-for-your-vpc). | "10.241.40.0/24" |
|`protocol_instances`| Specify the list of virtual server instances to be provisioned as protocol nodes in the cluster. Each object in the list includes the instance profile (machine type), the count (number of instances), the image (OS image to use). This configuration allows allows for a unified data management solution, enabling different clients to access the same data using NFS protocol.This input can be used to provision virtual server instances (VSI). If persistent, high-throughput storage is required, consider using bare metal instances instead. Ensure you provide valid instance profiles. Maximum of 32 VSI or baremetal nodes are supported. For more information, see [Instance Profiles](/docs/vpc?topic=vpc-profiles&interface=ui). |[{profile = "cx2-32x64" count  = 2 image  = "hpcc-scale5232-rhel810-v1"}]|
|`filesets_config`| Specify a list of filesets with client mount paths and optional storage quotas (0 means no quota) to be created within the IBM Storage Scale filesystem. | [{client_mount_path = "/mnt/scale/tools" quota  = 0},{client_mount_path = "/mnt/scale/data" quota  = 0}] |
|`client_instances`	| Specify the list of virtual server instances to be provisioned as client nodes in the cluster. Each object includes the instance profile (machine type), number of instances (count), OS image to use. This configuration allows customization of the compute tier to suit specific performance and workload requirements. The profile must match a valid IBM Cloud VPC Gen2 instance profile format. For more information, see [Instance Profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-profiles&interface=ui). | [{profile = "cx2-2x4" count  = 0 image  = "ibm-redhat-8-10-minimal-amd64-6"}] |
|`colocate_protocol_instances`| Enable this option to colocate protocol services on the same virtual server instances used for storage. When set to true, the storage nodes will also act as protocol nodes for reducing the need for separate infrastructure. This can optimize resource usage and simplify the cluster setup, especially for smaller environments or cost-sensitive deployments. For larger or performance-intensive workloads, consider deploying dedicated protocol instances instead. | true |
{: caption='CES Variables'}

The successful scale deployment with the CES feature enabled consists of different clusters:

* Storage cluster with defined storage and CES protocol nodes.
* Client cluster with defined client nodes that mounts file shares that are exported by protocol nodes with NFS protocol.
* **(Optional)** Compute cluster with defined compute nodes.

## Verifying CES on the file system
{: #verify-ces}

1. Log in to any of the clusters (storage or compute nodes) by running the following SSH command:

    ```pre
    ssh -J root@BASTION_SERVER vpcuser@STORAGE_NODE
    ```

2. To view the cluster shared root configuration on the storage cluster, run the following command:

    ```pre
    mmlsconfig cesSharedRoot
    ```

3. To list the protocol nodes in the cluster, run the following command:

    ```pre
    mmces node list
    ```

4. To view the protocol cluster information, use the mmlscluster command:

    ```pre
    mmlscluster --ces
    ```

5. Use the service list command that provides comprehensive list of the services that are running in the CES cluster, use --verbose and -a flag for detailed information:

    ```pre
    mmces service list --verbose -a
    ```

6. Use the mmuserauth command to view the details on the type of authentication used for CES:

    ```pre
    mmuserauth service check
    ```

7. Use the mmnfs export command to add, change, list, load, or remove NFS export declarations for IP addresses on nodes that are configured as CES types. Use list to view the current NFS exports:

    ```pre
    mmnfs export list
    ```

8. Use the mmlsquota command to display quota information for a user, group, or file set. The -j flag is used for displaying the quota for file set in a file system.

    ```pre
    mmlsquota -j data FILESYSTEM
    ```

The CES feature is only available with the custom image that is provided by the solution.
{: note}
