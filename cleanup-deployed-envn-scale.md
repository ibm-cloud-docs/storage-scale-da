---

copyright:
  years: 2025
lastupdated: "2025-07-22"

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

# Cleaning up deployed environments
{: #cleaning-deploy-envn}

If you want to destroy the {{site.data.keyword.scale_short}} cluster and all of its associated VPC resources, you can remove them from your {{site.data.keyword.cloud}} account.

## Destroying resources by using the UI
{: #destroy-resources-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to the navigation menu and select **Platform Automation** > **Schematics** > **Terraform**. Select the resource to be deleted and click **Actions** drop-down and select **Destroy resources** to destroy all the related VPC resources that were deployed as part of that workspace.
2. If you select the option to destroy resources, decide whether you want to destroy all of them. This action cannot be undone.
3. Confirm the action by entering the workspace name in the text box and click **Destroy resources**.

## Deleting a workspace by using the UI
{: #delete-workspace-ui}
{: ui}

1. In the {{site.data.keyword.cloud_notm}} console, go to the navigation menu and select **Platform Automation** > **Schematics** > **Terraform**. Select the workspace and click **Actions** drop-down and select **Delete workspace** to delete the schematics workspace.
2. Confirm the action by entering the workspace name in the text box and click **Delete workspace**.

If you directly delete the workspaces, the resources cannot be deleted through the Schematics and it should be deleted manually.
{: note}

## Deleting a workspace by using the CLI
{: #delete-workspace-cli}
{: cli}

Run the following command to delete your workspace:

```pre
ibmcloud schematics workspace delete --id <WORKSPACE_ID>
```
{: pre}

You can monitor the log files to view the deletion progress of your workspace.
{: note}

## Destroying resources by using the CLI
{: #destroy-resources-cli}
{: cli}

Run the following command to remove your VPC resources from your workspace:

```pre
ibmcloud schematics destroy --id <WORKSPACE_ID>
```
{: pre}

You can monitor the log files to view the deletion progress of all {{site.data.keyword.cloud_notm}} resources.
{: note}
