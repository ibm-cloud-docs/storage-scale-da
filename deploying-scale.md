---

copyright:
  years: 2025
lastupdated: "2025-05-29"

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

{{site.data.keyword.scale_short}} enables all three interfaces: UI, API, and CLI. To use the API and CLI interfaces, the Terraform-based automation code is available in this [public GitHub repository](https://github.com/IBM/ibm-spectrum-scale-ibm-cloud-schematics){: external}.

The offering enables the initial {{site.data.keyword.scale_short}}-based Scale cluster creation. Any updates that are needed post-deployment regarding {{site.data.keyword.scale_short}} configuration or setup should be performed by using {{site.data.keyword.scale_short}} tools and commands. If you use the {{site.data.keyword.bpshort}} interface to make changes to configuration properties and reapply those changes, you can cause disruptions to the running {{site.data.keyword.scale_short}} cluster. Restoring it back to a working state might not be easy.
{: important}

## Creating a workspace by using the UI
{: #create-workspace-ui}
{: ui}

1. Log in to the [{{site.data.keyword.cloud_notm}} catalog](https://cloud.ibm.com/catalog){: external} by using your credentials.
2. In the Software section, select Storage and then select the **{{site.data.keyword.scale_full}}** tile.
3. In the _Configure your workspace_ section:
    * Specify the **Name** for your {{site.data.keyword.bpshort}} workspace.
    * Select a Location.
    * Select a Resource group.
    * Define any **Tags** that you want to associate with the resources provisioned through the offering. The tags can later be used to query the resources in the {{site.data.keyword.cloud_notm}} console.
4. In the _Set the deployment values_ section, specify the values for the required properties.
5. Expand the _Parameters with default values_ section, and review it to determine whether you need to override any of the default values provided for the configuration properties.
6. Review and accept the **{{site.data.keyword.scale_full_notm}}** license terms and conditions in the order summary.
7. Click Install. The {{site.data.keyword.bpshort}} workspace is created with the name specified. You can see the list of workspaces in _View the existing installations_. If the workspace creation is successful, the Apply Plan action is started to trigger the deployment of the respective {{site.data.keyword.vpc_short}} resources in your {{site.data.keyword.cloud_notm}} account.
8. You can also review the status of your deployment process by identifying the workspace name in the _View the existing installations_ section. When you click a record in _View the existing installations_ section, you are taken to the {{site.data.keyword.bpshort}} workspace view. 

## Next steps (UI)
{: #next-steps-create-ui}
{: ui}

After you have successfully created a workspace, you can begin [Generating a plan](/docs/storage-scale?topic=storage-scale-generate-plan&interface=ui) to validate all the configuration properties.

## Before you begin
{: #before-you-begin-creating-cli}
{: cli}

Before you get started, make sure that you have completed the prerequisites in [Setting up the {{site.data.keyword.bplong_notm}} CLI](/docs/storage-scale?topic=storage-scale-setting-up-cli).

## Creating a workspace by using the CLI
{: #create-workspace-cli}
{: cli}

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
Name                  ID                                              Description           Status         Frozen
spectrum-scale-test   us-east.workspace.hpcc-scale-test.7cbc3f6b      Sample workspace      INACTIVE       False
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

## Next steps (CLI)
{: #next-steps-create-cli}
{: cli}

After you have successfully created a workspace, you can begin Generating a plan to validate all of the configuration properties.
