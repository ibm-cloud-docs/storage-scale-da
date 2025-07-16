---

copyright:
  years: 2025
lastupdated: "2025-07-16"

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

{{site.data.keyword.scale_full}} enables you to quickly deploy high-performance computing (HPC) clusters with powerful storage capabilities. By leveraging open-source, Terraform-based automation, you can easily provision and configure IBM Cloud® resources. In simple steps, you can define the configuration properties and make use of automated deployment to build your own storage-rich clusters in minutes. {{site.data.keyword.scale_full}} supports the configuration of both compute and storage nodes, allowing you to build a complete, end-to-end Storage cluster. The offering uses a bootstrap node where actual provisioning of compute nodes, storage nodes, installation, and configuration of {{site.data.keyword.scale_short}} takes place. The top-level Terraform code deploys the bootstrap node and starts subprocesses to trigger the secondary layer of Terraform code for actual deployment of cluster components.
{: shortdesc}

A deployable architecture is designed with components, modules, and dependencies that work together to enable seamless deployment. This structure allows developers and operations teams to:
* Quickly deploy new features and updates
* Reduce the need for manual intervention
* Maintain consistency across environments

For more information, see [Deployable architecture on IBM Cloud](https://www.ibm.com/think/insights/deployable-architecture-on-ibm-cloud-simplifying-system-deployment) document.

The bootstrap node—also referred as the Ansible Controller Node in the [architecture diagram](/docs/storage-scale-da?topic=storage-scale-da-storage-scale#architecture-diagram), manages the deployment and configuration of both compute and storage cluster resources.

A custom image - `bootstrap_osimage_name` parameter in [Deployment values](/docs/storage-scale-da?topic=storage-scale-da-deployment-values) is provided as part of this solution. This image includes all the automation scripts and required packages necessary for the bootstrap node to function effectively. The bootstrap node plays a critical role throughout the entire lifecycle of the cluster. For example, it is also required for future operational tasks, such as like cleaning up resources.

The bootstrap node should not be deleted while the cluster is still in use.
{: important}

The default VPC instance profile for the bootstrap node is selected based on the performance requirements of the Ansible scripts, which deploy compute and storage cluster resources in parallel. If you choose a smaller VPC instance profile, the deployment time might be longer.

## BYOL license support
{: #license-support}

This offering supports the Bring-Your-Own-License (BYOL) model for deploying {{site.data.keyword.scale_full_notm}} in an Storage cluster on {{site.data.keyword.cloud_notm}}.

* **BYOL deployment** - Ensure that you have sufficient software licenses to deploy the required capacity on the {{site.data.keyword.cloud_notm}} cluster.

* **Evaluation licenses** - If you do not have the license, contact your {{site.data.keyword.cloud_notm}} sales or support team for evaluation licenses.

* **License-free evaluation** - You can test the offering without a license by selecting the evaluation (scratch) storage type.
