---

copyright:
  years: 2025
lastupdated: "2025-09-09"

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
{:beta: .beta}
{:row-headers: .row-headers}
{:step: data-tutorial-type='step'}
{:table: .aria-labeledby="caption"}

# Expanding the IBM Storage Scale cluster
{: #expand-scale-cluster}

This document describes the procedure to expand an existing IBM Storage Scale cluster using Terraform.

Expanding the Scale cluster should be performed with caution. Running incorrect steps or skipped verifications may result in cluster misconfiguration. You must proceed only if you are familiar with the process and its impact.
{: important}

## Prerequisites
{: #prerequisites}

* Access to the deployer node (the node where Terraform is configured for your cluster).
* Sufficient permissions to modify Terraform configuration files and run the Terraform commands.
* Backup of your existing Terraform state and configuration.

Following are the steps required to expand the Scale cluster using Terraform.

## Log in to the Deployer node
{: #login-deployer}
{: step}

Access the deployer node that was originally used to provision the cluster. All cluster expansion operations must be initiated from this node.

`ssh_to_deployer = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@bastion_ip vpcuser@deployer_ip"`

## Update the Terraform variable file
{: #terraform-var-file}
{: step}

1. Run the command: `sudo su -`
    The VPC user do not have the required permissions to make the changes. So you need to switch to `root` and perform the changes.
2. cd /opt/ibm/terraform-ibm-hpc/terraform.tfvars.json
3. vi /opt/ibm/terraform-ibm-hpc/terraform.tfvars.json
4. Update the file with the required changes and save.

## Run the Terraform plan
{: #run-terraform-plan}
{: step}

Generate an execution plan to review the changes Terraform will apply:

`terraform plan`

Ensure only the planned modifications (for example, new nodes or resources) are listed. Verify there are no destructive changes that could impact the existing cluster.

## Apply the changes
{: #apply-changes}
{: step}

If the plan looks correct, apply the changes to expand the cluster using the command `terraform apply`.

Confirm the action when prompted. Terraform will provision the additional resources as specified in your updated configuration.

**Important**

* Expanding the cluster is an at-your-own-risk operation.
* If the steps are not followed correctly, the cluster may be misconfigured or become unstable.
* Contraction or reduction of the cluster is not supported. Attempting to reduce resources may result in complete cluster failure and permanent data loss.
* Always maintain proper backups and validate cluster health after expansion.
