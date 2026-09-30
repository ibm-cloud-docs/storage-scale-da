---

copyright:
  years: 2026
lastupdated: "2026-09-30"

keywords:

subcollection: storage-scale-da

---



{{site.data.keyword.attribute-definition-list}}
{:external: target="_blank" .external}
{:release-note: data-hd-content-type='release-note'}



# Release notes
{: #storagescale-service-relnotes}

The release notes describes the brief overview of the new features, enhancements, known and fixed issues added to IBM® Storage Scale for the release.
{: shortdesc}

## September 2026
{: #subcollection-sep26}

### 30 September 2026 [New release]{: tag-green}
{: #subcollection-sep3026}
{: release-note}

The following enhancements and updates are included in this release 1.2.0:

IBM Cloud Monitoring for Scale

:   Enabled cloud monitoring for IBM Storage Scale (GPFS) to provide visibility into cluster health, performance, and resource utilization. This enhancement enables proactive monitoring of GPFS metrics and infrastructure, helping identify potential issues and simplify troubleshooting.

Operating System Upgrade (RHEL 9)

:   Upgraded the operating system from Red Hat Enterprise Linux (RHEL) 8 to RHEL 9, providing an updated platform with improved security, performance, and compatibility. The upgrade also ensures compatibility with the latest system packages and dependencies, providing a stable foundation for future enhancements and updates.

Outbound Network Security Enhancements

:   Restricted outbound network access by limiting open ports to only those required for operation. This reduces the network attack surface and strengthens the overall security of the Storage Scale deployment.

## April 2026
{: #subcollection-jan26}

### 14 April 2026 [New release]{: tag-green}
{: #subcollection-jan1626}
{: release-note}

**For this release, the Storage Scale version is 1.1.0**

- In compliance with IBM’s SSH policy, automation disables root user access on all newly provisioned instances. Therefore, customers are required to access instances using the `vpcuser` account as root login is not permitted.

- Updated the GKLM images for the release.

## December 2025
{: #subcollection-dec25}

In this release, IBM Storage Scale deployable architecture is introduced. {{site.data.keyword.scale_full}} enables configuration for compute nodes and storage nodes to build a complete end to end working HPC cluster. For more information, see [Overview of IBM Storage Scale](/docs/storage-scale-da?topic=storage-scale-da-overview-storage-scale).

### 12 December 2025
{: #subcollection-dec1225}
{: release-note}

The following new features are added as part of the release:

* [IBM Storage Scale deployable architecture](/docs/storage-scale-da?topic=storage-scale-da-storage-scale): You can deploy the dedicated Storage Scale cluster for High-Performance Computing (HPC) clusters using IBM Storage Scale as the storage solution. This offering leverages deployable architecture automation to streamline the provisioning and configuration of the cloud resources.

* [SSD Defined Performance (SDP)](/docs/storage-scale-da?topic=storage-scale-da-sdp-intro): The SSD Defined Performance (SDP) is a second-generation IBM Cloud Block Storage profile designed to provide enhanced flexibility in defining performance and capacity characteristics for block volumes. By using the `sdp` profile, you can specify the capacity and the maximum throughput limit.

* [Private Path Network Load Balancer (PPNLB)](/docs/storage-scale-da?topic=storage-scale-da-ppnlb-overview): The IBM Cloud Private Path service enables secure and private connectivity between IBM Cloud services and third-party applications without exposing traffic to the public internet. By leveraging the IBM Cloud backbone, it ensures that all data transmission remains within the trusted IBM network infrastructure, enhancing both security and performance.

* In this release, the storage types "scratch" is renamed to **VSI** and "persistent" to **bare-metal**. This change was introduced to avoid the confusion caused by SDP implementation.
