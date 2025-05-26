---

copyright:
  years: 2025
lastupdated: "2025-05-26"

keywords: # Not typically populated

subcollection: storage-scale-da

authors:
  - name: Piyush Chaudhary

deployment-url: url

docs: https://cloud.ibm.com/docs/solution-guide

image_source:

use-case: IBM Storage Scale
industry: Electronics, Healthcare, LifeSciences, Automotive, AerospaceAndDefense
compliance:
content-type: reference-architecture

production: false

---
{{site.data.keyword.attribute-definition-list}}

# IBM Storage Scale
{: #storage-scale}
{: toc-content-type="reference-architecture"}
{: toc-industry="Electronics, Healthcare, LifeSciences, Automotive, AerospaceAndDefense"}
{: toc-use-case="StorageScale"}

With IBM® Storage Scale, you can deploy the High-Performance Computing (HPC) clusters by using IBM Storage Scale as the storage solution. This offering leverages open-source, Terraform-based automation to streamline the provisioning and configuration of the cloud resources. In simple steps, you can define the configuration properties and make use of automated deployment to build your own storage-rich clusters in minutes. {{site.data.keyword.scale_full}} supports the configuration of both compute and storage nodes, allowing you to build a complete, end-to-end HPC cluster.

## Architecture diagram
{: #architecture-diagram}

![Architecture diagram.](images/scale-arch-diagram-da.svg "Storage Scale Architecture diagram"){: caption="Storage Scale Architecture diagram" caption-side="bottom"}{: external download="scale-arch-diagram-da.svg"}

## Design concepts
{: #design-concepts}

The architecture framework design covers design considerations and architecture decisions for the following aspects and domains:

* **Data:** Data storage
* **Compute:** Virtual servers
* **Storage:** Primary storage
* **Networking:** Isolation and domain name service
* **Security:** Data security
* **Service management:** Logging and automated deployment

Add the Architecture design scope.

## Requirements
{: #requirements}

The following table outlines the requirements that are addressed in this architecture.

| Aspect | Requirements |
| -------------- | -------------- |
| Data            | Provide a location to store {{site.data.keyword.scale_full_notm}} configuration and data. |
| Compute            | Provide properly isolated compute resources with adequate compute capacity for the applications. |
| Storage            | Provide storage that meets the application and database performance requirements. |
| Networking         | * Deploy workloads in an isolated environment and enforce information flow policies. \n * Distribute incoming application requests across available compute resources. \n * Support failover of application within the cluster event of planned or unplanned node outage. \n * Provide private DNS resolution to support the use of hostnames instead of IP addresses. |
| Security           | * Ensure that all operator actions are run securely through bastion host. \n * Provide users with the ability to use keys to ensure that all data meets regulatory compliance requirements for more security and user control. \n * Protect secrets through their entire lifecycle and secure them using access control measures.|
| Service Management | * Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. \n * Monitor audit logs to track changes and detect potential security problems. |
{: caption="Requirements" caption-side="bottom"}

## Components
{: #components}

| Aspects | Requirement | Architecture component | How the component is used |
|-------------|-------------|-----------|--------------------|
| Data and Storage | Create file shares | Scale Storage nodes, Protocol nodes| Creates file shares for configuring user file data sharing. |
| Compute | Provide infrastructure and administration access | VPC service | Provides a VPC service so that you can log in and submit an job. |
|  | Create virtual server instances to support bastion. | Scale Compute nodes |  |
|  | Create virtual server instances to support management. | Scale Client nodes |  |
| Networking | Bastion, Deployer, GKLM, LDAP | Security group rules for each subnet | As an alternative, more CIDR or ports can be manually added after deployment. |
|  | Enable floating IP on bastion node for user access. | Floating IP on the bastion node | Allows user access to the HPC VPC. |
|  | Enable a public gateway for the HPC management subnet. | Public gateway for management subnet | Allows outbound communication for the Scale management node for any internet access (for example, repositories, packages, and so on). |
|  | DNS service for the HPC compute nodes | DNS service | Helps with the IP and name resolution for the HPC compute nodes. |
|  | (Optional) Load VPN configuration to simplify VPN setup. | VPN | VPN configuration is the responsibility of the user. |
| Security | Create virtual server instances to support management. | Bastion node | Configures security group rules to allow access to {{site.data.keyword.cloud_notm}} services. |
|  |  | Deployer node| |
|  |  | GKLM node| |
|  |  | LDAP node| |
|  | Create virtual server instances that run Storage Scale as a distributed batch HPC application for HPC workload (jobs). | Scale management node | Configures security group rules to allow access to {{site.data.keyword.cloud_notm}} services. |
|  | Provide users with the ability to use keys to ensure that all data meets regulatory compliance requirements for more security and user control. | [{{site.data.keyword.keymanagementservicefull}}](/docs/key-protect) | Provides the ability to use keys to ensure that all data meets regulatory compliance requirements for more security and user control. |
|  | Protect secrets through their entire lifecycle and secure them using access control measures. | [{{site.data.keyword.cloud}} Secrets Manager](/docs/secrets-manager?topic=secrets-manager-getting-started&interface=ui) | Protects secrets through their entire lifecycle and secure them using access control measures.
| Service Management | Schedule and run distributed batch HPC applications. | [IBM Storage Scale](https://www.ibm.com/docs/en/storage-scale/5.2.3){: external} cluster includes:  | {{site.data.keyword.scale_full}} enables configuration of compute nodes and storage nodes to build a complete end to end working HPC cluster. The offering uses a bootstrap node where actual provisioning of compute nodes, storage nodes, installation and configuration of {{site.data.keyword.scale_short}} takes place. |
|  | (Optional) Monitor system and application health metrics and logs to detect issues that might impact the availability of the application. | [{{site.data.keyword.monitoringfull_notm}}](/docs/monitoring?topic=monitoring-getting-started) | Monitors system and application health to detect issues that might impact the availability of the application. |
|  | (Optional) Monitor audit logs to track changes and detect potential security problems. | [{{site.data.keyword.atracker_full}}](/docs/atracker?topic=atracker-getting-started) | Monitors audit logs to track changes and detect potential security problems. |
{: caption="Components" caption-side="bottom"}
