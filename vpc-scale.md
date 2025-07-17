---

copyright:
  years: 2025
lastupdated: "2025-07-17"

keywords: vpc, scale

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

# Virtual Private Clouds (VPCs)
{: #vpc}

You can choose to deploy the Storage Scale solution by creating a new VPC or using an existing VPC and existing subnets.
{: shortdesc}

You can use {{site.data.keyword.vpc_full}} as your VPC. {{site.data.keyword.vpc_short}} supports creating your own space in {{site.data.keyword.cloud}} for a secure, isolated virtual network that combines the security of a private cloud with the availability and scalability of {{site.data.keyword.IBM_notm}}'s public cloud. {{site.data.keyword.vpc_short}} gives your applications logical isolation from other networks, and provides scalability and security. To make this logical isolation possible, the VPC is divided into subnets that use a range of private IP addresses. You can create subnets in suggested prefix ranges, or bring your own public IP address range (BYOIP) to your IBM Cloud account. By default, all resources within the same VPC can communicate with each other over the private network, regardless of their subnet.

For more details about {{site.data.keyword.vpc_short}}, see the [{{site.data.keyword.vpc_short}} documentation](/docs/vpc?topic=vpc-about-vpc).

## Using a new VPC for your Storage Scale cluster
{: #vpc-new}

If you choose to create a new VPC, set the `vpc_name` input value as null during the Storage Scale cluster deployment. With this setting, the deployment automatically creates a brand-new VPC by using the provided address prefix that you provide for the `vpc_cidr` input value. Make sure that you provide a valid address prefix for the `vpc_cidr` input value.

With a new VPC, the cluster deployment automatically isolates the network, and creates three different subnets under the new VPC by using the `vpc_cidr` value:

* It splits the larger CIDR range from in `vpc_cidr`, into three different networks ranges based on number of IP addresses needed under that subnet.

* After the CIDR ranges are passed in the `vpc_cidr`, `client_subnets_cidr`, `protocol_subnets_cidr`, and `storage_subnets_cidr` input values, the cluster deployment automatically creates the VPC and subnets. One subnet range with the same CIDR range is used only for the creation of bastion and login nodes. The other subnets are used to create management nodes or VPC file shares and compute nodes.

## Using an existing VPC for your Storage Scale cluster
{: #vpc-existing}

If you have existing VPC infrastructure, you can use that VPC for your Storage Scale cluster. There are two possible approaches to using an existing VPC:

### An existing VPC with existing subnets
{: #existing-vpc}

If you use your existing VPC for your cluster, set the `vpc_name` value during deployment with the name of your existing VPC. With this setting, the cluster deployment automatically skips creating a new VPC and uses the one you specify and its existing VPC details for all networking.

With an existing VPC, you can also choose to use existing subnets to create Storage Scale cluster nodes. Cluster deployment needs three subnets:

* Provide a larger subnet ID for the `storage_subnets_cidr` deployment input value, as it is used to create all storage nodes.
* Provide another subnet ID for the `client_subnets_cidr` to create the client nodes.
* Provide another subnet ID for the `protocol_subnets_cidr` to create the protocol nodes.

You cannot use existing subnets with new subnets.
{: note}

### An existing VPC and automatically creating three new subnets from the Storage Scale cluster deployment
{: #existing-three-subnets}

If you have an existing VPC but there are no existing subnets to use, then provide the available valid CIDR range for the `vpc_cluster_private_subnets_cidr_blocks` and `vpc_cluster_login_private_subnets_cidr_blocks` deployment input values. The deployment creates three new subnets under your provided existing VPC. When a new VPC is created, subsequent VPC IDs are attached as an allowed network under the DNS zones. Custom resolvers can also resolve all the DNS entries for the traffic that originates from VPC or subnets.

* Provide a smaller CIDR range for `vpc_cluster_login_private_subnets_cidr_blocks` for the creation of bastion and login nodes. Provide a bigger range of CIDR under `vpc_cluster_private_subnets_cidr_blocks` for the creation of compute nodes.

When you provide existing VPC detail, subsequent VPC IDs are attached as an allowed network under the DNS zones. Custom resolvers can also resolve all the DNS entries for the traffic that originates from VPC or subnets.

Provide a valid CIDR range for the creation of the subnets.
{: important}

`vpc_name` is the name of the VPC variable and `cluster subnet id` is the ID of the subnet and not the CRN.
{: note}
