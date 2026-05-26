---

copyright:
  years: 2026
lastupdated: "2026-05-26"

keywords: deploy, storage scale
subcollection: storage-scale-da
content-type: tutorial

docs: https://cloud.ibm.com/docs/allowlist/storage-scale-da

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
{: toc-content-type="tutorial"}

You can deploy the {{site.data.keyword.scale_full_notm}} to have a bare-metal storage cluster.

If you are creating Storage Scale on a bare metal, ensure that you have sufficient storage in your account before deploying the cluster.
{: tip}

The Bare Metal server capacities are limited and support for only specific regions. You need to check the server capacities are available in that region. If you provide the zones that the Bare Metal does not support, then the automation fails in the planning phase with:
`error_message = "The solution supports bare metal server creation in only given availability zones i.e. us-south-1, us-south-3, us-south-2, eu-de-1, eu-de-2, eu-de-3, jp-tok-2, eu-gb-1, us-east-1, us-east-2, eu-es-3, eu-es-1, jp-tok-3, jp-tok-2, ca-tor-2 and ca-tor-3. To deploy bare-metal storage provide any one of the supported availability zones."`
{: important}

## Confirm your {{site.data.keyword.cloud}} settings
{: #confirm-cloud-settings-scale}

Complete the following steps before you deploy the {{site.data.keyword.scale_full}}:

1. Confirm that you have an {{site.data.keyword.cloud_notm}} Pay-As-You-Go or Subscription account. If you have a Trial or Lite account, [upgrade your account](/docs/account?topic=account-upgrading-account).

2. Log in to your [{{site.data.keyword.cloud_notm}}](https://cloud.ibm.com){: external} account with your IBMid.

## Prerequisites
{: #pre-req}

As an admin, you need to have the following permissions to perform the deployment:

* Administrator for All Identity and Access enabled service
* Administrator for IAM Identity Service
* Administrator for All Account Management services

## Setting IAM permissions - CLI
{: #iam-permissions-cli}

Before deploying an {{site.data.keyword.scale_full}} cluster, specific IAM permissions must be assigned to either a user or an access group. The automation script enables this process.

User has the flexibility to run the specific scripts to gain the required IAM permissions to perform the cluster deployment. The automation ensures that if the user has a certain permission, then the script omits them and add only the required permissions to perform the deployment.

For example, for the **App configuration** service, the user requires Administrator and Manager permissions. If the user already has the Administrator permission, then the script omits this and provide only Manager permission.

Benefits of the scripts:

* **Interactive input collection** - The script prompts for the IBMid (admin email), Account ID, and target (User or Access Group).
* **Permission check** - The script verifies that the admin has account-level Administrator rights, which are required to assign policies.
* **Assigns required permissions for Scale deployment** - This script grants the appropriate permissions across IBM Cloud services that Storage Scale depends upon (for example, VPC, COS, DNS services, KMS, Secrets Manager, and Sysdig Monitoring).
* **Avoids duplicates** - The script skips the assignment if a matching policy already exists.

You can get the scripts by performing **gitclone** on the branch:

```pre
git clone -b main https://github.com/terraform-ibm-modules/terraform-ibm-hpc.git
```

1. Login to the IBM Cloud CLI with your API key. Run the following command:

    ```pre
    ibmcloud login --apikey <YOUR_API_KEY> -g <RESOURCE_GROUP>
    ```
    {: codeblock}

2. Navigate to `cd tools/access-management`, you get the `permissions.sh` file.

    ```pre
    chmod +x permissions.sh
    ```
    {: codeblock}

    To update the script permissions, run `./permissions.sh` command.

3. Enter the admin email or IBMid.
4. Enter the Account ID.

    For the Account ID, login to the {{site.data.keyword.cloud_notm}} account by using your unique credentials. Go to **Manage** > **Account** > **Account settings**. You will find the Account ID.
5. You are asked to assign the roles:
    1. **Access Group** - Select this option, if you want to assign the access to the entire access group.
    2. **User** - Select this option, if you want to assign the access to an individual user.

    Select the required option.
6. Enter the target user email, if you select the option 2.
7. User policy is successfully created.

If the user skips to enter the `ACCOUNT_ID`, then script displays the error message:

```pre
:x: ACCOUNT_ID is required.
```

This script ensures the user or access group has all the required IAM permissions to successfully deploy a Storage Scale environment.

## Setting IAM permissions - UI
{: #iam-permissions-ui}

{{site.data.keyword.iamlong}} (IAM) access policies are required to install this deployable architecture and provision clusters.

To view access policies, complete the following steps:

1. In the {{site.data.keyword.cloud_notm}} console, select **Manage > Access (IAM)**.
2. In the _IAM_ navigation menu, select **Users** and then select the account user.
3. Select **Access** to view the associated access policies and access groups. See the following table for the permissions that you need for this deployable architecture:

   | Service | Resources | Platform roles | Service roles |
   | ------- | --------- | ---- | ---- |
   | App configuration | All | Administrator | Manager |
   | All Identity and Access enabled services | All | Administrator | Manager |
   | All Account Management services | All | Administrator | -- |
   | Cloud Object Storage | All | Service Configuration Reader | Writer |
   | DNS Services | All | Editor | Manager |
   | IAM Identity Service | All | Administrator | -- |
   | IBM Key Protect | All | Service Configuration Reader | Manager |
   | Security and Compliance Center Workload Protection | All | Administrator | -- |
   | Secrets Manager | All | Administrator | Manager |
   | VPC Infrastructure Services | All | Administrator | -- |
   {: caption="Verify access policies" caption-side="bottom"}

   The above-mentioned permissions are mandatory, failing to have these permissions lead to deployment failure. Contact the account administrator for the permissions.
   {: tip}

## Before you begin
{: #before-begin}

The deployment is performed using Terraform and IBM Projects.
Once the necessary input values are gathered to define your cluster configuration, you are ready to deploy your {{site.data.keyword.scale_full_notm}} cluster. For more information, see [Deploying IBM Storage Scale](/docs/allowlist/storage-scale-da?topic=storage-scale-da-deploying-storage-scale).
{: note}

To get started with the deployment, complete the following steps:

## Create an IBM Cloud API key
{: #create-api-key}
{: step}

Verify whether you have an {{site.data.keyword.cloud_notm}} API key. `ibmcloud_api_key` is the value required for this variable. For more information, see [Creating an API key](/docs/account?topic=account-userapikey&interface=ui#create_user_key).

## Create SSH key
{: #create-ssh-key}
{: step}

Create SSH keys in your {{site.data.keyword.cloud_notm}} account. If you want to use multiple SSH keys to access the bastion host, compute cluster, and storage cluster, ensure that the SSH keys are present in the same resource group and region where the cluster is provisioned. You can select the required SSH key for the supported region/zone from the drop-down list. `ssh_keys` is the value required for this variable. For more information, see [Managing SSH keys](/docs/vpc?topic=vpc-managing-ssh-keys).

In the UI, the drop-down lists all the available ssh keys from all the regions. If you have a similar name across all the region and you click the drop down, then all the keys are selected but the right SSH key will be picked only in the back-end based upon the input provided for the zones.

## Set the remote_allowed_ips
{: #gather-ip-address}
{: step}

You need to provide your public IP addresses from where you want to access the environment after it is provisioned. You provide these public IP addresses in the `remote_allowed_ips` deployment value. For more information, see [Deployment values](/docs/allowlist/storage-scale-da?topic=storage-scale-da-deployment-values).

If this field is left empty (for example, [""]) or not provided, then the cluster deployment fails during the initial setup phase. It is essential to supply a valid entry to proceed with a successful deployment.

## Set the storage_gui_username
{: #storage-gui-username}
{: step}

You need to provide the username to access the GUI to perform the system management and monitoring tasks on the storage cluster. `storage_gui_username` is the value required for this variable.

## Set the storage_gui_password
{: #storage-gui-password}
{: step}

You need to provide the password to access the GUI to perform the system management and monitoring tasks on the storage cluster. `storage_gui_password` is the value required for this variable.

## Provide the zones
{: #identify-cluster}
{: step}

Choose the {{site.data.keyword.cloud_notm}} region and availability zone where you want to deploy your cluster. You provide this location information to configure your workspace. `zones` is the value required for this variable. For more information, see [Region and data center locations for resource deployment](/docs/overview?topic=overview-locations).

You can view or set the optional values by toggling the **Advanced** option in the UI.
{: note}

## Enabling optional features
{: #optional-steps}

After completing the mandatory steps, you can enable the optional features by looking into the input values in the {{site.data.keyword.scale_short}} cluster. For example, to enable the encryption, you need to set the `scale_encryption_type` value.

### Enable IBM Customer Number (ICN)
{: #gather-scale-entitlement-information}

The offering uses Bring Your Own License (BYOL) for {{site.data.keyword.scale_full}} when deploying a cluster on {{site.data.keyword.cloud_notm}}. The IBM Storage Scale solution is installed with the **Data Management Edition**. For more information about this edition, see [Features in IBM Storage Scale editions](https://www.ibm.com/docs/en/storage-scale/6.0.0?topic=overview-storage-scale-product-editions#prodstruct__table_atn_tqp_rhb) table. Users may need to have the appropriate entitlement to use this version of the software. For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the Scale cluster. In case of failure to comply with licenses during the production use of software is a violation of the [IBM International Program License Agreement](https://www.ibm.com/software/passportadvantage/licensing){: external}.

### Enable encryption
{: #enable-encryption}

You need to decide whether you want to enable encryption for your file system. The {{site.data.keyword.scale_short}} cluster file system can be encrypted by using the IBM Security® Guardium® Key Lifecycle Manager (GKLM) or the IBM KeyProtect. If you want to enable encryption, you need to define the `scale_encryption_type` deployment values when you configure your workspace. For more information about enabling encryption and configuring these deployment values, see [Enabling Encryption](/docs/allowlist/storage-scale-da?topic=storage-scale-da-enable-encryptions).

### Enable parallel vNIC (MROT)
{: #enable-parallel-vnic}

As per parallel vNIC support for each node of the compute and storage cluster, a secondary vNIC comes up based on the bandwidth of a profile. According to the parallel vNIC functionality, if a VSI profile has a Bandwidth Cap (Gbps) of 64 Gbps or more, then a secondary network interface is activated.

If CES is enabled, parallel vNIC functionality cannot be used.
{: note}

### Enable CES
{: #enable-ces}

To enable CES, set `protocol_instances` to a value greater than zero. For more information, see [Deployment values](/docs/allowlist/storage-scale-da?topic=storage-scale-da-deployment-values).

### Enable boot drive encryption for bare-metal storage
{: #enable-boot-encryption}

To enable boot drive encryption for bare-metal storage, set `bms_boot_drive_encryption` parameter to true.

### Enable LDAP
{: #enable-ldap}

To enable LDAP, set `enable_ldap` parameter to true and complete other variables such as `ldap_admin_password`, `ldap_user_name`, and `ldap_user_password`. Existing LDAP is also supported. For more information, see [Deployment values](/docs/allowlist/storage-scale-da?topic=storage-scale-da-deployment-values).

### Enable AFM
{: #enable-afm}

To enable AFM, set `afm_instances` parameter to a value greater than zero. For more information, see [Deployment values](/docs/allowlist/storage-scale-da?topic=storage-scale-da-deployment-values).

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

After cluster deployment, executing any scale commands requires root access to the solution nodes. However, when these commands are run using `sudo su -`, the following errors are encountered:
```pre
[vpcuser@scale-strg-8017-001 ~]$ mmgetstate -a
-bash: /usr/lpp/mmfs/bin/mmces: Permission denied
```
To execute any scale commands, switch to the root user using `sudo su`.
{: note}
