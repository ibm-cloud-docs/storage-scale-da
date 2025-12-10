---

copyright:
  years: 2025
lastupdated: "2025-12-10"

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

# Private Path Network Load Balancer (PPNLB)
{: #ppnlb-overview}

The IBM Cloud Private Path service enables secure and private connectivity between IBM Cloud services and third-party applications without exposing traffic to the public internet. By leveraging the IBM Cloud backbone, it ensures that all data transmission remains within the trusted IBM network infrastructure, enhancing both security and performance.

The private path requires the below key components:

* A **Private Path Network Load Balancer (PPNLB)** to host and expose services privately within the IBM Cloud.

* A **Virtual Private Endpoint (VPE)** gateway that allows clients to access the service through private connectivity.

This solution addresses key challenges around security, privacy, and operational complexity by ensuring point-to-point private communication. It supports private connections across different VPCs or even between multiple IBM Cloud accounts, allowing flexible and scalable deployment architectures.

When private path is integrated with IBM Spectrum Scale CES for NFS, provides a robust and secure method to deliver file storage to clients within the same VPC offering direct and efficient access to CES (NFS) storage.

## Key features
{: #ppnlb-key-features}

Following are the key features of PPNLB:

* Enhanced security with no internet exposure.

* Private and point-to-point connectivity across IBM Cloud infrastructure.

* Support for third-party and IBM Cloud services over private connections.

* Simplified network management with consistent privacy and performance.

To enable the PPNLB feature on a Scale cluster, the following variables need to be defined:

| Value | Description | Is it required? | Default value |
| ----- | ----------- | --------------- | ------------ |
| `enable_private_path_nlb` | Enable private path network load balancer for providing CES (NFS) storage. | No | False |
| `ibm_account_id` | Specified the IBM Cloud account ID for PPNLB. | No | "" |
| `protocol_instance_eth1_mtu` |Specifies the Maximum Transmission Unit (MTU) for protocol instance `eth1`. When PPNLB is enabled, the MTU must be 8500 or lower. When disabled, MTU can be up to 9000. | No | 9000 |
{: caption='PPNLB Variables'}

When using PPNLB for CES (NFS/SMB/S3) access, ensure that the client node MTU is also set to 8500 or lower.
{: note}

PPNLB does not support MTU 9000 and any client configured with a higher MTU can cause packet fragmentation, dropped packets, or stalled file operations—especially for large file transfers.
{: important}

To maintain consistent end-to-end connectivity, the:
* Protocol node `eth1` MTU must be ≤ 8500, and
* All client nodes accessing CES through PPNLB must also use an MTU ≤ 8500.

If PPNLB is not enabled, protocol nodes and clients can safely use an MTU of 9000 for optimal performance.
