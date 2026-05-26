---

copyright:
  years: 2025
lastupdated: "2025-12-10"

keywords:
subcollection: storage-scale-da

---

{:external: target="_blank" .external}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:download: .download}
{:important: .important}
{:note: .note}
{:new_window: target="_blank"}

# Deployer node
{: #deployer-node-overview}

In the Storage Scale solution, a deployer node is provisioned by default to facilitate and streamline the entire deployment and management process of the HPC cluster.

In the earlier releases, all the cluster infrastructure, including networking, compute, DNS, monitoring, and security services were provisioned through an IBM Cloud Schematics workspace. In this setup, users interacted with the solution through the Project UI and the back-end depended entirely on the Schematics automation.

Following are the limitations of Schematics:

* **Limited log visibility** - users did not have the access to the Schematics container.
* **Troubleshooting and lifecycle operations** - the cluster expansion required additional manual steps.
* **Longer deployment times** - occurred due to centralized execution outside the user environment.

With the current release, the solution has been enhanced by introducing a dedicated deployer node. This node serves as the central control point for running both Terraform and Ansible operations.

Following are the key benefits of deployable node:

* **Enhanced troubleshooting and visibility:** Direct access to the deployer node allows users to review deployment logs and diagnose issues in real-time.

* **Accelerated provisioning:** Running the deployment workflows within the VPC, significantly reduces the cluster creation time.

* **Post-deployment flexibility:** Users can perform cluster scaling and configuration updates.

* **Code and Playbook access:** All the automation logic, including the Ansible playbooks and Terraform configurations reside on the deployer node. This allows inspection, modification, or reruns as needed.

## Deployment Workflow
{: #workflow-dn}

The deployment process is divided into two stages:

**Base infrastructure provisioning (through Schematics):**

* The Schematics workspace provisions the VPC, bastion host, and the deployer node.

**Cluster provisioning and configuration (through Deployer node):**

* A terraform apply command is used to provision cluster VSI nodes, DNS records, and optional resources.

* Once the infrastructure is in place, Ansible playbooks run the configuration and software installation, completing the cluster deployment.

This hybrid model improves modularity, enhances user control, and ensures a transparent, maintainable, and scalable HPC solution.

**Command to ssh** - `ssh_to_deployer = "ssh -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null -J ubuntu@<prefix-deployer-node> vpcuser@<prefix-bastion-node>"`

## Software deployment and management
{: #scale-sw-mgmt-dn}

All the deployments in this solution use the Scale software (GPFS). The relevant Ansible playbooks tailored for the specific Scale version are stored on the deployer node. This ensures consistency and reliability in the deployment process.

**Additionally:**

* All the required Scale GPFS versions and dependencies are preloaded on the deployer node.
* This setup eliminates the need for external downloads during deployment, speeding up installation, and reducing dependency issues.
* The deployer node acts as the central source for Scale GPFS package distribution across nodes in the cluster.
