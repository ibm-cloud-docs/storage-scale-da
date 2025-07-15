---

copyright:
  years: 2025
lastupdated: "2025-07-15"

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

Deploy the {{site.data.keyword.scale_short}} deployable architecture with Storage Scale cluster using either the IBM Cloud console UI, or the IBM Cloud catalog CLI, and then access the deployed environment.

The offering enables the initial {{site.data.keyword.scale_short}}-based Scale cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.scale_short}} configuration or setup should be performed by using {{site.data.keyword.scale_short}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.scale_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Creating the project by using the UI
{: #deploy-project-gui}
{: ui}

You can deploy your {{site.data.keyword.scale_short}} cluster by using the {{site.data.keyword.cloud_notm}} console UI to create an {{site.data.keyword.scale_short}} project.

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. Search for IBM Storage Scale in the search catalog.
3. Click **Add to project**.
4. In the _Create new_ section:
    * Specify a **Name** for your {{site.data.keyword.scale_short}} project.
    * Optionally provide a **Description** to describe the purpose of the project.
    * Specify a **Configuration name** for your {{site.data.keyword.scale_short}} project. The name can be up to 64 characters.
    * Select a **Region** for the location where you want the {{site.data.keyword.scale_short}} project deployed. The region for the LSF project container can be different from the actual region where the cluster is deployed.
    * Select a **Resource group** for where to get resources for your {{site.data.keyword.scale_short}} project.
    * Click **Create** to save and add your project. When created, the project is added to the **Projects** view of the {{site.data.keyword.cloud_notm}} console.

5. In the _Configure_ section of the _Edit configuration_ page, edit the configuration by entering the **Security** and **Configure architecture** input values.
6. You can edit all the required values from **Configure architecture**, toggle the **Advanced** option to view and edit all the optional values.

    Descriptions of the deployment input values are next to each variable in the {{site.data.keyword.cloud_notm}} console.
    {: tip}

    Secure deployment input values might be entered directly or might be referenced from an existing [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-arbitrary-secrets&interface=ui). As a best practice, the more secure option is to use a Secrets Manager to store secured input values.

    * In the **Security** tab, you have two sections:
        * **Authentication**: specify an API key for the {{site.data.keyword.cloud_notm}} account where you want to deploy your {{site.data.keyword.scale_short}} cluster to fulfill the `ibmcloud_api_key` input variable.
        * **Compliance**: configure the {{site.data.keyword.compliance_full}} controls that you want to use to validate the deployable architecture code before the deployment. You can use the architecture defaults or select your own from an existing {{site.data.keyword.compliance_short}} instance. When you deploy the {{site.data.keyword.scale_short}} cluster and create a new {{site.data.keyword.compliance_short}} instance, you set these deployment input variables in the **Optional** tab.

    * In the **Required** tab, specify the deployment values for the mandatory input variables: `ibm_customer_number`, `storage_gui_username`, `storage_gui_password`, `existing_resource_group`, `remote_allowed_ips`, `ssh_keys`, and `zones`.

    For production clusters, work with your business owners or license management team to make sure that your organization has procured enough licenses to deploy the LSF cluster by using {{site.data.keyword.scale_short}}.

    * When you toggle the **Advanced** option tab, you can specify optional deployment values for advanced configuration and for deeper customization of the provisioned elements. Click **Done**.

    For example, to enable the `override` variable, you need to set the value to **true**.

7. Click **Save** to save your configuration options.
8. Click **Validate**.
   {{site.data.keyword.cloud_notm}} projects run a Code Risk Analyzer scan that includes a [supported set of {{site.data.keyword.compliance_short}} rules](/docs/ContinuousDelivery?topic=ContinuousDelivery-cra-cli-plugin#terraform-scc-goals). It checks controls that are part of the {{site.data.keyword.scale_short}} deployment and that {{site.data.keyword.cloud_notm}} projects support. Any extra controls that are not included in the list of supported {{site.data.keyword.compliance_short}} rules are not checked when you validate the configuration.

   Provide a comment to approve the validation and proceed to deployment.

9. Click **Deploy** to proceed with the deployment. Deploying the deployable architecture can take several minutes. You are notified when the deployment is successful. Optionally click **View resources** from the **Summary** tab to see details about the deployed {{site.data.keyword.scale_short}} project. When deployed, you can then access your deployed environment.

To view the created workspace in the Schematics, follow the steps:

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your unique credentials.
2. Go to the **Navigation Menu**.
3. Select **Platform Automation** > **Schematics** > **Terraform**.
4. You can see the list of workspaces created.

## Deploying {{site.data.keyword.scale_short}} by using the CLI
{: #create-project-cli}
{: cli}

Before you get started, make sure that you have completed the prerequisites in [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/storage-scale?topic=storage-scale-setting-up-cli).

The first step using {{site.data.keyword.bpshort}} is to create a workspace with the specific configuration parameters defined in the corresponding Terraform source code.

Use the following CLI command to create a workspace with your `config.json` file. Make sure that the `config.json` file exists in the directory where you run the command.

`ibmcloud schematics workspace new -f hpc_workspace_config.json --github-token GITHUB_TOKEN`

The `--github-token` parameter is optional and only needed if you are using a private GitHub repository. If you are using the [{{site.data.keyword.IBM_notm}} public GitHub repository](https://github.com/IBM/ibm-spectrum-scale-ibm-cloud-schematics){: external}, you do not need to provide it.
{: note}

### Listing available workspaces
{: #list-available-workspaces-cli}
{: cli}

You can list the workspaces in your account by using the following command:

`ibmcloud schematics workspace list`

Example response with workspace details:

```pre
Name                  ID                                              Description           Status         Frozen
spectrum-scale-test   us-east.workspace.hpcc-scale-test.7cbc3f6b      Sample workspace      INACTIVE       False
```
{: screen}

### Retrieving workspace details
{: #retrieve-workspace-details-cli}
{: cli}

You can retrieve the details of an existing workspace, including the values of all input variables, by running the following command:

`ibmcloud schematics workspace get --id WORKSPACE_ID [--output OUTPUT][--json]`

### Updating a workspace
{: #update-workspace-cli}
{: cli}

You can update the details for an existing workspace, such as the workspace name, variables, or source control URL by running the following command:

`ibmcloud schematics workspace update --id WORKSPACE_ID --file FILE_NAME [--github-token GITHUB_TOKEN]`

To provision or modify {{site.data.keyword.cloud_notm}} resources, you can run the command `ibmcloud schematics plan` command. For more information, see the [{{site.data.keyword.bplong_notm}} CLI](/docs/schematics?topic=schematics-schematics-cli-reference) reference.

## Accessing the deployed environment
{: #access-deployed-environment}
{: ui}

Regardless of whether you deployed the {{site.data.keyword.scale_short}} environment by using the {{site.data.keyword.cloud_notm}} console UI or the CLI after you deploy:

* Verify that you have access to the bastion host by using an SSH key.
* Verify that you can log in to all created {{site.data.keyword.scale_short}} instances.
* Verify that you can connect to the {{site.data.keyword.scale_short}} environment by using the following SSH commands:

Run the command to login to the management node:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<bastion_node_IP> lsfadmin@<management_node_IP>
```
{: codeblock}


Run the command for the login node:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<bastion_node_IP> lsfadmin@<login_node_ip>
```
{: codeblock}

For example:

```ssh
ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@150.239.215.145 lsfadmin@10.241.0.4
```
{: codeblock}

If you deployed by using a project, you can copy this SSH command from the {{site.data.keyword.cloud_notm}} console: select **Projects > _project_name_ > Configurations > _project_configuration_name_ > Outputs** tab, and use the copy icon to copy the `ssh_command` value and run it from a command line.
{: tip}
