---

copyright:
  years: 2025
lastupdated: "2025-12-11"

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

This solution addresses key challenges around security, privacy, and operational complexity by ensuring point-to-point private communication. It supports private connections across different VPCs allowing flexible and scalable deployment architectures.

When private path is integrated with CES for NFS, provides a robust and secure method to deliver file storage to clients within the same VPC offering direct and efficient access to CES (NFS) storage.

For more information, see [Creating a Private Path network load balancer](https://cloud.ibm.com/docs/vpc?topic=vpc-ppnlb-ui-creating-private-path-network-load-balancer&interface=ui).

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
| `protocol_instance_eth1_mtu` |Specifies the Maximum Transmission Unit (MTU) for protocol instance `eth1`. When PPNLB is enabled, the MTU must be 8500 or lower. When disabled, MTU can be up to 9000. | No | 9000 |
{: caption='PPNLB Variables'}

When using PPNLB for CES (NFS/SMB/S3) access, ensure that the client node MTU is also set to 8500 or lower.
{: note}

PPNLB does not support MTU 9000 and any client configured with a higher MTU can cause packet fragmentation, dropped packets, or stalled file operations—especially for large file transfers.
{: important}

If PPNLB is not enabled, protocol nodes and clients can safely use an MTU of 9000 for optimal performance.

## Limitations
{: #limitation}

1. With NFSv3, file locking is not fully supported. During a failover, NFS grace handling requires the client to perform an NFS reconnect to re-establish lock state. PPNLB does not support this reconnect, therefore NFSv3 locking cannot function correctly. As a result, NFSv3 file locking is not supported when using PPNLB.

2. If multiple clients are running on the same hypervisor, at some point the CPU on hypervisor become saturated, resulting in lower performance than expected. In contrast, when the clients are distributed across different hypervisors, resource contention is reduced and overall performance improves.

3. NFSv4.x with nconnect is designed to improve throughput by parallelizing I/O streams between client and server.

4. Each core independently applies its own round-robin logic when choosing the backend NFS gateway for a new connection. This lack of coordination means that multiple connections from the same client can be distributed unevenly across different servers. As a result, you end up with lopsided load balancing and inconsistent NFS session state, especially when using features like nconnect or NFSv4 sessions that require all streams to land on the same server.
