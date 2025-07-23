---

copyright:
  years: 2025
lastupdated: "2025-07-23"

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
{:table: .aria-labeledby="caption"}

# Deployment values
{: #deployment-values}

The following deployment values can be used to configure the {{site.data.keyword.scale_short}} cluster instance on {{site.data.keyword.cloud}}.

## Mandatory deployment values
{: #mandate-values}

The following are the mandatory deployment values used to configure the {{site.data.keyword.scale_short}} cluster instance on {{site.data.keyword.cloud}}:

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `ibm_customer_number` | Comma-separated list of the IBM Customer Number(s) (ICN) that is used for the Bring Your Own License (BYOL) entitlement check. For more information on how to find your ICN, see [What is my IBM Customer Number (ICN)?](https://www.ibm.com/support/pages/what-my-ibm-customer-number-icn). | Yes | Null |
| `ibmcloud_api_key` | IBM Cloud API Key will be used for authentication in scripts run in this module. Only required if certain options are required. | Yes | Null |
| `storage_gui_username` | Specify the storage cluster GUI username to perform system management and monitoring tasks. | Yes | "" |
| `storage_gui_password` | Specify the password for storage cluster GUI. | Yes | "" |
| `existing_resource_group` | Resource group name from your IBM Cloud account where the VPC resources must be deployed. For more information, see [Managing resource groups](/docs/account?topic=account-rgs&interface=ui). The strings describe the resource groups to create or reference. | Yes | Default |
| `remote_allowed_ips` | Comma-separated list of IP addresses that can access the IBM Spectrum LSF cluster instance through an SSH interface. For security purposes, provide the public IP addresses assigned to the devices that are authorized to establish SSH connections (for example, [\"169.45.117.34\"]). To fetch the IP address of the device, use [https://ipv4.icanhazip.com/](https://ipv4.icanhazip.com/). | Yes | None |
| `ssh_keys` | Provide the list of SSH key names already configured in your IBM Cloud account to establish a connection to the Storage Scale cluster. For more information, see [SSH Keys](https://cloud.ibm.com/docs/vpc?topic=vpc-ssh-keys&interface=ui).| Yes | Null |
| `zones` | Specify the IBM Cloud zone within the chosen region where the IBM Spectrum LSF cluster will be deployed. A single zone input is required, and the management nodes, file storage shares, and compute nodes will all be provisioned in this zone.[Learn more](https://cloud.ibm.com/docs/vpc?topic=vpc-creating-a-vpc-in-a-different-region#get-zones-using-the-cli). | Yes | ["us-east-1"] |
{: caption="Mandatory deployment values" caption-side="top"}

## Optional deployment values
{: #optional-values}

The following are the optional deployment values used to configure the {{site.data.keyword.scale_short}} cluster instance on {{site.data.keyword.cloud}}:

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `cluster_prefix` | A unique identifier for resources. Must begin with a letter and end with a letter or number. This cluster_prefix will be prepended to any resources provisioned by this template. Prefixes must be 16 or fewer characters.| No | scale |
| `vpc_name` | Provide the name of an existing VPC in which the cluster resources will be deployed. If no value is given, solution provisions a new VPC. [Learn more](https://cloud.ibm.com/docs/vpc). | Yes | Null|
| `vpc_cidr` | An address prefix is created for the new VPC when the `vpc_name` variable is set to null. This prefix is required to provision subnets within a single zone, and the subnets will be created using the specified CIDR blocks. For more information, see [Setting IP ranges](https://cloud.ibm.com/docs/vpc?topic=vpc-vpc-addressing-plan-design). | No | "10.241.0.0/18" |
| `placement_strategy` | VPC placement groups to create (null / host_spread / power_spread) | No | Null |
| `bastion_instance` | Configuration for the Bastion node, including the image and instance profile. Only Ubuntu stock images are supported. | No | {image = "ibm-ubuntu-22-04-5-minimal-amd64-3" profile = "cx2-4x8"} |
| `login_subnets_cidr` | Provide the CIDR block required for the creation of the login cluster's private subnet. Only one CIDR block is needed. If using a hybrid environment, modify the CIDR block to avoid conflicts with any on-premises CIDR blocks. Since the login subnet is used only for the creation of login virtual server instances, provide a CIDR range of /28. | No | "10.241.16.0/28" |
| `deployer_instance` | Configuration for the deployer node, including the custom image and instance profile. By default, uses fixpack_15 image and a bx2-8x32 profile. | No | {image = "test-deployer-instance-image" profile = "mx2-4x32"} |
| `client_subnets_cidr` | Subnet CIDR block to launch the client host. | No | "10.241.50.0/24" |
| `client_instances` | Number of instances to be launched for client. | No | [{ profile = "cx2-2x4" count   = 2 image   = "ibm-redhat-8-10-minimal-amd64-4" }] |
| `compute_subnets_cidr` | Provide the CIDR block required for the creation of the compute cluster's private subnet. One CIDR block is required. If using a hybrid environment, modify the CIDR block to avoid conflicts with any on-premises CIDR blocks. Ensure the selected CIDR block size can accommodate the maximum number of management and dynamic compute nodes expected in your cluster. For more information on CIDR block size selection, refer to the documentation, see [Choosing IP ranges for your VPC](https://cloud.ibm.com/docs/vpc?topic=vpc-choosing-ip-ranges-for-your-vpc). | No | "10.241.0.0/20" |
| `compute_gui_username` | GUI user to perform system management and monitoring tasks on compute cluster. | No | Null |
| `compute_gui_password` | Password for compute cluster GUI | No | Null |
| `storage_subnets_cidr` | Subnet CIDR block to launch the storage cluster host. | No | "10.241.30.0/24" |
| `storage_instances` | Number of instances to be launched for storage cluster. | No | [{ profile    = "bx2d-32x128" count      = 2 image      = "ibm-redhat-8-10-minimal-amd64-4" filesystem = "/ibm/fs1" }]
| `storage_servers` | Number of BareMetal Servers to be launched for storage cluster. | No | [{ profile    = "cx2d-metal-96x192" count = 0 image  = "ibm-redhat-8-10-minimal-amd64-4" filesystem = "/gpfs/fs1" }]
| `tie_breaker_bm_server_profile` | Specify the bare metal server profile type name to be used for creating the bare metal Tie breaker node. If no value is provided, the storage bare metal server profile will be used as the default. For more information, see [bare metal server profiles](https://cloud.ibm.com/docs/vpc?topic=vpc-bare-metal-servers-profile&interface=ui). [Tie Breaker Node](https://www.ibm.com/docs/en/storage-scale/5.2.2?topic=quorum-node-tiebreaker-disks) | No | Null |
| `protocol_subnets_cidr` | Subnet CIDR block to launch the storage cluster host. | No | "10.241.40.0/24" |
| `protocol_instances` | Number of instances to be launched for protocol hosts. | No | [{ profile = "bx2-2x8" count = 2 image = "ibm-redhat-8-10-minimal-amd64-4" }] |
| `colocate_protocol_instances` | Enable it to use storage instances as protocol instances. | No | True |
| `filesystem_config` | File system configurations | No | [{ filesystem  = "/ibm/fs1" block_size  = "4M" default_data_replica  = 2 default_metadata_replica = 2 max_data_replica  = 3 max_metadata_replica  = 3 }] |
| `filesets_config` | Fileset configurations | No | [{ client_mount_path = "/mnt/scale/tools"
quota  = 0}, { client_mount_path = "/mnt/scale/data" quota  = 0}] |
| `afm_instances` | Number of instances to be launched for afm hosts. | No | [{profile = "bx2-2x8" count  = 0 image  = "ibm-redhat-8-10-minimal-amd64-4" }] |
| `afm_cos_config` | AFM configurations | No | [{ afm_fileset  = "afm_fileset" mode  = "iw" cos_instance  = "" bucket_name  = "" bucket_region  = "us-south" cos_service_cred_key = "" bucket_storage_class = "smart" bucket_type  = "region_location" }] |
| `dns_instance_id` | IBM Cloud HPC DNS service instance ID. | No | Null |
| `dns_custom_resolver_id`| IBM Cloud DNS custom resolver ID. | No | Null |
| `dns_domain_names` | IBM Cloud HPC DNS domain names. | No | { compute  = "comp.com" storage  = "strg.com" protocol = "ces.com" client  = "clnt.com" gklm  = "gklm.com"} |
| `enable_cos_integration` | Integrate COS with HPC solution | No | True |
| `cos_instance_name` | Exiting COS instance name | No | Null |
| `enable_vpc_flow_logs` | Enable Activity tracker | No | True |
| `override` | Override default values with custom JSON template. This uses the file `override.json` to allow users to create a fully customized environment. | No | False |
| `override_json_string` | Override default values with a JSON object. Any JSON other than an empty string overrides other configuration changes. | No | Null |
| `enable_ldap` | Set this option to true to enable LDAP for {{site.data.keyword.cloud_notm}} HPC, with the default value set to false. | No | false |
| `ldap_basedns` | The dns domain name is used for configuring the LDAP server. If an LDAP server is already in existence, ensure to provide the associated DNS domain name.	|  No  | "ldapscale.com" |
| `ldap_server` |  Provide the IP address for the existing LDAP server. If no address is given, a new LDAP server is created. | No | Null|
|`ldap_server_cert` |Provide the existing LDAP server certificate. This value is required if the `ldap_server` variable is not set to null. If the certificate is not provided or is invalid, the LDAP configuration may fail. For more information on how to create or obtain the certificate, refer to [Enabling OpenLDAP service](https://cloud.ibm.com/docs/storage-scale?topic=storage-scale-enable-openldap).| No  | Null |
| `ldap_admin_password` | The LDAP administrative password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required. It is important to avoid including the username in the password for enhanced security. | Yes | Null |
| `ldap_user_name` | Custom LDAP User for performing cluster operations. Note: Username should be between 4 to 32 characters, (any combination of lowercase and uppercase letters).[This value is ignored for an existing LDAP server] | No | ""|
| `ldap_user_password` | The LDAP user password should be 8 to 20 characters long, with a mix of at least three alphabetic characters, including one uppercase and one lowercase letter. It must also include two numerical digits and at least one special character from (~@_+:) are required.It is important to avoid including the username in the password for enhanced security.[This value is ignored for an existing LDAP server]. | Yes | "" |
| `ldap_instance` | Profile and Image name to be used for provisioning the LDAP instances. Note: Debian based OS are only supported for the LDAP feature. | No | [{ profile = "cx2-2x4" image  = "ibm-ubuntu-22-04-5-minimal-amd64-1" }]
| `scale_encryption_enabled` | To enable the encryption for the filesystem. Select true or false | No | False |
|`scale_encryption_type` | To enable filesystem encryption, specify either 'key_protect' or 'gklm'. If neither is specified, the default value will be 'null' and encryption is disabled. | No | Null |
| `gklm_instances` | Number of GKLM instances to be launched for scale cluster. | No | [{ profile = "bx2-2x8" count   = 2 image  = "hpcc-scale-gklm4202-v2-5-2" }] |
| `scale_encryption_admin_password` | Password that is used for performing administrative operations for the GKLM.The password must contain at least 8 characters and at most 20 characters. For a strong password, at least three alphabetic characters are required, with at least one uppercase and one lowercase letter.  Two numbers, and at least one special character from this(~@_+:). Make sure that the password doesn't include the username. Visit this [page](https://www.ibm.com/docs/en/gklm/3.0.1?topic=roles-password-policy){: external}  to know more about password policy of GKLM.| Yes | Null |
| `key_protect_instance_id` | An existing Key Protect instance used for filesystem encryption. | No | Null |
| `storage_type` | Select the {{site.data.keyword.scale_short}} file system deployment method. The {{site.data.keyword.scale_short}} scratch and evaluation types deploy the {{site.data.keyword.scale_short}} file system on virtual server instances, and the persistent type deploys the {{site.data.keyword.scale_short}} file system on bare metal servers. | No | scratch |
| `observability_atracker_enable` | Activity Tracker Event Routing to configure how to route auditing events. While multiple Activity Tracker instances can be created, only one tracker is needed to capture all events. Creating additional trackers is unnecessary if an existing Activity Tracker is already integrated with a COS bucket. In such cases, set the value to false, as all events can be monitored and accessed through the existing Activity Tracker. | No | False |
| `observability_atracker_target_type` | All the events will be stored in either COS bucket or Cloud Logs on the basis of user input, so customers can retrieve or ingest them in their system. | No | "cloudlogs" |
| `sccwp_service_plan` | Specify the plan type for the Security and Compliance Center (SCC) Workload Protection instance. Valid values are free-trial and graduated-tier only. | No | "free-trial" |
| `sccwp_enable` | Set this flag to true to create an instance of IBM Security and Compliance Center (SCC) Workload Protection. When enabled, it provides tools to discover and prioritize vulnerabilities, monitor for security threats, and enforce configuration, permission, and compliance policies across the full lifecycle of your workloads. To view the data on the dashboard, enable the cspm to create the app configuration and required trusted profile policies.[Learn more](https://cloud.ibm.com/docs/workload-protection?topic=workload-protection-about). | No | False |
| `cspm_enabled` | CSPM (Cloud Security Posture Management) is a set of tools and practices that continuously monitor and secure cloud infrastructure. When enabled, it creates a trusted profile with viewer access to the App Configuration and Enterprise services for the SCC Workload Protection instance. Make sure the required IAM permissions are in place, as missing permissions will cause deployment to fail. If CSPM is disabled, dashboard data will not be available.[Learn more](https://cloud.ibm.com/docs/workload-protection?topic=workload-protection-about). | No | True |
| `app_config_plan` | Specify the IBM service pricing plan for the app configuration. Allowed values are 'basic', 'lite', 'standardv2', 'enterprise'. | No | "basic" |
| `skip_flowlogs_s2s_auth_policy` | Skip auth policy between flow logs service and COS instance, set to true if this policy is already in place on account. | No | False |
| `existing_bastion_instance_name` | Provide the name of the bastion instance. If none given then new bastion will be created. | No | Null |
| `existing_bastion_instance_public_ip` | Provide the public ip address of the bastion instance to establish the remote connection. | No | Null |
| `existing_bastion_security_group_id` | Specify the security group ID for the bastion server. This ID will be added as an allowlist rule on the HPC cluster nodes to facilitate secure SSH connections through the bastion node. By restricting access through a bastion server, this setup enhances security by controlling and monitoring entry points into the cluster environment. Ensure that the specified security group is correctly configured to permit only authorized traffic for secure and efficient management of cluster resources. | No | Null |
| `existing_bastion_ssh_private_key` | Provide the private SSH key (named id_rsa) used during the creation and configuration of the bastion server to securely authenticate and connect to the bastion server. This allows access to internal network resources from a secure entry point. Note: The corresponding public SSH key (named id_rsa.pub) must already be available in the ~/.ssh/authorized_keys file on the bastion host to establish authentication. | No | Null |
| `bms_boot_drive_encryption` | To enable the encryption for the boot drive of the bare metal server. Select true or false. | No | false |
| `enable_sg_validation` | If `enable_sg_validation` is set to true, the deployment confirms that the correct security groups are attached and allows the appropriate rules. When set to false, no validation is performed, and the deployment proceeds without verifying the security groups. | No | true |
| `login_security_group_name` | Provide the security group name to provision the bastion node. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the bastion node to function properly. | No | Null |
| `storage_security_group_name` | Provide the security group name to provision the storage nodes. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the storage nodes to function properly. | No | Null |
| `cluster_security_group_name` | Provide the security group name to provision the compute nodes. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the compute nodes to function properly. | No | Null |
| `client_security_group_name` | Provide the security group name to provision the gklm nodes. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the gklm nodes to function properly. | No | Null |
| `gklm_security_group_name` | Provide the security group name to provision the gklm nodes. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the gklm nodes to function properly. | No | Null |
| `ldap_security_group_name` | Provide the security group name to provision the ldap nodes. If set to null, the solution will automatically create the necessary security group and rules. If you choose to use an existing security group, ensure it has the appropriate rules configured for the ldap nodes to function properly. | No | Null |
| `login_subnet_id` | Provide the ID of an existing subnet to deploy cluster resources, this is used only for provisioning bastion, deployer, and login nodes. If not provided, new subnet will be created.[Learn more](https://cloud.ibm.com/docs/vpc). | No | Null |
| `cluster_subnet_id` | Provide the ID of an existing subnet to deploy cluster resources; this is used only for provisioning VPC file storage shares, management, and compute nodes. If not provided, a new subnet will be created. Ensure that a public gateway is attached to enable VPC API communication. [Learn more](https://cloud.ibm.com/docs/vpc). | No | Null |
| `storage_subnet_id` | Name of an existing subnet for storage nodes. If no value is given, a new subnet will be created. | No | Null |
| `protocol_subnet_id` | Name of an existing subnet for protocol nodes. If no value is given, a new subnet will be created. | No | Null |
| `client_subnet_id` | Name of an existing subnet for protocol nodes. If no value is given, a new subnet will be created. | No | Null |
{: caption="Deployment values" caption-side="top"}
