---

copyright:
  years: 2025
lastupdated: "2025-05-21"

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
{:beta: .beta}
{:important: .important}

# Overview of IBM Storage Scale
{: #overview-storage-scale}

With {{site.data.keyword.scale_full}}, you can deploy the High-Performance Computing (HPC) clusters by using {{site.data.keyword.scale_full_notm}} as the storage solution. This offering uses open source Terraform-based automation to provision and configure {{site.data.keyword.cloud}} resources. In simple steps, you can define the configuration properties and make use of automated deployment to build your own storage-rich clusters in minutes. {{site.data.keyword.scale_full}} enables configuration of compute nodes and storage nodes to build a complete end to end working HPC cluster. The offering uses a bootstrap node where actual provisioning of compute nodes, storage nodes, installation and configuration of {{site.data.keyword.scale_short}} takes place. The top-level Terraform code deploys the bootstrap node and starts subprocesses to trigger the secondary layer of Terraform code for actual deployment of cluster components.
{: shortdesc}

A deployable architecture involves components, modules, and dependencies that allows for seamless deployment and makes easy for developers and operations teams to quickly deploy new features and updates to the system, without requiring extensive manual intervention. Refer the [Deployable architecture on IBM Cloud](https://www.ibm.com/think/insights/deployable-architecture-on-ibm-cloud-simplifying-system-deployment) document for more detailed information.

The bootstrap node (Ansible Controller Node in the [architecture diagram](/docs/storage-scale-da?topic=storage-scale-da-storage-scale#architecture-diagram)) performs the deployment and configuration of the compute and storage cluster resources. A custom image (see `bootstrap_osimage_name` in [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values)) is provided as part of this solution and it contains all of the automation scripts and packages that are needed for the bootstrap node. The bootstrap node is critical during the entire lifetime of this cluster. For example, you need this node for future actions like cleaning up resources. The bootstrap node should not be deleted until the cluster is no longer required.

The default VPC instance profile for the bootstrap node has been selected based on the performance of the Ansible scripts that are triggered to deploy the compute and storage cluster resources in parallel. If you choose a smaller VPC instance profile, the deployment time might be longer.

## BYOL license support
{: #license-support}

This offering supports the Bring-Your-Own-License (BYOL) model for {{site.data.keyword.scale_full_notm}} to deploy an HPC cluster on {{site.data.keyword.cloud_notm}}. Make sure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster. Contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses. Or, you can also try out the scratch storage capability of the offering on {{site.data.keyword.cloud_notm}} without a license by selecting the evaluation storage type.
