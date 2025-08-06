---

copyright:
  years: 2025
lastupdated: "2025-08-06"

keywords: deploy, storage scale
completion-time: 1h
subcollection: storage-scale-da
content-type: tutorial


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

{{site.data.keyword.attribute-definition-list}}

# Before you begin deploying
{: #before-begin-deploy}
{: toc-completion-time="1h"}
{: toc-content-type="tutorial"}

You can deploy the {{site.data.keyword.scale_full_notm}} to have a persistant storage cluster.

If you are creating Storage Scale on a persistent model (with baremetal), ensure that you have sufficient storage in your account before deploying the cluster.
{: tip}

## Confirm your {{site.data.keyword.cloud}} settings
{: #confirm-cloud-settings-scale}

Complete the following steps before you deploy the {{site.data.keyword.scale_full}}:

1. Confirm that you have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account. If you have a Trial or Lite account, [upgrade your account](/docs/account?topic=account-upgrading-account).

2. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account with your IBMid.

## Verify access policies
{: #verify-access-policies}

{{site.data.keyword.iamlong}} (IAM) access policies are required to install IBM Storage Scale using the deployable architecture and provision the clusters.

To view access policies, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, select **Manage > Access (IAM)**.
2. In the _IAM_ navigation menu, select **Users** and then select the account user.
3. Select **Access** to view the associated access policies and access groups. See the following table for the permissions that you need for this deployable architecture:

   | Service | Resources | Platform roles | Service roles |
   | ------- | --------- | ---- | ---- |
   | App configuration | All | Administrator | Manager |
   | All Identity and Access enabled services | All | Administrator | Manager |
   | Cloud Object Storage | All | Service Configuration Reader | Writer |
   | DNS Services | All | Editor | Manager |
   | Flow Logs for VPC | All | Editor | -- |
   | IAM Identity Service | All | Administrator | -- |
   | Key Protect | All | Service Configuration Reader | Manager |
   | Security and Compliance Center Workload Protection | All | Administrator | -- |
   | Virtual Private Cloud | All | Editor | -- |
   {: caption="Verify access policies" caption-side="bottom"}

## Gather Scale entitlement information
{: #gather-scale-entitlement-information}

The offering uses Bring Your Own Licenses (BYOL) for {{site.data.keyword.scale_full}} when you deploy an cluster on {{site.data.keyword.cloud_notm}}. For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the Scale cluster. Incase of failure to comply with licenses during the production use of software, is a violation of the [IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/programlicense.html){: external}.

The {{site.data.keyword.IBM_notm}} Customer Number (ICN) `ibm_customer_number` variable is used for the Bring Your Own License (BYOL) entitlement check. ICN is required if the storage type is set as Scratch or Persistent.

An ICN is not required if the `storage_type` selected is evaluation.
{: note}

## Before you begin
{: #before-begin}

The deployment is performed using Terraform and IBM Projects.
Once the necessary input values are gathered to define your cluster configuration, you are ready to deploy your {{site.data.keyword.scale_full_notm}} cluster. For more information, see [Deploying IBM Storage Scale](/docs/storage-scale-da?topic=storage-scale-da-deploying-storage-scale).
{: note}

To get started with the deployment, complete the following steps:

## Create an IBM Cloud API key
{: #create-api-key}
{: step}

Verify whether you have an {{site.data.keyword.cloud_notm}} API key. For more information, see [Creating an API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key).

## Create SSH key
{: #create-ssh-key}
{: step}

Create SSH keys in your {{site.data.keyword.cloud_notm}} account. You need to choose multiple SSH keys if you want to use different keys to access the bastion host, compute cluster, and storage cluster. Ensure that the SSH keys are present in the same resource group and region where the cluster is provisioned. The offering supports passing multiple, comma-separated SSH keys, if the cluster needs multiple SSH keys. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

You can select the required SSH key for the supported region/zone from the drop-down list.

## Set the remote_allowed_ips
{: #gather-ip-address}
{: step}

You need to provide your public IP addresses from where you want to access the environment after it is provisioned. You provide these public IP addresses in the `remote_allowed_ips` deployment value. For more information, see [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values).

## Set the storage_gui_username
{: #storage-gui-username}
{: step}

You need to provide the username to access the GUI to perform the system management and monitoring tasks on storage cluster.

## Set the storage_gui_password
{: #storage-gui-password}
{: step}

You need to provide the password to access the GUI to perform the system management and monitoring tasks on storage cluster.

## Provide the zones
{: #identify-cluster}
{: step}

Choose the {{site.data.keyword.cloud_notm}} region and availability zone where you want to deploy your cluster. You provide this location information to  configure your workspace. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

You can view or set the optional values by toggling on the **Advanced** option in the UI.
{: note}

## Enabling optional features
{: #optional-steps}

After completing the mandatory steps, you can enable the optional features by looking into the input values in the {{site.data.keyword.scale_short}} cluster. For example, to enable the encryption, you need to set the `scale_encryption_type` value.

### Enable encryption
{: #enable-encryption}

You need to decide whether you want to enable encryption for your file system. The {{site.data.keyword.scale_short}} cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. If you want to enable encryption, you need to define the `scale_encryption_xxx` deployment values when you configure your workspace. For more information about enabling encryption and configuring these deployment values, see [Enabling Encryption](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions).

### Enable parallel vNIC (MROT)
{: #enable-parallel-vnic}

As per parallel vNIC support for each node of the compute and storage cluster, a secondary vNIC comes up based on the bandwidth of a profile. According to the parallel vNIC functionality, if a VSI profile has a Bandwidth Cap (Gbps) of 64 Gbps or more, then a secondary network interface is activated.

If CES is enabled, parallel vNIC functionality cannot be used.
{: note}

### Enable CES
{: #enable-ces}

To enable CES, set `total_protocol_cluster_instances` to a value greater than zero. For more information, see [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values).

### Enable boot drive encryption for persistent storage
{: #enable-boot-encryption}

To enable boot drive encryption for persistent storage, set `bms_boot_drive_encryption` parameter to true.

### Enable LDAP
{: #enable-ldap}

To enable LDAP, set `enable_ldap` parameter to true and complete other variables such as `ldap_admin_password`, `ldap_user_name`, and `ldap_user_password`. For more information, refer to [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values). Existing LDAP is also supported.

### Enable AFM
{: #enable-afm}

To enable AFM, set `total_afm_cluster_instances` parameter to a value greater than zero. For more information, see [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values).

## Accessing the deployed environment
{: #accessing-cluster}

The values for `remote_allowed_ips` must be provided to identify a list of IP addresses of systems that can access the bastion node. All the cluster nodes can be directly accessed through bastion nodes.

**Deployer node:**

```pre
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<replace this with your bastion_node IP address> vpcuser@<replace this with your deployer_node IP address>
```

**Scale Storage node:**

```pre
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<replace this with your bastion_node IP address> vpcuser@<replace this with your storage node IP address>
```
