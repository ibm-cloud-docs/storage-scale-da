---

copyright:
  years: 2026
lastupdated: "2026-07-20"

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
{:ui: .ph data-hd-interface='ui'}
{:cli: .ph data-hd-interface='cli'}
{:api: .ph data-hd-interface='api'}
{:table: .aria-labeledby="caption"}

# Deploying IBM Storage Scale
{: #deploying-storage-scale}

Deploy the Storage Scale deployable architecture using the IBM Cloud console.
{: ui}

Deploy the Storage Scale deployable architecture using the IBM Cloud CLI.
{: cli}

The offering enables the initial {{site.data.keyword.scale_short}}-based cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.scale_short}} configuration or setup must be performed by using {{site.data.keyword.scale_short}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.scale_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Creating the project by using the UI
{: #deploy-project-gui}
{: ui}

You can deploy your {{site.data.keyword.scale_short}} cluster by using the {{site.data.keyword.cloud_notm}} console UI to create a project.

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. Search for _IBM Storage Scale_ in the search catalog.
3. Click **Create project**.
4. In _Create a project_ section:
    * Specify a **Name** for your project.
    * Optionally provide a **Description** to describe the purpose of the project.
    * Select a **Region** for the location where you want the Schematics workspace to be created. The region selected here is not for the Scale cluster deployment but for the schematics workspace to be created.
    * Select a **Resource group** for your Schematics workspace.
    * Click **Create** to save your project. The project is added to the **Projects** view of the {{site.data.keyword.cloud_notm}} console.

5. In the **Configure and deploy** section, edit the configuration by entering the **Security** and **Inputs** details.
6. In the **Security** tab, you have two sections:

    * **Authentication**: specify an API key for the {{site.data.keyword.cloud_notm}} account where you want to deploy your {{site.data.keyword.scale_short}} cluster to fulfill the `ibmcloud_api_key` input variable.
    * **Compliance**: configure the {{site.data.keyword.compliance_full}} controls that you want to use to validate the deployable architecture code before the deployment. You can use the architecture defaults or select your own from an existing {{site.data.keyword.compliance_short}} instance.

7. In the **Inputs** tab, you can:

    * Edit the **required** deployment values for the mandatory input variables: `storage_gui_username`, `storage_gui_password`, `existing_resource_group`, `remote_allowed_ips`, `ssh_keys`, and `zones`.
    * Toggle the **Optional values**, to add the optional values.
8.  Click **Done**.

    Click the information icon **(i)** to view the descriptions for the input values of each variable in the {{site.data.keyword.cloud_notm}} console.
    {: tip}

    Secure deployment values might be entered directly or might be referenced from an existing [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). As a best practice, the more secure option is to use a Secrets Manager to store secured input values.

    For example, to enable the `scale_encryption_enabled` variable, you need to set the value to **true**.

9. Click **Save** to save your configuration options.
10. Click **Validate**.
   {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a [supported set of {{site.data.keyword.compliance_short}} rules](/docs/ContinuousDelivery?topic=ContinuousDelivery-cra-cli-plugin#terraform-scc-goals). It checks the controls that are part of the {{site.data.keyword.scale_short}} deployment and that {{site.data.keyword.cloud_notm}} projects support. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} rules are not checked when you validate the configuration.
   Once the validation is completed, provide a comment in the pop-up to approve the validation and proceed to deployment.

11. Click **Deploy** to proceed with the deployment. Deploying the deployable architecture can take several minutes. You are notified when the deployment is successful. Optionally click **View resources** from the **Summary** tab to see details about the deployed {{site.data.keyword.scale_short}} project. When deployed, you can then access your deployed environment.

## Schematics workspace
{: #schematics}
{: ui}

Once you deploy the project, in the back end a Schematics workspace is created for the cluster deployment. To view the created workspace follow the steps:

1. Go to the **Navigation Menu**.
2. Select **Platform Automation** > **Schematics** > **Terraform**.
3. You can see the list of workspaces created.

When deployed, you can then access your deployed environment.
For more information on accessing the cluster after the deployment, see [Accessing the deployed environment](/docs/storage-scale-da?topic=storage-scale-da-before-begin-deploy&interface=ui#accessing-cluster).

You can retrieve the logs from either the Schematics workspace or the deployer node to view both successful and failed cluster deployments.
{: shortdesc}

### Retrieving apply plan logs in the Schematics workspace
{: #retrieve-apply-plan-logs-schematics-workspace}

After you apply a plan, a new log file is generated, which can be viewed in the _Jobs_ tab in the Schematics workspace. In the following sections you can see the instructions and examples of successful or failed deployments.

### Successful cluster deployment
{: #successful-apply-plan}

1. In the _Jobs_ tab in the Schematics workspace, select the job and expand the log file.
2. If the job was successful in creating all of the resources that are part of the deployment, then your workspace goes to an active state.
3. Use the SSH command in the output of your apply plan log to log in as `vpcuser` for the deployer, compute, and storage node through the bastion host as `ubuntu` user. See the following sample response of a successful deployment:

**Sample response**:

```pre
2022/05/09 14:35:53 Terraform apply | Apply complete! Resources: 41 added, 0 changed, 0 destroyed.
2022/05/09 14:35:53 Terraform apply |
2022/05/09 14:35:53 Terraform apply | Outputs:
2022/05/09 14:35:53 Terraform apply |
2022/05/09 14:35:53 Terraform apply | ssh_command = "ssh -J ubuntu@141.125.161.0 vpcuser@10.241.1.5"
2022/05/09 14:35:53 Command finished successfully.
```
{: screen}

### Failed cluster deployment
{: #failed-apply-plan}

1. In the _Jobs_ tab in the Schematics workspace, select the job and expand the log file for a better view.
2. If the job fails to create any of the resources that are a part of the deployment, then your workspace goes to a failed state. Deployment might error out if any of the deployment values are incorrect or if there are any issues at the infrastructure level.
3. Fix the errors, and then click Apply plan again. See the following sample response of a failed deployment:

**Sample response**:

```pre
2022/05/09 12:51:12 Terraform plan | Error: [ERROR] No SSH Key found with name ssh-key-east-new
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | on main.tf line 85, in data "ibm_is_ssh_key" "compute_ssh_key":
2022/05/09 12:51:12 Terraform plan | 85: data "ibm_is_ssh_key" "compute_ssh_key" { # This block is trying to fetch the key_pair details for compute node, which is dependent on the code present on public repo
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | Error: [ERROR] No SSH Key found with name ssh-key-east-new
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan | on main.tf line 89, in data "ibm_is_ssh_key" "storage_ssh_key":
2022/05/09 12:51:12 Terraform plan | 89: data "ibm_is_ssh_key" "storage_ssh_key" { # This block is trying to fetch the key_pair details for compute node, which is dependent on the code present on public repo
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 Terraform plan |
2022/05/09 12:51:12 �[1m�[31mTerraform PLAN error: Terraform PLAN errorexit status 1�[39m�[0m
2022/05/09 12:51:12 �[1m�[31mCould not execute job: Error : Terraform PLAN errorexit status 1�[39m�[0m
```
{: screen}

## Deploying Storage Scale cluster using the CLI
{: #create-project-cli}
{: cli}

To generate the API key, see [Managing user API keys](/docs/account?topic=account-userapikey&interface=cli).
To login to the IBM Cloud CLI, see [ibmcloud login](/docs/cli?topic=cli-ibmcloud_cli#ibmcloud_login).
{: note}

You can deploy your Storage Scale cluster by using the {{site.data.keyword.cloud_notm}} CLI to create a catalog workspace with the supported Storage Scale cluster version. The CLI requires a `values.json` file with your configuration settings.

1. Install the [{{site.data.keyword.cloud_notm}} CLI and the catalogs management plug-in](/docs/cli?topic=cli-manage-catalogs-plugin) before you run any CLI commands.

2. The CLI requires a `values.json` file with your configuration settings. Use the [values.json](https://github.com/terraform-ibm-modules/terraform-ibm-hpc/blob/main/samples/configs/scale_catalog_values.json) file as a reference. You can copy the contents, change the values to meet your own deployment configurations, and then save it as `values.json`.

3. Run this command in the {{site.data.keyword.cloud_notm}} CLI to deploy your Storage Scale cluster with the configuration you specified in your `values.json` file.

    ```text
    ibmcloud catalog install --vl <version_locator_value> --override-values values.json
    ```
    {: codeblock}

    **Outputs:**

    ```text
    Attempting install of Storage Scale cluster version x.x.x...
    Schematics workspace: https://cloud.ibm.com/schematics/workspaces/us-south.workspace.globalcatalog-collection.40b1c1e4/jobs?region=
    Workspace status: DRAFT
    Workspace status: INACTIVE
    Workspace status: INPROGRESS
    Workspace status: ACTIVE
    Installation successful
    OK
    ```
    {: codeblock}

4. The CLI requires a `version_locator_value`. You can retrieve this value from the {{site.data.keyword.cloud_notm}} console UI by clicking **View details**.

    1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
    2. Click **Review deployment options**.
    3. In the _Deployment options_ section, select **Create from the CLI**, copy the `version_locator_value`, and save this value to be used in a later step. The value is an 80 character alphanumeric string, such as:

        ```text
        1082e7d2-5e2f-0a11-a3bc-f88a8e1931fc.6c26cd4c-4f72-45e5-8bde-77387aa05138-global
        ```
        {: codeblock}

    * Provide the mandatory deployment values for your Storage Scale cluster, specifically, replace the **Fill the value here** text with values.

If you deployed by using a project, you can copy this SSH command from the {{site.data.keyword.cloud_notm}} console: select **Projects > _project_name_ > Configurations > _project_configuration_name_ > Outputs** tab, and use the copy icon to copy the `ssh_command` value and run it from a command line.
{: tip}

### Schematics actions
{: #schematics}

1. You can view the logs in your workspace by running the command `ibmcloud schematics logs --id us-east.workspace.hpcc-cluster.7cbc3f6b`

2. You can list the workspaces in your account by running the command `$ ibmcloud schematics workspace list`

Example response with workspace details:

```pre
Name               ID                                            Description   Status     Frozen
hpcc-cluster       us-east.workspace.hpcc-cluster.7cbc3f6b       INACTIVE      False
OK
```
{: screen}

For more information, see the [IBM Cloud Schematics CLI reference](/docs/schematics?topic=schematics-schematics-cli-reference).

If you deployed by using a project, you can copy this SSH command from the {{site.data.keyword.cloud_notm}} console: select **Projects > _project_name_ > Configurations > _project_configuration_name_ > Outputs** tab, and use the copy icon to copy the `ssh_command` value and run it from a command line.
{: tip}
