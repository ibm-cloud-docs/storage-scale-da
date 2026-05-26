---

copyright:
  years: 2026
lastupdated: "2026-05-26"

keywords:

subcollection: storage-scale-da

---



{{site.data.keyword.attribute-definition-list}}

# Troubleshooting
{: #troubleshooting-spectrum-scale}

## Why is IBM Cloud Schematics not able to clone the public GitHub repo?
{: #troubleshoot-topic-2}
{: troubleshoot}
{: support}

Schematics unable to clone the public GitHub repository, and you are seeing one of the following error messages:

* `Fatal, could not download repo, Failed to clone git repository, authentication required (or the git url is incorrect). Problems found with the Repository. Please Rectify and Retry`
* `Template error: Failed to clone git repository, authentication required (or the git url is incorrect)`
{: tsSymptoms}

You did not provide the correct GitHub URL, or you provided a GitHub token, which is not required to clone a public repo. A GitHub access token is only required to access a private repo.
{: tsCauses}

Do not provide a GitHub token, and check to see whether the GitHub token was provided in the `github_token` parameter while creating a workspace by using the [public repo](https://github.com/terraform-ibm-modules/terraform-ibm-hpc/blob/main/main.tf){: external}.
{: tsResolve}

## Why is IBM Cloud Schematics not able to create a workspace?
{: #troubleshoot-topic-3}
{: troubleshoot}
{: support}

Schematics is unable to create a workspace, and you are seeing the following error message: `You do not have the required access to create a workspace in any resource groups. You must be assigned the manager role on the Schematics service in at least one resource group. Contact your account administrator for access.`
{: tsSymptoms}

You do not have the required access to create a workspace in any resource groups. You are assigned as the manager role on the Schematics service for the resource group where you want to deploy the cluster resources.
{: tsCauses}

Contact your account administrator and get assigned with the manager role on the Schematics service for the resource group where you want to deploy the cluster resources.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an authorization error?
{: #troubleshoot-topic-4}
{: troubleshoot}
{: support}

Schematics is unable to provision the cluster, and you are seeing the following error message: `Request is not authorized. Check your user permissions and authorizations and try again.`
{: tsSymptoms}

You do not have the required access to get any VPC resources provisioned.
{: tsCauses}

Contact your account administrator and get the required access permissions. For more information, see [Required permissions](/docs/account?topic=account-userroles).
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an error that the provided name is not unique?
{: #troubleshoot-topic-5}
{: troubleshoot}
{: support}

Schematics is unable to provision the cluster, and you are seeing the following example error message:
{: tsSymptoms}

```pre
"code": "validation_unique_failed",
"message": "Provided Name (sample-scale-vpc) is not unique",
"target": {
"name": "name",
"type": "field",
"value": "sample-scale-vpc"
}
```
{: screen}

Resource names must be unique. If a resource exists with the same name, you might get a similar error.
{: tsCauses}

Since the resource names need to be unique, check which resource is generating the error. From the UI and CLI, you can fetch the details of that specific resource. If that resource is owned by you, update the resource name so it is unique. If the resource is owned by another user, navigate to your {{site.data.keyword.bpshort}} workspace and destroy all the resources. Change the cluster prefix name, and provision the cluster again through {{site.data.keyword.bpshort}}.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster while using a custom image?
{: #troubleshoot-topic-6}
{: troubleshoot}
{: support}

While using a custom image, Schematics is unable to provision the cluster, and you are seeing one of the following error messages:

* `The argument "image" is required, but no definition was found.`
* `Unknown variable. There is no variable named "image_id".`
{: tsSymptoms}

The custom image that is used for one of the virtual server instances is not present in the target region and zone or it is not accessible by the account and API key that is used to provision the cluster.
{: tsCauses}

If you are using a custom image for any of your virtual server instances, ensure that the custom image is available in the target region and zone and is accessible by the account and API key that is used to provision the cluster.
{: tsResolve}

## Why error occurs with the resource group when we generate or apply a plan?
{: #troubleshoot-topic-7}
{: troubleshoot}
{: support}

You are receiving the following error when you try to either generate or apply a plan to your workspace: `Apply failed due to "[ERROR] Given Resource Group is not found in the account %!!(MISSING)s()."`
{: tsSymptoms}

Either during generating or applying a plan, Terraform tries to validate if all of the basic input parameters that are provided to configure the resources are available. During the process, Terraform checks whether the name for the resource group is valid and available on {{site.data.keyword.cloud_notm}}.
{: tsCauses}

You need to check whether the provided resource group name is available in the specific account where the deployment will be. You can also check to see whether there are any spaces included in the resource group name and validate if the name is case-sensitive.
{: tsResolve}

## Why error occurs stating that the "image is not found"?
{: #troubleshoot-topic-8}
{: troubleshoot}
{: support}

You are receiving the following error when you try to either generate or apply a plan to your workspace: `Apply failed due to "Error: [ERROR] No image found with name hpcc-scale5232-rhel810-v1."`
{: tsSymptoms}

Either during generating or applying a plan, Terraform tries to validate if the provided image name and its image ID are present in the `image_map.tf` file. If Terraform finds the correct image details, it provisions the instances, but if the correct image details cannot be found, Terraform tries to fetch the image details from {{site.data.keyword.cloud_notm}} through `data_source`.

Even if the provided image is not present in the cloud from that specific region, you still might receive the error.
{: tsCauses}

You need to check whether the provided image name has any spaces and if that image is present in the region where you want to do the deployment.
{: tsResolve}

## Why am I receiving a `cannot_start_capacity` error?
{: #troubleshoot-topic-9}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to "code : cannot_start_capacity : message : Can't start instance because resource capacity is unavailable."`
{: tsSymtoms}

During the apply plan process, Terraform initiates the virtual server instance provisioning process based on the selected deployment values. If there is a resource capacity issue or a quota issue from the region where you are trying to deploy, the resources won't be provisioned as expected.
{: tsCauses}

You need to talk to the administrator of the account to increase the quota for the specific region, or you can try to clean up all of the unwanted resources that are associated with the cloud infrastructure. If you clean up unwanted resources, you might free up space for the deployment to process.
{: tsResolve}

## Why does the cluster fail with an IBM Customer Number error?
{: #troubleshoot-topic-10}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to "ERROR - [CLOUD-DEPLOY] Provided IBM Customer Number is not entitled to use Storage Scale on Cloud. Kindly contact IBM Support Team. Exiting!"`
{: tsSymptoms}

During the apply plan process, the deployer node initiates provisioning the resources for storage and compute cluster creation. During the process, rpm- and gpfs-related packages need to be decrypted through the Bring-Your-Own-License concept. If the {{site.data.keyword.IBM_notm}} Customer Number is valid, the deployment begins. If not, the automation errors out the deployment.
{: tsCauses}

You need to provide a valid {{site.data.keyword.IBM_notm}} Customer Number that is entitled to {{site.data.keyword.scale_short}} without any spaces in the number. If the value that you provided is valid and you still received this error, contact {{site.data.keyword.IBM_notm}} support to clarify about the entitlement.
{: tsResolve}

## Why does the SSH connection fail and I cannot connect to the nodes?
{: #troubleshoot-topic-11}
{: troubleshoot}
{: support}

You are not able to SSH to the nodes from a local system after a successful cluster deployment.
{: tsSymptoms}

After a successful cluster deployment, you will not be able to SSH to the nodes due to the following issues:
1. SSH keys were not properly installed on the virtual server instance during the automation process.
2. The wrong SSH key names were used. For example, SSH key names that are not owned by the individual trying to establish SSH connection were used.
3. The security group does not have the appropriate source range.
{: tsCauses}

You can try the following procedures to help troubleshoot the SSH issue:
1. Ensure that you have the correct IP address to establish an SSH connection. Refresh the UI to fetch the latest IP address details.
2. Check whether the SSH connection works for the bastion host (for example, `ssh ubuntu@bastion-IP-address`). If the connection is successful, then you can troubleshoot the SSH issue for the other nodes.
3. Open the security group of the bastion host and check if TCP port 22 with source range is open from the user system.
4. Use https://ipv4.icanhazip.com/ to fetch the current IP address and validate whether there is a different, updated IP address on the security group of the source address range.
5. Open the security group of the deployer, compute, and storage nodes to see the bastion node security group source details to access SSH to connect to the other nodes.
6. Make sure that the public SSH key that is used in the region matches the `id_rsa.pub` content from the local system. Use the commands `cd .ssh` and `cat id_rsa.pub` to check.
7. Ensure that there are no duplicate `id_rsa.pub` files present in the `.ssh` folder in the local system.
{: tsResolve}

## What is the `subnet_not_in_address_prefix` error or invalid CIDR format error during the VPC and subnet creation?
{: #troubleshoot-topic-12}
{: troubleshoot}
{: support}

You are receiving the following error when you try to apply a plan to your workspace: `Apply failed due to Error: [ERROR] Error while creating subnet. The specified CIDR does not fit in any of the address prefixes in the specified VPC. Make sure the subnet's CIDR is a subset of the CIDR of one of the address prefixes.`
{: tsSymptoms}

During the apply plan process, the workspace tries to create the VPC and subnet with the specified range of the CIDR address prefix from the deployment value. If the address prefix range is out of scope or does not belong to the family of the IP address range of the VPC, then you get an error that the address is not in range.
{: tsCauses}

Validate if the address prefix range that's provided for subnet creation is from the same range of addresses that are used for the VPC. For example, if the VPC address prefix is 10.241.0.0/18, then the subnet should be in the 10.241.x.x range. If a different IP address range is used, then you need to divide the [subnets](https://www.davidc.net/sites/default/subnets/subnets.html?network=10.23.124.128&mask=25&division=11.721){: external} and choose the IP address range that is required for subnet creation.
{: tsResolve}

## Why does the instance provisioning remain in `Starting` state?
{: #troubleshoot-topic-13}
{: troubleshoot}
{: support}

After you apply a plan, the workspace takes a long time to provision the virtual server instance, and you notice in the UI that the virtual server instance remains in `Starting` state.
{: tsSymptoms}

During the apply plan process, Terraform initiates the provisioning process of the virtual server instance in the cloud infrastructure. If there is a capacity issue or any issue from the infrastructure side for that particular region and zone, you might see this issue.
{: tsCauses}

You can try to manually create an instance in the UI with the same image that is used during the automation process to see whether you get the same issue, or you can try a different zone for the deployment. You can also raise a support request for this issue to see whether it's originating from the infrastructure side.
{: tsResolve}

## Why do I get an error that I'm not authorized to view the instance?
{: #troubleshoot-topic-14}
{: troubleshoot}
{: support}

You are receiving the following errors when you try to apply a plan to your workspace:

* `Apply failed due to error fetching keys. The provided token is not authorized to list keys in this account`
* `Error: the provided token is not authorized to view the specified instance (ID:*) in this account`
{: tsSymptoms}

During the apply plan process, the {{site.data.keyword.scale_short}} automation is integrated with trusted profiles, which is used for authorization purposes to provide permissions for the deployer node to create compute resources.
{: tsCauses}

Contact your administrator to provide the required set of permissions on the trusted profile for the deployment to happen. For more support, you can raise a request with {{site.data.keyword.IBM_notm}} support.
{: tsResolve}

## Why does the health of the PERFMON show a failed state?
{: #troubleshoot-topic-15}
{: troubleshoot}
{: support}

After the cluster setup is done and you run the command `mmhealth node show` or `mmhealth cluster show` to validate the health of the node, the PERFMON node is in a failed state.
{: tsSymptoms}

The PERFMON node uses one of the `pmsensors` for the setup, and the `pmsensors` service might not come up as expected.
{: tsCauses}

Run the following commands to see the specific logs to fix the issue:
{: tsResolve}

1. Run the following command to check the status of `pmsensors`:

    ```pre
    systemctl status pmsensors
    ```
    {: pre}

    **Sample Response**

    ```pre
    pmsensors.service - zimon sensor daemon
    Loaded: loaded (/usr/lib/systemd/system/pmsensors.service; enabled; vendor preset: disabled)
    Active: failed (Result: start-limit) since Wed 2022-05-11 11:13:44 UTC; 2h 5min ago
    Main PID: 19206 (code=exited, status=78)
    May 11 11:13:44 test-scale-r4-compute-1 systemd[1]: pmsensors.service failed.
    ```
    {: screen}

2. Run the following command to restart the `pmsensors` service:

    ```pre
    systemctl restart pmsensors
    ```
    {: pre}

The logs can be checked at `cd /var/adm/ras` and `cat mmsysmonitor.log`.

## Why does my cluster deployment fail in Schematics with a `signal KILL` error?
{: #troubleshoot-topic-16}
{: troublshoot}
{: support}

While {{site.data.keyword.bpshort}} sets up the cluster, the deployment fails with an error similar to: `Error: error executing "/tmp/terraform_516879781.sh": Process exited with status 137 from signal KILL`
{: tsSymptoms}

Cluster provisioning is a two-phase process. In the first phase, {{site.data.keyword.bpshort}} deploys some initial resources and the bastion host. In the second phase, {{site.data.keyword.bpshort}} uses SSH for remote access from the bastion host to the deployer node and then starts the subsequent resource provisioning. During the second phase, if any resource provisioning takes too long, {{site.data.keyword.bpshort}} stops the SSH process, which results in the `signal KILL` error. This can happen if bare metal provisioning takes longer than 40 minutes.

Since {{site.data.keyword.bpshort}} is a free service, there is a time limit of one hour on remote execution. If the one hour time limit elapses, {{site.data.keyword.bpshort}} automatically stops the deployment and returns a `signal KILL` error.
{: tsCauses}

The only way to fix this issue is by cleaning up all of the resources from the failed deployment and then trying to do a new deployment.
{: tsResolve}

## Why does the cluster deployment fail with a "unique name" error?
{: #troubleshoot-topic-17}
{: troubleshoot}
{: support}

During cluster provisioning, the deployment fails with the following error: `The provided name is not unique name: <bare_metal_server_name>`.
{: tsSymptoms}

In the cloud environment, all of the resources should be uniquely named. During cluster provisioning, all resource names are stored in a backend database. If a cluster provisioning attempt fails and you try to clean up resources from this failed attempt, it can take a while for the resource names to be cleaned up from the backend database. If during a subsequent reprovisioning attempt the same cluster prefix is used, it's possible that the new resources' names collide with the stale entries from the previous attempt.
{: tsCcauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt.
{: tsResolve}

## Why does the cluster fail with an "API Marketplace" check error?
{: #troublehsoot-topic-18}
{: troubleshoot}
{: support}

During cluster provisioning, the deployment fails with the following errors:
* `ERROR - IBM Marketplace API error: An error occurred processing the request`
* `ERROR - IBM Marketplace API error: Internal Server Error`
{: tsSymptoms}

The {{site.data.keyword.scale_short}} solution is based on a "Bring Your Own License" model. During cluster provisioning, there is a check to ensure that you are entitled to use the {{site.data.keyword.scale_short}} software.

To validate the software entitlement, the automation code uses a Marketplace API URL to check whether the provided IBM Customer Number (ICN) has entitlement to the {{site.data.keyword.scale_short}} software part numbers. It's possible that the Marketplace API service is experiencing a temporary issue and is not able to service the request.
{: tsCauses}

Open an issue with {{site.data.keyword.cloud_notm}} Support. This needs to be reported to the specific API team that provides this capability. Ask the IBM customer support team to pass this issue to the Marketplace API team.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with a passwordless SSH error?
{: #troubleshoot-topic-19}
{: troubleshoot}
{: support}

After the deployer node creates all the resources, the solution triggers the Ansible code to configure the entire Scale configuration on storage bare metal servers. During the Ansible configuration, the following error occurs: `[ERROR] Check passwordless SSH on all scale inventory hosts (1 retries left)`
{: tsSymptoms}

After all the infrastructure-related resources are up and running, the Ansible code tries to perform the Scale configuration through a passwordless SSH method. During this process, on the storage bare metal server, if the SSH service is not in a running state, then Ansible cannot SSH to that specific bare metal storage node and it fails with the error.
{: tsCauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt. If the issue continues to occur, open an issue with {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}

## Why is IBM Cloud Schematics not able to provision the cluster and fails with an `Enabling the Custom resolver` error?
{: #troubleshoot-topic-20}
{: troubleshoot}
{: support}

While {{site.data.keyword.bpshort}} tries to create the VPC resources, it attempts to create a custom resolver, but fails with this error: `[ERROR] Error Enabling the Custom resolver : MaxTimeout`
{: tsSymptoms}

Terraform attempts to create the custom resolver environment and waits for the custom resolver state to reach active state. During this process, if the custom resolver takes more time than expected, Terraform throws the error message.
{: tsCauses}

After a failed deployment, clean up all resources. During a subsequent attempt, use a new cluster prefix to avoid any name collisions with resources from the previous failed attempt. If the issue continues to occur, open an issue with {{site.data.keyword.cloud_notm}} Support.
{: tsResolve}

## Why does the CES nodes within the storage cluster on Bare metal encounter the “nfs_sensors_not_configured” issue?
{: #troubleshoot-topic-21}
{: troubleshoot}
{: support}

When NFS sensor details on Bare metal with CES-enabled cluster does not update properly, then the "nfs_sensors_not_configured" error occurs.
{: tsSymptoms}

Specifically, on the Bare metal with CES-enabled cluster, if the required NFS sensors are not configured properly on CES nodes, then “nfs_sensors_not_configured” issue occurs.
{: tsCauses}

The following steps address the "nfs_sensors_not_configured" issue by configuring NFS sensors as outlined in the documentation provided at https://www.ibm.com/docs/en/storage-scale-system/6.1.9?topic=gui-configure-nfs-sensors:
{: tsResolve}

1. Use the following content to define the NFS sensor configuration:

    ```pre
    content='sensors={
    name = "NFSIO"
    period = 10
    proxyCmd = "/opt/IBM/zimon/GaneshaProxy"
    restrict = "cesNodes"
    type = "Generic"
    }'
    ```
    {: screen}

2. Add the content to the sensor configuration file:

    `echo "$content" > /var/lib/mmfs/gui/tmp/sensorDMP.txt`

3. Add the sensor configuration by using the `mmperfmon` command:

    `mmperfmon config add --sensors /var/lib/mmfs/gui/tmp/sensorDMP.txt`

4. Refresh the NFS health status specifically for CES nodes:

    `/usr/lpp/mmfs/bin/mmhealth node show nfs --refresh -N cesNodes`

## Why do we see the `rkmconf_filenotfound_err` error on the compute nodes?
{: #troubleshoot-topic-22}
{: troubleshoot}
{: support}

During the deployment of a Scale cluster with encryption enabled, the following error message might occur in the compute node:
`rkmconf_filenotfound_err`
{: tsSymptoms}

In the simplified setup, the `mmkeyserv` command manages its own `RKM.conf` file and updates it automatically. But sometimes, if the configuration file does not exist or the content is not valid then `rkmconf_filenotfound_err` error occurs in the compute nodes.
{: tsCauses}

Check whether the `/var/mmfs/etc/RKM.conf` file exists (regular setup only), or the file system encryption is enabled by using the simplified setup. The event can be manually cleared by using the `mmhealth event resolve rkmconf_filenotfound_err` command. For more information, see [Encryption events](https://www.ibm.com/docs/en/storage-scale/5.1.9?topic=events-encryption).
{: tsResolve}

## Why is the GUI component degraded?
{: #troubleshoot-topic-23}
{: troubleshoot}
{: support}

The GUI shows DEGRADED with the following warning message:

```pre
Node Name:                        hpcc-scl002-c16-comp-mgmt-e0b7-001.comp.com
Node Status:                      HEALTHY
Status Change:                    13 min ago
Component           Status                Status Change	                Reasons & Notices
GPFS	            HEALTHY	           13 min ago	                  -
NETWORK	            HEALTHY	           13 hours ago	                -
FILESYSTEM	    HEALTHY	           13 min ago	                       -
ENCRYPTION	    HEALTHY	           9 hours ago	                     -
GUI	            DEGRADED	          26 min ago	              gui_refresh_task_failed
PERFMON	            HEALTHY	          13 min ago	                    -
THRESHOLD	    HEALTHY	          12 hours ago
```
{: tsSymptoms}

The GUI component shows DEGRADED with `gui_refresh_task_failed` warning because the GUI’s background refresh task fails due to temporary load, network issues, or cached data problems.
{: tsCauses}

Run the following command to refresh the GUI health status: `mmhealth node show --refresh`
{: tsResolve}

## Why does the filesystem component on some nodes appear as DEGRADED?
{: #troubleshoot-topic-24}
{: troubleshoot}
{: support}

The node health shows TIPS or DEGRADED status. The filesystem `fs1` is reported as unmounted on the node.
Health check raises `unmounted_fs_check`, marking the filesystem component as **DEGRADED**.

```pre
[root@test-jan-per6-comp-1-2c30-004.comp.com ~]# mmhealth node show

Node name:      test-jan-per6-comp-1-2c30-004.comp.com
Node status:    TIPS
Status Change:  23 min. ago

Component          Status       Status Change       Reasons & Notices
--------------------------------------------------------------------------
GPFS                TIPS            23 min. ago     gpfs_pagepool_small_4g
NETWORK            HEALTHY          24 min. ago             -
FILESYSTEM         DEGRADED         16 min. ago     unmounted_fs_check(fs1)
PERFMON            HEALTHY          22 min. ago             -
THRESHOLD          HEALTHY          22 min. ago             -
```
{: tsSymptoms}

The node was recently incremented or updated in the cluster. During the addition process the GPFS services was running, but the filesystem did not automatically mount on this node. As a result, Storage Scale detected a discrepancy between the expected and actual filesystem mount state and generated a warning.
{: tsCauses}

Perform a GPFS shutdown and restart the affected node:
{: tsResolve}

1. The node health returned to **HEALTHY** state.
2. The filesystem mounted successfully.
3. The `unmounted_fs_check` warning was cleared. The following commands are used:

```text
mmshutdown -N test-jan-per6-comp-1-2c30-004.comp.com
mmstartup -N test-jan-per6-comp-1-2c30-004.comp.com
```
{: codeblock}

As a result, no further health warnings observed. The node is functioning as expected.

## Why does the filesystem component show ill_unbalanced_fs (fs1) warning?
{: #troubleshoot-topic-25}
{: troubleshoot}
{: support}

Run the `mmhealth node show -a` command to check the health status of all nodes in the cluster.
{: tsSymptoms}

The node **test-jan-case1-strg-f5d4-001.strg.com** is in TIPS state due to:
1. `callhome_not_enabled`
2. `ill_unbalanced_fs (fs1)` on the filesystem component.

The node was recently incremented in the cluster. After addition, the filesystem `fs1` became temporarily unbalanced, leading to `ill_unbalanced_fs` warning.
{: tsCauses}

Filesystem restriping was performed on the storage node using the command `mmrestripefs fs1 -b`. After restriping, the imbalance reason was removed from the filesystem health status.
{: tsResolve}

## Why does encryption fail on Cluster Health?
{: #troubleshoot-topic-26}
{: troubleshoot}
{: support}

The encryption fails with the following warning message:

```text
[root@jl-gktest5-strg-bc74-001 vpcuser]# mmhealth node show

Node name:      jl-gktest5-strg-bc74-001.strg.com
Node status:    TIPS
Status Change:  1 hour ago

Component      Status        Status Change     Reasons & Notices
-----------------------------------------------------------------------------------------
GPFS           TIPS          1 hour ago        callhome_not_enabled, gpfs_maxstatcache_low
NETWORK        HEALTHY       1 hour ago        -
FILESYSTEM     HEALTHY       1 hour ago        -
DISK           HEALTHY       1 hour ago        -
CES            HEALTHY       1 hour ago        -
CESCLUSTER     HEALTHY       1 hour ago        -
ENCRYPTION     FAILED        47 min. ago       rkm_no_access(rkm_ClusterTenant1-jl-gktest5-gklm-bc74-002.gklm.com)
PERFMON        HEALTHY       1 hour ago        -
THRESHOLD      HEALTHY       1 hour ago        -
```
{: codeblock}
{: tsSymptoms}

This happens when the GKLM servers are created more than 2. This has no impact on the cluster or the files created on the filesystem.
{: tsCauses}

* The RKM conf is not synchronized between the master and slave.
* This warning message will be recovered after the next replication between the GKLM server.
* Approximately 24 hours after the cluster was created.

To fix the issue, you need to enable replication to synchronize the master node with the slave nodes.
{: tsResolve}

**With GUI:**

1. Access the GKLM server through SSH tunneling, using the following command:
    ```text
    ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -L 9443:localhost:9443 -J ubuntu@BASTION_IP vpcuser@GKLM_MASTER_SERVER
    ```
    {: codeblock}

2. Access the GKLM dashboard through a browser https://localhost:9443
3. Login using admin user credentials.
4. Access the **Administration** dashboard.

    ![IBM Security Guardium Key Lifecycle Manager (GKLM) - Replication](images/GKLM_gui.png "IBM Security Guardium Key Lifecycle Manager (GKLM) - Replication"){: caption="IBM Security Guardium Key Lifecycle Manager (GKLM) - Replication" caption-side="bottom"}{: external download="GKLM_gui.png"}

4. Click **Replicate Now** option to synchronize the master configuration across all slave nodes.

    ![IBM Security Guardium Key Lifecycle Manager (GKLM) - Information](images/GKLM_gui_info.png "IBM Security Guardium Key Lifecycle Manager (GKLM) - Information"){: caption="IBM Security Guardium Key Lifecycle Manager (GKLM) - Information" caption-side="bottom"}{: external download="GKLM_gui_info.png"}

Once the replication is complete, the encryption state returns to healthy.

**With API commands:**

1. Login to any node in the cluster that is accessible to the GKLM servers.
2. Run the below API command to retrieve the auth token:
    ```text
    curl -k -X POST \
      "https://<SCALE_ENCRYPTION_SERVER>:9443/SKLM/rest/v1/ckms/login" \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -d '{
            "userid": "<SCALE_ENCRYPTION_ADMIN_USERNAME>",
            "password": "<SCALE_ENCRYPTION_ADMIN_PASSWORD>"
          }'
    SCALE_ENCRYPTION_SERVER – Master Key Server
    SCALE_ENCRYPTION_ADMIN_USERNAME – SKLAdmin
    SCALE_ENCRYPTION_ADMIN_PASSWORD – Admin Password
    ```
    {: codeblock}

3. With the above command, you will receive an authentication token. Use this token on the below replication command:
    ```text
    curl -k -X POST \
      "https://<SCALE_ENCRYPTION_SERVER>:9443/SKLM/rest/v1/replicate/now" \
      -H "Content-Type: application/json" \
      -H "Accept: application/json" \
      -H "Authorization: SKLMAuth userAuthId=<USER_AUTH_ID>" \
      -d '{
            "hostname": "<ENCRYPTION_SLAVE_SERVER_1 >",
            "port": "2222"
          }'
    SCALE_ENCRYPTION_SERVER – Master Key Server
    USER_AUTH_ID – Auth Token retrieve on first command
    ENCRYPTION_SLAVE_SERVER_1 – Slave Key Server 1
    ```
    {: codeblock}

    Repeat the same command for remaining slave nodes.
    {: note}

4. The final result is as below:
    ```text
    [{"code":"CTGKM2200I","status":"CTGKM2200I Replication successful for ENCRYPTION_SLAVE_SERVER_1:2222"}]
    ```
    {: codeblock}

    After replication the encryption state becomes healthy.

## Why do the Scale commands fail?
{: #troubleshoot-topic-27}
{: troubleshoot}
{: support}

When running Spectrum Scale (GPFS) commands like `mmgetstate -a` after switching to the root user using `sudo su -`, the command prompts for a remote node password and fails to display the cluster state.

**Observed logs:**
{: tsSymptoms}

```text
[vpcuser@test-1-scale-strg-515d-001 ~]$ sudo su -
Last login: Thu Feb  5 17:03:48 UTC 2026 on pts/2
[root@test-1-scale-strg-515d-001 ~]# mmgetstate -a
root@test-1-scale-strg-515d-002.strg.com's password:
mmgetstate: Interrupt received.
```
{: codeblock}

Using `sudo su -` starts a login shell for the root user, which resets the environment variables such as path, profiles, and working directory. As a result, the Spectrum Scale environment is not initialized and the passwordless authentication between cluster nodes breaks. This causes GPFS commands to prompt for remote node credentials and prevents them from completing successfully.
{: tsCauses}

To execute any scale commands, switch to the root user using `sudo su`.
{: tsResolve}

**Observed logs after fix:**

```text
[vpcuser@test-1-scale-strg-515d-001 ~]$ sudo su
[root@test-1-scale-strg-515d-001 vpcuser]# mmgetstate -a

Node number             Node name                   GPFS state
---------------------------------------------------------------
    1       test-1-scale-strg-515d-001              active
    2       test-1-scale-strg-515d-002              active
    3       test-1-scale-strg-tie-515d-001          active
    4       test-1-scale-afm-515d-001               active
    5       test-1-scale-strg-mgmt-515d-001         active
```
{: codeblock}
