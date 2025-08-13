---

copyright:
  years: 2025
lastupdated: "2025-08-13"

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

The offering enables the initial {{site.data.keyword.scale_short}}-based cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.scale_short}} configuration or setup should be performed by using {{site.data.keyword.scale_short}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.scale_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Creating the project by using the UI
{: #deploy-project-gui}
{: ui}

You can deploy your {{site.data.keyword.scale_short}} cluster by using the {{site.data.keyword.cloud_notm}} console UI to create a project.

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. Search for _IBM Storage Scale_ in the search catalog.
3. Click **Add to project**.
4. In the _Create new_ section:
    * Specify a **Name** for your project.
    * Optionally provide a **Description** to describe the purpose of the project.
    * Specify a **Configuration name** for your project. The name can be up to 64 characters.
    * Select a **Region** for the location where you want the Schematics workspace to be created. The region selected here is not for the Scale cluster deployment but for the schematics workspace to be created.
    * Select a **Resource group** for your Schematics workspace.
    * Click **Create** to save your project. The project is added to the **Projects** view of the {{site.data.keyword.cloud_notm}} console.

5. In the **Configure** section of the Edit configuration page, edit the configuration by entering the **Security** and **Configure architecture** input values.
6. In the **Security** tab, you have two sections:
    * **Authentication**: specify an API key for the {{site.data.keyword.cloud_notm}} account where you want to deploy your {{site.data.keyword.scale_short}} cluster to fulfill the `ibmcloud_api_key` input variable.
    * **Compliance**: configure the {{site.data.keyword.compliance_full}} controls that you want to use to validate the deployable architecture code before the deployment. You can use the architecture defaults or select your own from an existing {{site.data.keyword.compliance_short}} instance. When you deploy the {{site.data.keyword.scale_short}} cluster and create a new {{site.data.keyword.compliance_short}} instance, you set these deployment input variables in the **Optional** tab.
    * In the **Required** tab, specify the deployment values for the mandatory input variables: `ibm_customer_number`, `storage_gui_username`, `storage_gui_password`, `existing_resource_group`, `remote_allowed_ips`, `ssh_keys`, and `zones`.
7. You can edit all the required values from **Configure architecture**. Toggle the **Advanced** option to view and edit all the optional values.

    Click the info icon **(i)** to view the descriptions for the input values of each variable in the {{site.data.keyword.cloud_notm}} console.
    {: tip}

    Secure deployment values might be entered directly or might be referenced from an existing [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). As a best practice, the more secure option is to use a Secrets Manager to store secured input values.

    * When you toggle the **Advanced** option tab, you can specify optional deployment values for advanced configuration and for deeper customization of the provisioned elements. Click **Done**.

    For example, to enable the `scale_encryption_enabled` variable, you need to set the value to **true**.

7. Click **Save** to save your configuration options.
8. Click **Validate**.
   {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a [supported set of {{site.data.keyword.compliance_short}} rules](/docs/ContinuousDelivery?topic=ContinuousDelivery-cra-cli-plugin#terraform-scc-goals). It checks the controls that are part of the {{site.data.keyword.scale_short}} deployment and that {{site.data.keyword.cloud_notm}} projects support. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} rules are not checked when you validate the configuration.
   Once the validation is completed, provide a comment in the pop-up to approve the validation and proceed to deployment.

9. Click **Deploy** to proceed with the deployment. Deploying the deployable architecture can take several minutes. You are notified when the deployment is successful. Optionally click **View resources** from the **Summary** tab to see details about the deployed {{site.data.keyword.scale_short}} project. When deployed, you can then access your deployed environment.

### Schematics workspace
{: #schematics}
{: ui}

Once you deploy the project, in back-end a Schematics workspace is created for the cluster deployment. To view the created workspace follow the steps:

1. Go to the **Navigation Menu**.
2. Select **Platform Automation** > **Schematics** > **Terraform**.
3. You can see the list of workspaces created.

When deployed, you can then access your deployed environment.
For more information on accessing the cluster after the deployment, see [Accessing the deployed environment](/docs/storage-scale-da?topic=storage-scale-da-before-begin-deploy&interface=ui#accessing-cluster)

## Deploying {{site.data.keyword.scale_short}} by using the CLI
{: #create-project-cli}
{: cli}

Before you begin using the {{site.data.keyword.bplong}} CLI to deploy {{site.data.keyword.scale_full_notm}}, review and complete the following prerequisites:

1. Install the [{{site.data.keyword.cloud_notm}} CLI](/docs/cli?topic=cli-install-ibmcloud-cli).
2. Log in to the {{site.data.keyword.cloud_notm}} CLI with your IBMid. If you have multiple accounts, you are prompted to select which account to use. If you do not specify a region with the `-r` flag, you must also select a region.

    ```pre
    ibmcloud login
    ```
    {: pre}

    If your credentials are rejected, you might be using a federated ID. To log in with a federated ID, use the `--sso` flag. For more information, see [Logging in with a federated ID](/docs/account?topic=account-federated_id).
    {: tip}

3. Install and set up the [{{site.data.keyword.bplong_notm}} CLI plug-in](/docs/schematics?topic=schematics-setup-cli#install-schematics-plugin).
4. Make sure to generate your {{site.data.keyword.cloud_notm}} API key. For more information, see [Managing user API keys](/docs/account?topic=account-userapikey).

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
