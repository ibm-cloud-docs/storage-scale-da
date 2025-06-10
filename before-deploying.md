---

copyright:
  years: 2025
lastupdated: "2025-06-10"

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

# Before you begin deploying
{: #before-begin-deploy}

With {{site.data.keyword.scale_full}}, you can deploy Scale clusters that use {{site.data.keyword.scale_full_notm}} as a storage solution. The deployment is performed by using Terraform and {{site.data.keyword.bplong_notm}} as automation frameworks.

## Confirm your {{site.data.keyword.cloud}} settings
{: #confirm-cloud-settings-scale}

Complete the following steps before you deploy the {{site.data.keyword.scale_full}}:

1. Confirm that you have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account. If you have a Trial or Lite account, [upgrade your account](/docs/account?topic=account-upgrading-account).

2. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account with your IBMid.

## Verify access policies
{: #verify-access-policies}

{{site.data.keyword.iamlong}} (IAM) access policies are required to install this deployable architecture and provision clusters.

To view access policies, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, select **Manage > Access (IAM)**.
2. In the _IAM_ navigation menu, select **Users** and then select the account user.
3. Select **Access** to view the associated access policies and access groups. See the following table for the permissions that you need for this deployable architecture:

   | Service | Resources | Role |
   | ------- | --------- | ---- |
   | All IAM Account Management services| All | Editor, Operator, Service ID creator, VPN Administrator, User API key creator, API key reviewer |
   | Resource group only | Deployment can be done from any resource group. Ensure that the resource group is enabled. | Editor, Viewer |
   | Schematics | All | Manager, Editor |
   | DNS Services | All | Manager, Editor |
   | Key Protect | All | Manager, Editor |
   | Cloud Object Storage | All | Writer, Editor |
   | All Identity and Access enabled services | All | Editor, Operator, Service ID creator, VPN Administrator, User API key creator, API key reviewer |
   | VPC Infrastructure Services | All | Writer, Editor, Bare Metal Advanced Network Operator, Bare Metal Console Admin, IP Spoofing Operator |
   {: caption="Verify access policies" caption-side="bottom"}

## Allow access to {{site.data.keyword.cloud_notm}} public endpoints
{: #public-endpoints}

The {{site.data.keyword.scale_full}} requires access to the following {{site.data.keyword.cloud_notm}} service API public endpoints. For a successful deployment to provision the infrastructure and the associated services, ensure that you are aware of these endpoints and allow them access:

| Endpoint | Type | Notes |
   | ------- | --------- | ---- |
   | `iam.cloud.ibm.com` | IAM | The IAM endpoint is protected by Akamai under the [Akamai IP ranges](https://techdocs.akamai.com/origin-ip-acl/docs/update-your-origin-server){: external} |
{: caption="{{site.data.keyword.cloud_notm}} public endpoints required for {{site.data.keyword.cloud_notm}} Scale deployment" caption-side="bottom"}

## Gather Scale entitlement information
{: #gather-scale-entitlement-information}

The offering uses Bring Your Own Licenses (BYOL) for {{site.data.keyword.scale_full}} when you deploy an cluster on {{site.data.keyword.cloud_notm}}. For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the Scale cluster by using {{site.data.keyword.scale_full}}. Failure to comply with licenses for production use of software is a violation of the [IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/programlicense.html){: external}.

Before you can deploy your {{site.data.keyword.scale_full}}, you need to create or gather some information. To get started, complete the following steps:

## Create an IBM Cloud API key
{: #create-api-key}
{: step}

Verify that you have an {{site.data.keyword.cloud_notm}} API key. For more information, see [Creating an API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key).

## Create SSH key
{: #create-ssh-key}
{: step}

Create SSH keys in your {{site.data.keyword.cloud_notm}} account. You might need multiple SSH keys if you want to use different keys to access the bastion host, compute cluster, and storage cluster. Ensure that the SSH keys are present in the same resource group and region where the cluster is provisioned. The offering supports passing multiple, comma-separated SSH keys, if the cluster needs multiple SSH keys. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

## Choose between IBM-managed or user-managed encryption
{: #encryption}
{: step}

By default, VPC volumes and file shares are encrypted with IBM-managed encryption. However, you can opt for user-managed encryption per your security requirements. Customer-managed encryption uses your root key, which gives you complete control over your data. You can provision or import existing encrypted keys by using {{site.data.keyword.keymanagementservicefull_notm}}.

If you decide to use user-managed encryption, complete the following steps before you deploy your {{site.data.keyword.scale_full}} architecture:

1. [Provision an instance of Key Protect](/docs/key-protect?topic=key-protect-provision#provision-gui)
2. [Create or import key](/docs/key-protect?topic=key-protect-getting-started-tutorial#get-started-keys)
3. [Authorize access between](/docs/vpc?topic=vpc-vpc-encryption-planning#byok-volumes-prereqs):
    * Cloud Block Storage and the key management service
    * File Storage for VPC and the key management service
4. Gather information for the following boot volume encryption deployment values (you provide this information when you deploy your {{site.data.keyword.scale_full}} architecture):
    * `enable_customer_managed_encryption`: Gives you toggling options.
    * `kms_instance_id`: Instance ID of the Key Protect instance that you create.
    * `kms_key_name`: Name of the KMS key that you create.

### Create custom image
{: #create-custom-image}
{: step}

You can use the default image or create a custom image for compute, storage, and client nodes. But for bootstrap, GKLM (if encryption is enabled) and LDAP (if LDAP is enabled), only the default image is supported. For more information, see [Planning for custom images](/docs/vpc?topic=vpc-planning-custom-images).

Stock image is not supported, when parallel vNIC and CES features are enabled.
{: note}

{{site.data.keyword.cloud_notm}} provides pre-built images with RHEL to help you get started quickly. See the `storage_vsi_osimage_name`, `storage_bare_metal_osimage_name` and `compute_vsi_osimage_name` parameter in [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values). In addition to the base operating system, the image includes the {{site.data.keyword.scale_short}} software packages that allow for the {{site.data.keyword.scale_short}} shared file system to be automatically mounted and ready for use after the creation and configuration of the cluster is complete.

### Gather public IP address
{: #gather-ip-address}
{: step}

You need to provide your public IP addresses from where you want to access the environment after it is provisioned. You provide these public IP addresses in the `remote_cidr_blocks` deployment value. For more information, see [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values).

### Identify cluster deployment location
{: #identify-cluster}
{: step}

You need to decide where you want your cluster that is deployed by choosing an {{site.data.keyword.cloud_notm}} region and availability zone. You provide this location information when you configure your workspace. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

## Enable optional features in the deployment values
{: #optional-steps}

After completing the mandatory steps, you can enable the optional parameters in deployment values in the {{site.data.keyword.scale_short}} cluster:

### Enable encryption
{: #enable-encryption}
{: step}

You need to decide whether you want to enable encryption for your file system. The {{site.data.keyword.scale_short}} cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. If you want to enable encryption, you need to define the `scale_encryption_xxx` deployment values when you configure your workspace. For more information about enabling encryption and configuring these deployment values, see [Enabling Encryption](/docs/storage-scale-da?topic=storage-scale-da-enable-encryptions).

### Enable parallel vNIC (MROT)
{: #enable-parallel-vnic}
{: step}

As per parallel vNIC support for each node of the compute and storage cluster, a secondary vNIC comes up based on the bandwidth of a profile. According to the parallel vNIC functionality, if a VSI profile has a Bandwidth Cap (Gbps) of 64 Gbps or more, then a secondary network interface is activated.

If CES is enabled, parallel vNIC functionality cannot be used.
{: note}

### Enable CES
{: #enable-ces}
{: ces}

To enable CES, set `total_protocol_cluster_instances` to a value greater than zero. Refer to [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values) topic for more details.

### Enable boot drive encryption for persistent storage
{: #enable-boot-encryption}
{: step}

To enable boot drive encryption for persistent storage, set `bms_boot_drive_encryption` parameter to true.

### Enable LDAP
{: #enable-ldap}
{: step}

To enable LDAP, set `enable_ldap` parameter to true and complete other variables such as `ldap_admin_password`, `ldap_user_name`, and `ldap_user_password`. For more information, refer to [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values). Existing LDAP is also supported.

### Enable AFM
{: #enable-afm}
{: step}

To enable AFM, set `total_afm_cluster_instances` parameter to a value greater than zero. For more information, refer to [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values).

## Next steps
{: #getting-started-next-steps}

Once the necessary input values are gathered to define your cluster configuration, you are ready to deploy your {{site.data.keyword.scale_full_notm}} cluster. The {{site.data.keyword.scale_short}} cluster can be deployed on {{site.data.keyword.cloud_notm}} by using the {{site.data.keyword.cloud_notm}} catalog tile, {{site.data.keyword.bpshort}} UI, {{site.data.keyword.bpshort}} CLI, or the {{site.data.keyword.bpshort}} APIs. If you want to deploy your cluster by using the CLI or API, review the prerequisites for your interface of choice:

* [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/storage-scale?topic=storage-scale-setting-up-cli)
* [Setting up the {{site.data.keyword.bplong_notm}} API](/docs/storage-scale?topic=storage-scale-setting-up-api)

After you have created and reviewed for any additional prerequisites for your interface, perform the following:

1. **Create a workspace** on {{site.data.keyword.bplong_notm}} that uses the [Terraform code](https://github.com/IBM/ibm-spectrum-scale-ibm-cloud-schematics){: external} that is developed for this offering. This step defines the set of configuration properties that are used to perform the automation. For more information, see [Creating a workspace](/docs/storage-scale?topic=storage-scale-creating-workspace).

2. **Generate a plan** to confirm whether the configuration properties are valid, so that when you run the Terraform code, all of the resources are provisioned correctly. If the validation fails, fix the configuration properties and try again.

3. **Apply a plan** triggers the actual deployment of the {{site.data.keyword.cloud_notm}} resources to have an Scale cluster up and running by the time the deployment completes. If the deployment fails, identify the reason for failure, fix the problem, and try again. If a change is needed to the configuration properties, it might be better to generate a plan again.

If instead of using {{site.data.keyword.bplong_notm}} you decide to deploy your {{site.data.keyword.scale_full_notm}} cluster through the {{site.data.keyword.cloud_notm}} catalog, when you click **Install**, the **Generate plan** action is skipped, and the steps go from **Create workspace** to **Apply plan** directly. You need to enter values in the catalog that work for your permissions and {{site.data.keyword.cloud_notm}} account. If the deployment fails, the {{site.data.keyword.bpshort}} UI can be used to fix the errors, and you can retry the **Apply Plan** step.
{: note}
